---
layout: post
title: "Amazon Cloud Drive Desktop 【Mac版】で日本のアカウントが利用できない"
categories: Mac Apps Amazon Storage Cloud
tags: Mac Apps Amazon Storage Cloud
---

昨日はエイプリルフールでしたが相変わらずグーグルさん面白いですね。フリック入力ほしいです。
毎年見ていたカヤックさんや円谷プロのは見忘れました。。。


## introduction

さて、スマートフォンやタブレットではAmazon のPhoto Storage Serviceを利用しているのですがDesktop App の紹介を見かけました。どうやら前からあったようなのですが知らなくて、ではさっそくいれてみようとしたのですがちょっとだけ面倒だったのでその過程を。

## Motivation

Amazonプライム会員だと去年から日本でも写真は無制限・無圧縮でオンラインストレージが利用できるようになりました。なのでバックアップ用として利用したいと思ったのと、このサービス毎日「一年前の今日」「二年前の今日」とか保存してある写真の中から教えてくれて、偶然なつかしいものを目にしたりするので、こういう日常の何気ないものを小さな楽しみとして使うのもいいなと思い、積極的に利用しようとここ1ヶ月くらいで思ってました。

特にいま子どもが小さいのですが成長が早く、1年違うだけでこんなに変わるのかという驚きを与えられることがしばしば。過去の写真をいちいち見直すのも面倒だし、ふつうはそういうことしないのでさりげなく出してくれるこのアプリはいいなと思います。

## issues

インストール自体は下記リンクから出来ます。

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=teamasa-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=B00W9J9X1M" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

でも起動すると、Amazon.comのアカウントをいれろと言われ、日本のAmazon.co.jpのアカウントは受け付けられません。
Amazonの商品に対するレビューを読むとどうやら、Macのシステム言語あたりが原因のようです。

## solution

単に言語を変えるだけです。
Macの設定から選択します。
自分は、"System Preferences" を起動し、"Language & Region" を選択、"Advanced..."ボタンを押下して"Format Language"の設定を"Japanese"にして、Amazon Cloud Drive App を起動しなおせばよい。

一度 Amazon.co.jpのアカウントでログインできれば、システムの言語設定を戻してもそのままになるかも。


#### 2016.4.3 追記

System Preferences から設定を戻しましたが、今のところアプリに起動したアカウントで使えています。あまり納得出来ない方法ですが、当面はこれでしのげるかと。


-----

## Appendix.

当初、アプリごとに使用言語を変えられるのではと思いその方法を探していました。

ref.) [Macで特定のアプリケーションの言語設定を変更する - tnil's memo](http://tnil.hatenadiary.jp/entry/20150203/1422958501)

これを実際試したのですが、Amazon Cloud Driveで表示される言語は日本語になって一瞬喜びましたが、アカウントはAmazon.comのをいれろというメッセージは変わらず。。


```sh
> defaults write com.amazon.clouddrive.mac AppleLanguages '(ja)'
```

```sh
> defaults find AppleLanguages
    Found 1 keys in domain 'com.amazon.clouddrive.mac': {
        AppleLanguages =     (
            ja
                );
    }
    Found 1 keys in domain 'Apple Global Domain': {
        AppleLanguages =     (
            en,
            ja
        );
}
```


以下のコマンドによって、Chooser.app を管理者権限で起動して言語を変えることもできる模様ですが試してません。おかしくなっても責任はとれません。

```sh
> sudo "/System/Library/CoreServices/Language Chooser.app/Contents/MacOS/Language Chooser"`
```

