
[vagrant@localhost ~]$ sudo su
[root@localhost vagrant]# sudo yum -y groupinstall "development tools"


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


config.vm.synced_folder "/Users/yuta/Documents/test/test", "/home/vagrant/test/KAMI_NEW",
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

