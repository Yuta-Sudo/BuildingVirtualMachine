# ここからは主にvagrant内でコマンド打ちます
- 要するに仮想環境内なので手元のmac自体に関係はないと考えてください

- さて__vagrant にすでに`ssh`している__(vagrantの中にいる)前提として進めます

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
[root@localhost vagrant]
# sudo yum -y groupinstall "development tools"
```
これは _ "development tools" _ という開発に必要なものとその依存環境を整えるパッケージを一気にinstallします。







firewallの際
$ yum update 
時間がかかります。

[vagrant@localhost ~]$ exit
ログアウト

~/Documents/kami1 
$ vagrant reload

sudo systemctl start firewalld

sudo firewall-cmd --add-service=http --zone=public --permanent

sudo firewall-cmd --reload

❯ cp -p .env.example .env
 
~/Documents/kami1/kami master*
❯ php artisan key:generate
Application key [意味のない文字列] set successfully.



/Users/yuta/Documents/test


,
   type: "rsync",
   owner: "vagrant",
   group: "vagrant"
   rsync__exclude: [".git", ".gitignore", "tmp", "log", "cache"],
   rsync__chown: false



   ❯ vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Clearing any previously set forwarded ports...
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
    default: Adapter 2: hostonly
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default: Warning: Connection reset. Retrying...
    default: Warning: Remote connection disconnect. Retrying...
    default: Warning: Connection reset. Retrying...
    default: Warning: Remote connection disconnect. Retrying...
    default: Warning: Connection reset. Retrying...
    default: Warning: Remote connection disconnect. Retrying...
    default: Warning: Connection reset. Retrying...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
    default: The guest additions on this VM do not match the installed version of
    default: VirtualBox! In most cases this is fine, but in rare cases it can
    default: prevent things such as shared folders from working properly. If you see
    default: shared folder errors, please make sure the guest additions within the
    default: virtual machine match the version of VirtualBox you have installed on
    default: your host and reload your VM.
    default:
    default: Guest Additions Version: 4.3.30
    default: VirtualBox Version: 5.2
==> default: Configuring and enabling network interfaces...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
==> default: Mounting shared folders...
    default: /vagrant => /Users/yuta/Documents/test
==> default: Machine already provisioned. Run `vagrant provision` or use the `--provision`


sudo /etc/init.d/vboxadd setup



## 環境構築を始めます

今回の最終目的がvagrantを使用してLaravel(PHP FW)のwelcome pageを表示するところまでです。

- 必要となるパッケージとして
  - apache / nginx (両方で表示ができるようにします)
  - php version 7.*.*
  - 現段階では、アプリケーションとして何か動作させるわけではないのでDBは、後ほど課題の際に導入してもらいます

ではinstallの手順です
  ```shell
  sudo yum -y install httpd
  ```

上記コマンドでapacheのinstallが完了し次にphpの最新versionをinstallします

apacheと同様なinstallですと古いphpがinstallされます。今回必要となるphpのversionが7以上です(Laravelの最新のversionは、phpのversionが7以上と指定されています)。古いversionをinstallするのでなく新しいversionをinstallするには、CentoOSに最新のphpが取得できるように設定を行ってあげます

  ```shell
  sudo yum -y install epel-release
  # yumがパッケージを取得しにいく場所の更新を行います
  sudo wget http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
  sudo rpm -Uvh remi-release-7.rpm
  sudo yum -y install --enablerepo=remi-php71 php php-pdo php-mysqlnd php-mbstring php-xml php-fpm php-common php-devel
  php -v
  ```

phpのversionが確認できたでしょうか？ phpのinstallに関しては、以上で終わりです。
apaceh + php を使用してのLaravelのwelcome pageの表示を行います。
LaravelのProject作成には、composerが必要になりますのでcomposerのinstallを行います。
公式のDownload手順を踏襲します。

  ```shell
  php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
  php composer-setup.php
  php -r "unlink('composer-setup.php');"
  # グローバルにコマンドを使用するためにfileの移動を行います
  sudo mv composer.phar /usr/local/bin/composer
  composer -v
  ```
composer のversionが確認できたら完了です。この状態でLaravelのProjectを作成する準備は、終わりました。

## Laravelを導入する

Laravelを作成するコマンドがありますでそれを実行します

  ```shell
  # まず最初にLaravelを作成するディレクトリに移動します
  cd /vagrant
  composer create-project laravel/laravel laravel_test
  ```
上記コマンドを実行すると少し時間がかかりますがLaravelのProjectが作成されます。

## apacheの設定fileにLaravelのProjectまでのPATHに変更する

apacheの設定fileは、*/etc/httpd/conf/httpd.conf* にありますのでこのfileをRootの権限を借りて編集します。
編集には、普段使うエディタを使用せず元々CentOSに存在する*vi*を使用し行います。

  ```shell
  # 必ずsudoを使用しなければvagrant userでは編集できません
  sudo vi /etc/httpd/conf/httpd.conf
  ```

## 編集箇所は、大きく分けて3箇所あります。(同一file内にてです)

```apache
# DocumentRoot

DocumentRoot "/var/www/html"

#↓ 以下に変更

DocumentRoot "/vagrant/laravel_test/public"

# Directory

<Directory "/var/www/">
  AllowOverride None
  Require all granted
</Directory>

#↓  以下に変更

<Directory "/vagrant/laravel_test/public"> # ここを変更
  AllowOverride All  # ここを変更
  Require all granted
</Directory>

# User, Group

User apache
Group apache

# ↓  以下に変更

User vagrant
Group vagrant
```

## 編集が終わったら一度apacheを起動しましょう！

  ```shell
  sudo systemctl start httpd
  ```

起動しましたらホストOS側でブラウザに *http://192.168.33.10* と入力し確認して見ましょう。ブラウザ画面には、Laravelのwelcomeページが表示されましたでしょうか？

おそらく答えは、「No」だと思います。画面すら表示されずの状態かと思います。
ではなぜこのような画面が表示されているのか、答えは、 ブラウザに書いてあります。

表示されている文言の下部に「ファイヤーウォール」という単語が確認できますでしょうか？聞きなれない単語かと思いますがセキュリティという観点では、「ファイヤーウォール」というのは、大切な機能です。なのでできればこの機能は、止めないように起動状態のままホストOS側からアクセスできるようにしたいのです。

Vagrantfileの編集をした際を思い出してください。コメントアウトされてた箇所を二箇所有効にしました。
その有効にした箇所でconfig.vm.network "forwarded_port", guest: 80, host: 8080の記述がありました。この80というのは、httpという通信を行うためのポートと呼ばれる窓口です。
なのでファイヤーウォールに対してこのポートを経由したhttp通信によるアクセスを許可するためのコマンドを実行します。

  ```shell
  sudo firewall-cmd --add-service=http --zone=public --permanent
  # 新たに追加を行ったのでそれをファイヤーウォールに反映させるコマンドも合わせて実行します
  sudo firewall-cmd --reload
  ```

では、一旦この状態で画面を確認しましょう。

もし表示されなかったのならば一度以下のコマンドを実行しましょう。

  ```shell
  sudo systemctl restart httpd
  ```

問題なく*Laravel*のwelcome画面が表示されましたでしょうか？
