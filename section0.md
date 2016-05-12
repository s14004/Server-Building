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
     `$dpkg -i vagrant_1.7.2_x86_64.deb`
4. インストール完了



# lesson
0: [講義の前のセットアップ](section0.md)  
1: [基本のサーバー構築](section1.md)  
2: [その他のWebサーバー環境](section2.md)  
3: [Ansibleによる自動化とテスト](section3.md)  
4: [PHP以外の環境を構築する](section4.md)  
5: [DNSサーバーを動作させる](section5.md)  
6: [AWS](section6.md)  
[section3_ansible](section3_ansible)  
[section6_ansible](section6_ansible)  

