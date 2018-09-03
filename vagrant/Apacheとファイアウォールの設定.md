# Apacheの設定

- 以前インストールだけ終わらせたhttpd(Apache)の設定をします。
- apacheの設定fileは、/etc/httpd/conf/httpd.conf にありますのでvimを使用し行います。

## systemctl status httpdで状態を確認する
`systemctl status httpd`で今のapacheの状態を確認できます。
おそらく動いてはいないと思いますが、下記のように表示されたら絶対に`systemctl status stop`で停止してください

### NGのパターン例
```
[root@localhost vagrant]# systemctl status httpd
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset: disabled)
   Active: active (running) since 木 2018-08-30 09:57:27 BST; 11min ago
     Docs: man:httpd(8)
           man:apachectl(8)
 Main PID: 3824 (httpd)
   Status: "Total requests: 0; Current requests/sec: 0; Current traffic:   0 B/sec"
   CGroup: /system.slice/httpd.service
           ├─3824 /usr/sbin/httpd -DFOREGROUND
           ├─3825 /usr/sbin/httpd -DFOREGROUND
           ├─3826 /usr/sbin/httpd -DFOREGROUND
           ├─3827 /usr/sbin/httpd -DFOREGROUND
           ├─3828 /usr/sbin/httpd -DFOREGROUND
           └─3829 /usr/sbin/httpd -DFOREGROUND
```


## httpd.confを編集する

/etc/httpd/conf/httpd.confにapatcheの設定があるので編集します。
とにかく安全のために編集時は絶対に`systemctl status stop`でapacheを止めてから編集してください。

```
[root@localhost vagrant]#
vi /etc/httpd/conf/httpd.conf
```
上のコマンドを入力したら、難しそうで気が滅入っちゃいそうな画面に入ります。

## 操作方法がわからなければ　`esc` と `:q!` を順に押して閉じてください
先ほど画面ではキーボードの`J`を使い下にスクロールし編集場所で`I`で編集モードモードに入り、編集します。そして、`esc`で編集モードから脱出します。。
操作方法は難しいと思うので下記に記述しておきますので、なれるようにしておいてください。
なお、練習中/本番中少しでもおかしいなと思ったら、落ち着いて`:q!`で脱出してください。
操作に慣れない人は[このページ](/ref/viの操作.md)も見ながら操作してください。

## 操作方法を理解したらhttpd.conf内を編集して行きます。
```
[root@localhost vagrant]#
vi /etc/httpd/conf/httpd.conf
```

６６・６７行目
変更前
```
User apache
Group apache
```

変更後
```
User vagrant
Group vagrant
```

119行目
変更前
```
DocumentRoot "/var/www/html"
```

変更後
```
DocumentRoot "/vagrant/プロジェクト名/public"
```

１２４〜１２８行目内
変更前
```
<Directory "/vagrant/プロジェクト名/public">
    AllowOverride None
    # Allow open access:
    Require all granted
</Directory>
```

変更後
```
<Directory "/vagrant/プロジェクト名/public">
    AllowOverride All
    # Allow open access:
    Require all granted
</Directory>
```






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





