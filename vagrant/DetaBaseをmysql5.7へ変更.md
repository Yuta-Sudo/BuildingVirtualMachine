# DBをmysql5.7に変更する

CentOS7にインストールされているDBはmariaDBとなってます。　

これをmysqlに変更する方法をここには載せておきます。

ここでもコマンド実行中に何か聞かれたらyと答えてください。

## 必ず全てをVagrant内で行ってください。

# まずはcentOSにmysqlのレポジトリRPMをインストールします。

RPMとはあらかじめコンパイル済みのバイナリと、関連するファイル群
　参考:「[Linuxの「パッケージ」と「yum」と「rpm」について勉強したのでまとめてみた。](https://qiita.com/sksmnagisa/items/05a6f8a707010b8bea56)」

1. vagrant内にパッケージをダウンロードしてきます。
```
[root@localhost vagrant]#
wget http://dev.mysql.com/get/mysql57-community-release-el7-7.noarch.rpm
```

2. ダウンロードしてきたパッケージをインストールします。
```
[root@localhost vagrant]#
sudo yum localinstall mysql57-community-release-el7-7.noarch.rpm
```

3. そこからMySQL本体をインストールします(時間かかります。)
```
[root@localhost vagrant]#
sudo yum install mysql-community-server
```
__これでmysqlはインストールされました。__


# 使えるように設定していく
さて、インストールもできたと思います。
必要なら休憩してください。
ここからは念のため休憩(空き時間)なしで、使う上での初期設定をしましょう。

## 1. 自動起動をonにします。
```
[root@localhost vagrant]#
sudo systemctl enable mysqld
```
## 2. 起動(デーモン)します。
```
[root@localhost vagrant]#
sudo systemctl start mysqld
```

## 3. mysqlに入るために仮に生成されたパスワードを確認してコピペしてください。
以下のコマンドで確認できます。
```
[root@localhost vagrant]#
sudo grep 'temporary password' /var/log/mysqld.log
```
実行
```
vagrant@localhost ~]# 
sudo grep 'temporary password' /var/log/mysqld.log
```
結果(自分の場合)
```
2018-08-28T06:37:12.386501Z 1 [Note] A temporary password is generated for root@localhost: lgF4=qF_Bp&q
```
よって私の環境でのパスワードは
```
A temporary password is generated for root@localhost: lgF4=qF_Bp&q
```
の`lgF4=qF_Bp&q`となります。(あくまでもランダムなので自分で毎回確認してください。)
## 4. 初めてmysql画面に入ります
1. ログインします。
```
[root@localhost vagrant]#
mysql -u root -p
```
上のコマンドを打ち込むと下記のように出ます。

2. 先ほど確認したパスワードを打ち込みます(コピーペーストでも可)
```
Enter password:
```
打ち込んだ文字は不可視ですので正確に打ち込んでください。

## 5. 画面が下記のようになったら成功です。
```
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.23
Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql>
```
mysqlにログインできました

## 新しく自分でパスワードを設定します
それでは自分用にパスワードを作成します
```
set password = "新しく設定するパスワード";
```
なお、パスワードには
- 大文字を１文字以上
- 記号を１つ以上
含む必要があるので気をつけてください。

## ここまででmysql(=DB)の設定は完了です
これ以降のmysqlの使い方は各々調べてください。