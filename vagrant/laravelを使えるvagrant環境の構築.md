# Vagrant環境の構築
## 今回はvagrantの起動まで整えます。
## 覚えておく、理解しておくといいコマンド
| コマンド            | 実行内容| 備考 |
| :-:                | :-: | :-:  |
| vagrant init       | 設定ファイルの生成 | box名を指定し作成,Vagrantfileが作成されます |
| vagrant up         | 起動 | |
| vagrant ssh        | 仮想環境にssh接続を行う | |
| vagrant halt       | 起動中の仮想環境を停止| |
| vagrant status     | vagarntの起動状態の確認 | |
| vagrant reload     | 起動中の仮想環境を再起動 | |
| vagrant destroy    | 対象仮想環境の削除(boxは消えない) | |

## まずはitermを開いてください

#### 1.任意の場所に移動する

```
~
$ cd 任意の場所
```
任意のディレクトリ
XAMPPやMAMPなどがある場合,
念のため __htdocs外__ の任意の場所
(_pooh3の場合,Documents(Desktopなどでも良い)_)


#### 2.新しいディレクトリを作成する

```
~/Documents
$ mkdir フォルダ名
```

#### 3.作成したディレクトリ内へ移動する

```
~/Documents
$ cd mkdirしたディレクトリ
```

_今回の場合この階層直下にプルしてくるためmkdirの際のディレクトリ名は適当で構いません
今回はkami1というディレクトリを作りました_

---

## vagrant構築環境があるか確認
#### Vagrantがインストールされているか確認するために、ヴァージョンを確認する

```
~/Documents/kami1
$ vagrant -v
Vagrant 2.1.2
```

`command not found`
と出た場合は手元のPCにVagrantを入れてください
## Vagrantにボックスを追加

#### 1.まずはボックスと言われるものを追加します

```
~/Documents/kami1 
$ vagrant box add centos7.1 https://github.com/CommanderK5/packer-centos-template/releases/download/0.7.1/vagrant-centos-7.1.box
```

今回インストールするのは`vagrant box add`の後にあるcentos7.1
_通信環境によっては時間がかかります。_

#### 2.ボックスがちゃんとダウンロードできているか確認しつつ、ボックス名をコピーしてください

以下のように出力されると思います

```
~/Documents/kami1
$ vagrant box list
centos7.1    (virtualbox, 0)
  他(以前boxを入れている場合)
```

#### 3.利用するためにまずボックスを初期化します
```
~/Documents/kami1
$ vagrant init centos7.1
```
結果にて

A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.

と出たら成功です

#### 4.Vagrantfileの中身をコメントインします。
わかりやすくするためにSublimeやvimなどのエディタを利用して３５行目の＃を削除してコメントインします

__変更前__
```
# config.vm.network "private_network", ip: "192.168.33.10"
```
__変更後__
```
 config.vm.network "private_network", ip: "192.168.33.10"
```

## これでVagrantは立ち上がります。
起動してみましょう
```
~/Documents/kami1
$ vagrant up
```
起動したら
```
~/Documents/kami1
$ vagrant ssh
```
下記のようになったら成功です
```
~/Documents/kami1
$ vagrant ssh
[vagrant@localhost ~]$
```

#### Vagrant環境内は`exit`で脱出です
#### Vagratnの終了は`exit`で自分のPCに戻って来た後に`vagrant halt`です
_ずっと起動しているのはPCに酷なので適宜終了してください。