---
layout: post
date: 2016-07-09
title: "EvernoteにPDFファイルをアップロードするRubyスクリプト"
categories: Ruby Evernote Programming
timezone: Asia/Tokyo
---

## preface / background

最近のランサムウェアの流行をうけて少し自分のデータをどうにかしたいと思いました。

### 写真が大事

一番大事なのは、”思い出プライスレス”なプライベート写真や動画になるかと。

対策として考えたのが、オンラインクラウドサービス。Dropboxだとローカルから見えてしまうのでここもランサムウェアにやられるリスクはあります。
いまはFlickrをはじめ、Google, Amazon などフォトストレージに対するサービスが無料または低価格であるのでここにプライバシーを恐れつつ、写真をアップロードすることで
バックアップ代わりになります。
そのままオンラインストレージを利用するのは少しコストが個人的には掛かり過ぎるのでこういう手段を利用。

### 2番目は文書

次に大事かなと思ったのが文書関係。とくにPDFたち。kindleなどで購入したものはアカウントがあるかぎり再ダウンロードできるので問題ないですが、
例えばこどもが描いた絵のスキャンデータであったり、レポートや名刺のスキャンデータは失うと代替手段がないなと思ったので
これもどうにかしたいと思いました。
そこで考えたのが、Evernoteの利用です。Evernoteにスキャンデータをアップロードすれば第2のバックアップストレージになると同時に
Evernoteの検索対象として扱うこともできます。
ただし、問題があって、100近くあるPDFファイル群を手作業でEvernoteにアップロードしてタグ付けしたり、タイトルつけるのは面倒すぎる。
ということでスクリプト書いて利用してみました。

EvernoteはAPI公開してるし、そもそもサンプルスクリプトが充実しててあっさり作成出来ました。

### その他

映画やCDの音楽データなどは失うのは金銭的には痛いですが、また購入・レンタルすれば良い話。
いまは便利なオンラインストリーミングサービスが充実してますし。


## 準備

1. 開発者用にテストできるsandbox 用アカウントを作成

    以下のリンクからサンドボックスのEvernoteを利用することが出来ます。
    アカウントがない場合は新規に作成します。既存のEvernote アカウントは使えないので、初めて開発する場合は新規作成します。

    https://sandbox.evernote.com/Login.action?targetUrl=%2FHome.action%3F

2. APIキーの作成

    APIキーはこちらから取得することが出来ます。

   [Home - Evernote Developers](https://dev.evernote.com/#apikey)

    今回は楽な(でもセキュリティ上リスクのある) Developer Tokenを使ったので、APIキーは使わなかった気がする。。。

3. Evernote SDK のインストール

    利用したいプログラミング言語に合ったものを利用します。
    今回はrubyを使うのでruby向けのものをインストールします。    

    ``` $ gem install evernote_oauth ```

    上記のgem ではなく、evernote-thrift だけで良いようです。     
     
    ``` $ gem install evernote-thrift ```    
    
4. サンプルコード

    サンプルコードのダウンロード
    ここのを参照します。

    [https://github.com/evernote/evernote-sdk-ruby](https://github.com/evernote/evernote-sdk-ruby)

    ``` $ git clone https://github.com/evernote/evernote-sdk-ruby.git```

    このsampleフォルダにはいっているClient の方に添付ファイルをアップロードする処理がそのまま書かれていて流用出来ました。


### デベロッパトークンの取得

以下から取得できます。最初はサンドボックス用トークンを使って自分の作成したスクリプトの挙動を確認しました。
その後、プロダクション用デベロッパトークンを作成し、スクリプトを本番用に修正して利用しました。

[認証 - Evernote Developers](https://dev.evernote.com/intl/jp/doc/articles/authentication.php#devtoken)


## Code

サンプルコードを参考にファイルを連続アップロードするスクリプトに変更しました。

* ノートブックはデフォルトをそのまま使うようにし、
* タイトルはファイル名を利用、
* tagは適当につけましたが複数つけています。
* また、PDFのみをアップロードするように制限していますので別のファイルも対象としたい方はそこを直してください。

あまり汎用性は考えていないのですが、こちらに置いておきます。

たまにアップロード中のネットワークの切断とかでエラーになります。
すでに同様のコンテンツがあるかどうかは確認していないため、重複が嫌な場合はそこを調整して再開する必要があります。


[https://github.com/maple/uploadhelper4Evernote/blob/master/uploadHelper2Evernote.rb](https://github.com/maple/uploadhelper4Evernote/blob/master/uploadHelper2Evernote.rb)

## 最後に

ランサムウェア対策として、バックアップ先の一つとしてEvernoteを選択しました。
大量のドキュメントをアップロードするのでプレミアム会員になりました。


## 参考情報

Evernoteにおける添付ファイルの扱いはこちらで解説されています。

[リソース - Evernote Developers](https://dev.evernote.com/intl/jp/doc/articles/resources.php)

ノートを作成する方法は以下を参考にします。
こちらに必要条件として「デベロッパトークンまたは有効な認証トークン取得が可能な OAuth の実施」との記載があり、
私はデベロッパトークンを利用したので結果APIキーも使わなかった気がします。






