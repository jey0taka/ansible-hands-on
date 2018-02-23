# Ansibleハンズオン



## これは何？

- 社内で実施したAnsibleハンズオンの資料です。

- このAnsibleを実行すると、サーバー内に

  - Apache2.4
  - PHP7.1
  - MySQL5.6
  - Wordpress

  がインストールされます。



## 前もって用意いただくもの

- SSHおよびAnsibleが実行可能なMac か Windows
  - 基本Mac前提です。
  - Windowsの方は、Windowsに詳しい人に聞いて！
  - ChromeBookの猛者がんばれ！！
- SSHが可能なEC2インスタンス（AmazonLinux V1）2台
  - Wordpressの動作確認のため、TCP:80番の解放を推奨



### リポジトリのClone

- 適当なディレクト

```
$ git clone https://github.com/jey0taka/ansible-hands-on.git
```



### 環境の準備

### Mac環境でのセットアップ

#### パターン1

- Homebrew のインストール
```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
- Anbileのインストール
```
$ brew install ansible
```

##### パターン2

※ 要 Command Line tools（たぶん）
```
$ sudo pip install ansible
```



### Windows環境

- がんばれ。




## ハンズオン準備

### ssh_config

```
Host web01.amazonlinux
  HostName XXX.XXX.XXX.XXX
  User ec2-user
  IdentityFile /PATH/TO/YOUREKEY.pem
```

- HostName : EC2のIPアドレスを記載
- IdentityFile : SSHキーへのパス



### group_varsの設定

- 2サイト分の以下の項目を設定します。
  - ドメイン名（サーバーのIPアドレス）
  - DB名
  - DBユーザー名
  - DBパスワード
  - サイト名
  - サイト管理者名
  - サイトパスワード
  - メールアドレス



####  hosts/dev/group_vars/app.yml

```
vhost_domain:           "web01.local"　
vhost_docroot:          "/var/www/vhosts/"　

db_user: wp01user
db_password: wp01password
db_name: wpdb01

site_name: site01
admin_name: site01-admin
admin_pass: pass01
admin_email: site01@hogehoge.com
```



####  hosts/dev/group_vars/web.yml

```
vhost_domain:           "web01.local"
vhost_docroot:          "/var/www/vhosts/"

db_user: wp01user
db_password: wp01password
db_name: wpdb01

site_name: site01
admin_name: site01-admin
admin_pass: pass01
admin_email: site01@hogehoge.com
```





## ハンズオン00

- Playbookの適用

```
$ ansible-playbook -i hosts/dev setup.yml
```



## ハンズオン01

- `setup.yml`の修正

```
  - role: handson00
  - role: handson01 #コメントを削除
 # - role: handson02
```

- Playbookの適用

```
$ ansible-playbook -i hosts/dev setup.yml
```



## ハンズオン02

-  `setup.yml`の修正

```
  - role: handson00
  - role: handson01 #コメントを解除
  - role: handson02 #コメントを解除
```

- Playbookの適用

```
$ ansible-playbook -i hosts/dev setup.yml
```



## Wordpressの画面見れましたか？