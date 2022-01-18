# 概要
導入したばかりの"Raspbian"は最新ではない．  
その為にセキュリティに問題を抱えている可能性があるため，OSのアップデートを行う必要がある．  

# OSのアップデート方法
## 1. OSのアップデート
OSのアップデートは以下のコマンドを入力するだけの簡単操作である．  
~~~
$ sudo apt update
$ sudo apt upgrade
$ sudo apt dist-upgrade
$ sudo apt autoremove
$ sudo apt autoclean
~~~

## 2. アップデートの自動化
Raspberry Piは常時動作することに真価を発揮する．  
その為，セキュリティホールを放置せずアップデートは速やかに行う必要がある．  
ただ，毎日ログインしてあるか分からないアップデートを確認するのは非常に面倒で，次第に放置することは自明の理である．  
そこで，アップデートを"crontab"に登録して自動化してしまおうと思う．  

以下のコマンドを実行すれば"crontab"の編集が行える．    
~~~
$ sudo crontab -e
~~~

初回起動時はインストールされているエディタを選ぶステップがあるので，選択する．  
私はVi(Vim)が好きなのでViにしている．  

選択が完了したら以下の設定を最下行に追加し，エディタを閉じる．  
~~~
0 0 * * * sudo apt-get update && sudo apt-get -y dist-upgrade && sudo apt-get -y autoremove && sudo apt-get autoclean
~~~
その後再起動を行い，エラーが発生していなければ作業は完了です．

# 参照
1. [「ラズパイの穴」を埋めるために、不要サービスの確認とOS自動更新を実装する](https://monoist.atmarkit.co.jp/mn/articles/1912/18/news026.html)