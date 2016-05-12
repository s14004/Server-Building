# section 0

##VirtualBoxインストール

1. リポジトリを追加する  
    echo "deb http://download.virtualbox.org/virtualbox/debian xenial contrib" | sudo tee /etc/apt/sources.list.d/oracle_vbox.list
2. キーを追加  
wget -q http://download.virtualbox.org/virtualbox/debian/oracle_vbox.asc -O- | sudo apt-key add -
3. インストール  
    sudo apt-get update  
	sudo apt-get install virtualbox-5.0
4. インストール完了

#Vagrantのインストール
1. [Vagrant公式サイト](https://www.vagrantup.com/)のDOWNLOADSページに移動
2. LINUX(DEB)の64bitをダウンロードする
3. ダウンロードしたファイルを端末で実行  
     `$dpkg -i vagrant_1.8.1_x86_64.deb`
4. インストール完了

