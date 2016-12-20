---
layout: post
date: 2016-12-20
title: "レジストリを書換えて、Windowsのキーマップを変更する"
categories: windows keyboard registry
excerpt: "Windowsでキーボードの配置に不満があったので変更した話"
timezone: Asia/Tokyo
---


## preface / background

だいぶ前になりますが Windows ノートPC 用にBluetooth キーボードを購入しました。
「スリムなこと」、「無線接続であること」、「USキー配列であること」でこの製品を選択しました。

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=teamasa-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=B00U260UR0&linkId=bdba6e10e37dea8f1b30cc380aa78583"></iframe>

使ってるうちに問題が生じたのがWindowsキーの存在です。私はわりとこのキーを使ってプログラム起動したり、シャットダウンしたり、エクスプローラーを開いたりとしてるので使えなくなるのは困りました。
また、他のキーでもちょっと困ったことが置きました。(前のこと過ぎて忘れた)

これまでもCaps LockとCntrol キーの配置を入れ替えることはしていたのですが、他のキーの入れ替えなどは今回初めてだったので整理します。


## 参考にしたサイト

- [「Caps」と「Ctrl」の入れ替え](http://uguisu.skr.jp/Windows/winCaps.html)

- [ ASCII.jp：CtrlとCapsを入れ替え！　Windowsのキー配列をカスタマイズ (2/2)｜深厚のWindows使いこなしテクニック](http://ascii.jp/elem/000/000/927/927191/index-2.html)


## やったこと


regedit.exe を起動して以下の場所に「バイナリ」を新規作成し、Scancode Map という名前にします。

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout]

reg ファイルの中身は以下のようになります。(この例はCaps LockをCtrlキーに置き換えたものです)

```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout]
"Scancode Map"=hex:00,00,00,00,00,00,00,00,03,00,00,00,1d,00,3a,00,3a,00,3a,00,00,00,00,00
```

```sh
# 無変換キーをWindow キーにマッピング + Caps Lock / Ctrl を全てCtrl キーに置き換え
# 左ctrl   00 3A
# CapsLock 00 1D
# スペースキーの隣りにある無変換キーをWindowsキーに変更
# 左Win    E0 5B
# 無変換   00 7B
00 00 00 00 00 00 00 00
05 00 00 00 1D 00 3A 00
3A 00 3A 00 5B E0 7B 00
1D 00 5B E0 00 00 00 00
```			 


## おわり

こういうのは調べればすぐ出来るのですが、滅多にしないのですっかり忘れてしまうので
備忘録としてここに残しておきます。

