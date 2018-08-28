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
[root@localhost vagrant]#
sudo yum -y groupinstall "development tools"
```
これは _ "development tools" _ という開発に必要なものとその依存環境を整えるパッケージを一気にinstallします。






composerをインストールしてきたら同期フォルダの設定をします。


絶対パスに
/home/vagrant/KAMI_NEW

/Users/yuta/Desktop/local/KAMI_NEW

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


app
artisan
bootstrap
composer.json
composer.lock
config
database
package.json
phpunit.xml
public
readme.md
resources
routes
server.php
storage
tests
vendor
webpack.mix.js


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



