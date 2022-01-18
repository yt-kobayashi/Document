# 概要
"Raspbian"の初期設定だとGUIに自動ログインがされてしまう．  
GUIは不要なのでログイン方法を変更する．  
また，リソースの節約にも役立つはず．  

# ログイン方法変更
## 1. ログイン方法変更
ラズパイの設定ツールを使用して変更を行う．  
~~~
$ sudo raspi-config
~~~
次に以下のように項目を選択していく．  

`3 Boot Options > B1 Desktop / CLI > B1 Cosole`  

選択後"Enter"キー押下後`<Finish>`を選択しEnterを押下する．  
以上で設定が行われるので，再起動を行いCUIでのログインを求められるか確認する．  

# 参考
1. [Raspberry Pi】GUIを無効にしてCUIで起動する方法](https://sekisuiseien.com/computer/11403/)
