# 概要
sshの初期設定だとセキュリティに問題を抱えているのでポート番号の変更以外の設定変更を行う．  

このページでは以下の設定で行います．  
* ラズパイのIPアドレス  :   192.168.10.84

# セキュリティ設定
## 1. 概要
`/etc/ssh/sshd_config`を編集する．  
~~~
$ sudo vi /etc/ssh/sshd_config
~~~
## 2. sshの設定
以下の項目について設定を行う．  

|項目|設定値|概要|
|---|:---:|---|
|LoginGraceTime|120|ログインを受け付ける時間の制限[秒]|
|PermitRootLogin|no|rootでのログインの禁止|
|StrictModes|yes|ssh関連のファイルの権限チェックを行う|
|MaxAuthtries|3|最大認証回数を3回に制限する|
|PasswordAuthentication|no|UNIXのパスワード認証の禁止|
|PasswordAuthentication|no|パスワード認証の禁止|
|permitEmptyPasswords|no|空パスワードの禁止|
|ChallengeResponseAuthentication|no|チャレンジレスポンスの禁止|
|UsePAM|no|PAMインタフェースによる認証の禁止|

これで最低限のセキュリティ設定が完了した．  

## 3. ssh接続の確認
ssh接続ができることが確認出来たら，sshの設定は完了です．  

# 参考
1. [Raspberry PiのSSH設定方法](https://qiita.com/tomokin966/items/bc22d09f97ebeb3955d2)  
1. [基礎から学ぶ！最初にやるべきSSHのセキュリティ設定【全コマンド例付き】](https://hackers-high.com/linux/ssh-config-for-beginners/)  
1. [SSHのセキュリティをさらに高める設定](https://hackers-high.com/linux/ssh-config-more-secure/)  
1. [sshd_configの設定項目](https://l-w-i.net/t/openssh/conf_001.txt)  
