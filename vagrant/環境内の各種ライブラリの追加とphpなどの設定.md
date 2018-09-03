# ここからは主にvagrant内でコマンド打ちます
- 要するに仮想環境内なので手元のmac自体に関係はないと考えてください

- さて__vagrant にすでに`ssh`している__(vagrantの中にいる)前提として進めます

- この章ではPHPとcomoserのインストール
- vagrantとホストのファイル同期
- laravelのクローン並びにcomposerを使ったアクティベートまで行います。

##### また、コマンドをわかりやすくするために以後一行目に場所を指定します。
```
[vagrant@localhost ~]#
コマンド
```
とします。
# rootユーザーとしてログインします
```
[vagrant@localhost ~]# 
sudo su
```
と打ち込んだら
```
[root@localhost vagrant]#
```
と現在のばしょが変わったように見えます。

これは自分がrootユーザーとしてログインしていることを示しています。



# 環境構築前に必要なパッケージを導入
これは環境により時間がかかる場合があります。
```
[root@localhost vagrant]#
sudo yum -y groupinstall "development tools"
```
これは _ "development tools" _ という開発に必要なものとその依存環境を整えるパッケージを一気にinstallします。
時間がかかるので気をつけてください。

# Apacheのインストール
Apacheをインストールしてください。
```
[root@localhost vagrant]#
  sudo yum -y install httpd
```
ここではインストールのみで結構です。
設定は後ほど行いますので、PHPを導入しましょう。

# PHPのインストール
今回はphp 7.1.21(最新)をインストールしていきます。

1. まずは、yumがパッケージを取得しにいく場所の更新を行います。
```
[vagrant@localhost ~]# 
sudo yum -y install epel-release
```
2. vagrantにダウンロードしてます。
```
[vagrant@localhost ~]# 
sudo wget http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
```
3. ファイルを整えます。
```
[vagrant@localhost ~]# 
sudo rpm -Uvh remi-release-7.rpm
```
4. vagrantにphpをインストールします。
```
[vagrant@localhost ~]# 
sudo yum -y install --enablerepo=remi-php71 php php-pdo php-mysqlnd php-mbstring php-xml php-fpm php-common php-devel
```
5. 確認します
下記のようにコマンドを打ち
```
[vagrant@localhost ~]$ 
php -v
```
下記のように返答が来たら成功です。
```
PHP 7.1.21 (cli) (built: 月 日 年 時:分:秒) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.1.0, Copyright (c) 1998-2018 Zend Technologies
```
次はcomposerを導入します。

# composerのインストール
コンポーサーとはlaravelの依存関係を整えてくれるツールです。
1. コンポーサーをダウンロードします(?)
```
[vagrant@localhost ~]# 
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
```
2. コンポーサーを使えるようにします。
```
[vagrant@localhost ~]# 
php composer-setup.php
```
その後、不要になったsetupを消します。
```
[vagrant@localhost ~]# 
php -r "unlink('composer-setup.php');"
```
3. コンポーサーをどこでも使えるようにします。
```
[vagrant@localhost ~]# 
sudo mv composer.phar /usr/local/bin/composer
```
4. コンポーサーのversionを確認します。
```
[vagrant@localhost ~]# 
composer -v
```
確認ができたら成功です。

# vagrant内にlaravelプロジェクト用にからのディレクトリを作成します。
1. 念のためにcdでHomeに移動しときます。
```
[vagrant@localhost ~]# 
cd
```
### __注意)新しくlaravel を作成する場合ここでcomposerを利用して作成してください。また、以降の設定とは異なるので自分で理解した上で設定してください__

2. Homeでディレクトリを作ります。
```
[vagrant@localhost ~]# 
mkdir プロジェクト名
```
3. 先ほど作成したディレクトリに移動します。
```
[vagrant@localhost ~]# 
cd プロジェクト名
```
4. pwdでpathを確認します。
```
[vagrant@localhost プロジェクト名]$ 
pwd
```
結果
```
/home/vagrant/プロジェクト名
```
となるはずです。
この `/home/vagrant/プロジェクト名`はコピーしてメモか何かにペーストしてください。
あとでホスト側でもpathを取り両方使います。

5. `exit`、`exit`でvagrantから脱出しホスト(Mac)に戻って来てください。

6. ホスト(Mac)に戻って来たら`vagrant halt`でvagrantを停止させてください。

# vagrantの同期機能を使う
ここではvagrant内とホストOS(Mac)のディレクトリを同期させます。
さて前回`exit`しホストに戻って来ました。

1. `vagrant up`を行ったディレクトリに戻って来ましたよね？

2. 新しくディレクトリをプロジェクト名で作成してください。
```
~/Desktop/local
mkdir プロジェクト名
```
3. 作成したディレクトリにプロジェクトをsorce tree などでクローンして来てください。
```図
  任意のフォルダ名 --- vagrantfile
                --- 新しく作ったdir ⬅ここにクローン
```

4. ターミナルでクローンして来たディレクトリまで行きpwdで場所を確認してください。
```例
~/Desktop/local/
cd プロジェクト名

~/Desktop/local/プロジェクト
$ pwd
/Users/yuta/Desktop/local/プロジェクト
```
こちらでもpathは先ほど同様、必ずコピーしメモか何かにペーストしておいてください。

5. 完了しましたら元のvagrantfileがあるディレクトリまでもどります。
```
~/Desktop/local/プロジェクト
cd ../
```

6. Vagrantfile内の４６行目をコメントインして編集して保存してください。
変更前
```
# config.vm.synced_folder "../data", "/vagrant_data"
```
変更後
```
config.vm.synced_folder "/Users/ユーザー名/場所/フォルダ名/プロジェクト名", "/home/vagrant/プロジェクト名"
```
変更後の例(偽名・モザイアリ)
```
config.vm.synced_folder "/Users/pooh/Desktop/local/プロジェクト名", "/home/vagrant/プロジェクト名"
```

7. 再びvagrant内に入って確認してください(コマンドのみ掲載)
```
vagrant up #この時点で時間がかかってmounting ../ => ../ と出てたらまず成功です。
vagrant ssh
#以下vagrant内
sudo su
cd プロジェクト名
ls
```
おそらく同期しているはずです。

8. 確認ができたらvagrant 内のプロジェクトのディレクトリでvendorを作成します。
```
[vagrant@プロジェクト名 ~]# 
composer install
```
成功するかと思います。

9. envファイルがないと思うので作成します。
```
[vagrant@プロジェクト名 ~]# 
cp .env.example .env
```

10. laravelアプリを使えるようにkeyを生成します。
```
[vagrant@プロジェクト名 ~]# 
php artisan key:generate
```

ファイルの同期、laravelの実装は完了したはずです。
しかし、今のままでは表示ができません。

## 次のチャプターで表示まで終わらせます

## [目次に戻る](/../README.md) 