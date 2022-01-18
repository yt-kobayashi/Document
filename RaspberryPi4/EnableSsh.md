# 概要
サブPCとして使用するなら兎も角，SyslogサーバやNAS(ネットワーク アタッチド ストレージ)として使用するならsshで接続できた方が便利．  
さらに，sshで接続できれば画面を1つ占有されることも無くなるので一石二鳥．  

このページでは以下の設定で行います．  
* ラズパイのIPアドレス  :   192.168.10.84

# sshの有効化と設定
## 1. sshの有効化
"Raspbian"ではセキュリティの観点からsshは初期状態で無効にされているため，有効化する必要がある．  
有効にするためには`/boot`配下にsshディレクトリを作成したのちに再起動すれば良い．  
~~~
$ sudo mkdir /boot/ssh
$ sudo reboot
~~~

## 2. sshの設定
"ssh"は初期の状態だと攻撃を受けやすい．  
これはsshで使用するポートが知られていること，初期での認証方法がパスワード方式であることなどに起因している．  
そのため使用するポートを変更し，認証方法を公開鍵認証に変更し，細かい設定を行うことでセキュリティの向上を図る．  

### 2.1 sshで接続
まずはsshで接続できるかを試す．  
~~~
$ ssh newRasPi@192.168.10.84
~~~

Windowsの場合は以下のアプリケーションを使用すればssh接続が可能である．  
それぞれGoogleで検索すれば使用方法などを詳細に解説しているサイトが存在している．  

|アプリケーション名|概要|
|---|---|
|Teraterm|有名なアプリで企業でも使用されている|
|PuTTY|GitForWindowsで採用されているアプリ|
|PowerShell|OpenSSHクライアントをPowerShellにインストールすれば使用が可能|

### 2.2 公開鍵認証への変更
接続が確認出来たら認証方法を公開鍵認証へと変更する．  
今回は以下の設定で鍵を生成する．  

|項目|設定値|
|---|---|
|暗号化方式|RSA|
|鍵長|8192 bit|
|パスワード|半角大小英数字 11文字|
|コメント|hoge@gmail.com|
~~~
$ ssh-keygen -t rsa -b 8192 -C "hoge@gmail.copm"
~~~
RSA鍵長は2048bitあれば問題ないそうだが，安心を求めて8192bitにしている．  
またコメントは自身のメールアドレスを指定すればよい．  

上記のコマンドを実行すると"秘密鍵 id_rsa"と"公開鍵 id_rsa.pub"が生成される．  
公開鍵はラズパイへアップロードして使用するためのものなので，以下のコマンドを上から実行することでアップロードと設定を行う．  
ついでにサーバ側の公開鍵用の名前に変更も行っておく．  

~~~
[PC側]
$ ssh newRasPi@192.168.10.84

[Raspberry Pi側]
$ mkdir ~/.ssh
$ chmod 700 ~/.ssh

[PC側]
$ scp ~/.ssh/id_rsa.pub newRasPi@192.168.10.84:/home/newRasPi/.ssh/authorized_keys

[Raspberry Pi側]
$ chmod 600 ~/.ssh/authorized_keys
~~~

chmodではファイルの権限を設定している．  
このファイルの権限を設定しないと，ssh接続ができない．  

次にサーバ側のsshの設定を変更する．  
~~~
sudo vi /etc/ssh/sshd_config
~~~
エディタが立ち上がるとsshの設定を編集できるので，以下のようにコメントアウトを削除する．  
また，sshで接続するポート番号も変更しておく．  
~~~
[変更前]
    :
    :
#Port 22
#AddressFamily any
    :
    :
#PubkeyAuthentication yes

#AuthorizedKeysFile  .ssh/authorized_keys .ssh/authorized_keys2
    :
    :
~~~
~~~
[変更後]
    :
    :
#Port 22
Port 1234
#AddressFamily any
    :
    :
PubkeyAuthentication yes

AuthorizedKeysFile  .ssh/authorized_keys .ssh/authorized_keys2
    :
    :
~~~

ここまで設定が完了したら，一度ラズパイを再起動する．  

次にssh接続先が増えた際に対応できるように秘密鍵の設定を行う．  
現状ではラズパイ以外にsshができないため，configファイルを編集することで対応する．  
~~~
$ cd ~/.ssh
$ ls -l
$ mv ./id_rsa ./raspberrypi_rsa         ← 次に鍵を生成したときの上書き防止
$ vi ./config
~~~
エディタが立ち上がったら，以下の内容を追記する．  
~~~
Host raspberrypi                        # sshコマンドの後に入力する名前
    HostName 192.168.10.84              # ラズパイのアドレス 
    User newRasPi                       # ログインするユーザ名
    Port 1234                           # 接続するポート番号 サーバと同一のものを設定する
    IdentityFile ./raspberrypi_rsa      # 秘密鍵ファイル
~~~
編集が完了したらファイルの権限を設定する．  
~~~
$ chmod 600 ./config
$ chmod 700 ./raspberrypi_rsa
~~~

ここまで設定が完了したら，以下のコマンドを実行してsshでログインができるか確認をする．
~~~
$ ssh raspberrypi
Enter passphrase for key '/home/hogefuga/.ssh/keys/raspberrypi_rsa': "設定したパスワードを入力"
~~~

ログインが確認出来たら作業は完了です．  

# 参考
1. [Raspberry PiのSSH設定方法](https://qiita.com/tomokin966/items/bc22d09f97ebeb3955d2)  
1. [初心者向！Raspberry Pi 最低限のセキュリティ設定【所要時間 30分】](https://qiita.com/mochifuture/items/00ca8cdf74c170e3e6c6)  
1. [自宅にあるRaspberry Piをセキュリティ設定してWebサーバー化](https://qiita.com/mukoya/items/606c6cfbdf2f5065e0e4)  