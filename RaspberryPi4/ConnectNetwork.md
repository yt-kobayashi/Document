# 概要
OSのアップデートや，SSH接続などを行うためにはネットワークに接続しなければならないので，設定を行います．  

# ネットワークの設定
## 1. IPアドレスの固定
サーバとして運用するならIPアドレスの固定は必須です．  
DHCPを生かしたままサーバとして運用するのは手間が掛かりますし，面倒です．  
ルータにDHCPのアドレス割り振りの範囲が設定されているはずですので，その範囲外でIPアドレスを固定すれば良いです．  

IPアドレスを固定するためには`/etc/dhcpcd.conf`を編集します．  
~~~
$ sudo vi /etc/dhcpcd.conf
~~~

設定する必要がある項目は以下の4つです．
1. 設定するインターフェースの名前
1. 設定したいIPアドレス
1. ルータのアドレス
1. DNS(ドメイン ネーム サーバ)のアドレス

ファイルには以下の記載を行います．  
今回はipv6の設定は行いません．  
また設定するDNSはGoogleのものにしています．  

~~~
# Excample static IP configuration:
#interface eth0
#static ip_address=192.168.0.10/24
#static ip6_address=fd51:42f8:caae:d92e::ff/64
#static routers=192.168.0.1
#static domain_name_servers=192.168.0.1 8.8.8.8 fd51:42f8:caae:d92e::1c
interface eth0
static ip_address=192.168.10.84/24
static routers=192.168.10.200
static domain_name_servers=8.8.8.8
~~~

以上が設定できたらラズパイを再起動して，ネットに繋がるか確認します．  
ネットに接続できたら作業は完了です．  


# 参考
1. [[Raspberry Pi] Wi-FiやIPアドレスの設定を行う方法](https://dev.classmethod.jp/articles/raspberrypi-remote-connect/)