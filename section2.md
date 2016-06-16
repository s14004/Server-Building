## 2-2 Vagrant設定

  1 Boxの追加  
   `$vagrant box add CentOS7 コピーしたboxファイル --force`  
  2 Vagrantの初期設定  
   `$vagrant init CentOS7`  
  3 ホストオンリーアダプターの設定  
    `$vi Vagrantfile`を開いて  

    Vagrant.configure(2) do |config|    
    config.vm.network :private_network, ip:"192.168.56.129"
    end

  ↑のように追加  
  4 Vagrantの起動  
   `$vagrant up`  
  5 Vagrantへ接続  
   `$vagrant ssh`  

## centosプロキシの設定  
 1 プロキシを設定する  
   `$vi /etc/profile` ファイルに下記を追記する  

     PROXY='172.16.40.1:8888'  
     export http_proxy=$PROXY  
     export HTTP_PROXY=$PROXY  
     export HTTPS_PROXY=$PROXY  
     export https_proxy=$PROXY  

  2 yumにプロキシを設定  
   `$vi /etc/yum.conf` ファイルに下記を追記  
   `proxy=http://172.16.40.1:8888/`  
  3 wgetにプロキシを設定  
   `$vi /etc/wgetrc` ファイルに下記を追記  

    http_proxy = 172.16.40.1:8888/  
    https_proxy = 172.16.40.1:8888/  
    ftp_proxy = 172.16.40.1:8888/  

  4 プロキシの設定が終わったのでupdateする  
   `$yum update`  

## 2−2 Wordpressに必要なものをインストール
  1 PHPのインストール  
  `$sudo yum install --enablerepo=epel,remi-php70 php php-mbstring php-pear php-fpm php-mcrypt php-mysql`
  2 mariadbのインストール  
   mariadbのインストール  
   `$sudo yum -y install mariadb mariadb-server`  

  4 nginxのインストール
    `$ sudo yum -y install http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
    `$yum -y install epel-release
	`$yum install --enablerepo=nginx nginx`

    インストール  
    `$yum -y install nginx`  

  5 nginxの実行と登録   

    $systemctl start nginx  
    $systemctl enable nginx  

  6 php-fpmの実行と登録  

    $systemctl start php-fpm  
    $systemctl enable php-fpm  

  7 ngixの設定でphp-fpmが動くようにする  
   `$vi /etc/nginx/conf.d/default.conf`  
   下記のように編集する  

      location / {  
      root   /usr/share/nginx/html;  
      index  index.html index.htm;  
      }  
 	↓  
      location / {  
      root   /usr/share/nginx/html;  
      index  index.php index.html index.htm;  
      }  


     location ~ \.php$ {  
      root           html;  
      fastcgi_pass   127.0.0.1:9000;  
      fastcgi_index  index.php;  
      fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;  
      include        fastcgi_params;  
      }  
    ↓  
     location ~ \.php$ {  
        root           /usr/share/nginx/html;  
        fastcgi_pass   127.0.0.1:9000;  
        fastcgi_index  index.php;  
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;  
        include        fastcgi_params;  
      }  
    

## 2-2 Wordpressで使うデータベースの作成
  1 Mysqlの起動  
   `$systemctl start mariadb`  
  2 MySQLを起動する  
   `$mysql -u root`  
  3 rootユーザーにパスワードを設定する  
   `mysql> SET PASSWORD FOR 'root'@'localhost' = PASSWORD('xxxxxxxxxx');`  
  4 1回終了してrootでログイン  
   `mysql>exit`  
   `$mysql -u root -p`  
  5 wordpressで使うデータベースを作成する  
   `mysql> CREATE DATABASE データベース名;`  
  6 ユーザの作成  
   `mysql> GRANT ALL PRIVILEGES ON データベース名.* to 'ユーザー名'@'localhost' IDENTIFIED BY '任意パスワード';`  

## 2-2 Wordpressのインストール
  1 Wordpressのzipファイルをインストールする  
   `$wget http://wordpress.org/latest.tar.gz `  
  2 インストールしたzipファイルを解凍する  
   `$tar xzfv latest-ja.tar.gz`  
  3 所有者とグループ変更
   `$ chown -R nginx:nginx wordpress`
  3 解凍したファイルを/usr/shard/ngix/htmlに移動する  
   `$mv wordpress /usr/shard/ngix/html`  
  4 wordpressのフォルダに移動  
   `$cd /usr/shard/ngix/html/wordpress`  
  5 wordpressフォルダの`wp-config-sample.php`をコピーして`wp-config.php`を作成する  
   `$cp wp-config-sample.php wp-confing.php`  
  6 コピーしたwp-config.phpの中を編集  
   `$vi wp-config.php`  

     WordPress のためのデータベース名  
     define('DB_NAME', 'database_name_here');  
    ↓
     define('DB_NAME', 'mysqlで作ったデータベース名');  
     MySQL データベースのユーザー名  
     define('DB_USER', 'username_here');  
    ↓
     define('DB_USER', 'mysqlで作ったデータベースの所有者名');  
     MySQL データベースのパスワード  
     define('DB_PASSWORD', 'password_here');  
    ↓
     define('DB_PASSWORD', 'mysqlで作ったデータベースの所有者名のパスワード');  

  7 ブラウザからWordpressにアクセス   
   `192.168.56.129/wordpress/wp-admin/install.php`  
