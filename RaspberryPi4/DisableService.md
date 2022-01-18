# 概要
実行されているサービスに使用しないものが存在していると，セキュリティホールになる可能性がある上にリソースを食い潰す．  
なので，不要なサービスは停止する必要がある．  

# 不要サービスの停止
## 1. サービスの確認
"netstat"コマンドを使用するとポートを使用しているサービスを確認することができる．  
ここで出てきたサービスに不要なものがあれば停止，自動起動の無効化を行う．  

## 2. 不要サービスの停止と自動起動の無効化
私は"avahi-daemon"が不要なので以下のコマンドでサービス停止と自動起動を無効化した．  
~~~
$ sudo systemctl stop avahi-daemon.socket
$ sudo systemctl disable avahi-daemon.socket
$ sudo systemctl stop avahi-daemon
$ sudo systemctl disable avahi-daemon
~~~

# 参考
1. [「ラズパイの穴」を埋めるために、不要サービスの確認とOS自動更新を実装する (1/2)](https://monoist.atmarkit.co.jp/mn/articles/1912/18/news026.html)
1. [Systemdを使ったRaspberry Piのプログラムの自動起動](https://tomosoft.jp/design/?p=11697#Systemd)