---
layout: post
date: 2016-09-27
title: "写真データの時間情報を編集する"
categories: Ruby Photo Exif Time Amazon
timezone: Asia/Tokyo
---

## preface / background

写真のバックアップにオンラインストレージをいくつか使っています。
そのうちの一つ、Amazon Photos (プライムフォト)を使ってるのですがこれの便利なサービスとして
「This Day」として"数年前の今日" の写真をみせてくれます。


この機能が好きな理由として、

- 「あー、あの頃こんなことがあったなぁ」と意図せず振り返ることができる
   大量にある昔の写真を全部みるのは大変なので何かをきっかけに軽く振り替えれるのはいいなと。

- ○年前に比べて、こんなに成長してる!/老けてる！/建物が新しい! といった時間経過に対する変化を感じることができる

というのがあります。

これを使うようになって、「出来る限り日時を当時の正確な時間に合わせたい」という気持ちが強くなりました。
特にずれるのがスキャンしたデータです。データが作成された日とスキャナでスキャンした日が違うのでファイルの
タイムスタンプがずれます。当然ながらExif 情報も入っていません。

スキャンする例として、イベントに参加したときとか写真撮影提携企業が撮影した写真があります。
業者からのデジタルデータ販売ってほとんどされなくて(スタジオアリスなどは1年遅れで提供してくれたりします。。)
紙の保存だけでは不便なのでスキャナで読み取っていたのですが、フォトストレージサービスを使ってみて
スキャンした日付で過去の思い出が表示されるのは違和感を感じるようになってしましました。


<!--
参考) <a href='{{ base.url }}{2016/04/02/Amazon-Cloud-Drive-App-On-Mac/}'>以前書いたAmazon Photos クラウドサービスの記事</a>
-->



## 事前準備

### サービス調査

nohanaという株式会社mixiの新規事業として立ち上がり、いまは独立したフォトブック作成のアプリ・サービスを提供している企業があります。
ここにファイルのタイムスタンプのずれているアップロードしても正しく日時表示される場合があり、おそらくこのサービスはExifのタグ情報を
利用しているのだと推測しました。
調べればいいのですが、きちんと確認していません。調査してない。


### 変更する箇所

以前、ruby でExifのタグ情報から位置情報を読み取り、Google Map上にセットするという
コードを書いたことがあり、rubyではライブラリを使えば簡単に出来ることが
分かっていたのでそのまま使用することにしました。

変更箇所は以下の2点にしました。

* ファイルの最終変更日時及びアクセス日時
* Exifのファイル生成日時

### 設定する日時の特定方法

いくつか方法があるのですが、条件によって使い分けることにしました。


* Case1: Exif情報が設定されていて、ファイルタイムスタンプがおかしい場合

    例えば、ネットワーク経由でファイルコピーした場合、起こりえます。
    その場合、Exif Tagから日時情報を読み取り、ファイルタイムスタンプに設定することにしました。

* Case2: ファイル名に時間情報を含んでいる場合

    主にDropboxですが、自動で写真をアップロードする機能(Camera Uploads)を利用している場合、画像ファイルを生成する際にファイル名にそのときの時間情報を含めるようです。
    例えば、"2015-11-11 19.51.15.jpg"と空白含む文字列になります。

* Case3: Exifの有無関係なく、指定した日時にファイルのタイムスタンプ及びExif情報を上書きしたい場合

    主にスキャンしたファイルが対象になります。

## Code

以下におきました。

### Case1.

[applytimestamp_fromExif.rb](https://github.com/maple/ruby_tools/blob/master/photos/applytimestamp_fromExif.rb)


### Case2. 


[applytimestamp_fromfilename.rb](https://github.com/maple/ruby_tools/blob/master/photos/applytimestamp_fromfilename.rb)

### Case3.

引数で日時を渡してあげることでその情報を適用するようにしました。
使い方はこんな感じです。

```sh
> ruby apply_Exif_timestamp_from_specific_date.rb 201609011100
````

[apply_Exif_timestamp_from_specific_date.rb](https://github.com/maple/ruby_tools/blob/master/photos/apply_Exif_timestamp_from_specific_date.rb)

```ruby
#!~/.rbenv/shims/ruby
### ---------------------------------
# need to install gem of mini_exiftool & exiftool
# 
# gem install mini_exiftool
#   Also install exiftool http://www.sno.phy.queensu.ca/~phil/exiftool/

require "mini_exiftool"

def set_timestamp_to_exiftags (time: , file: )
  photo = MiniExiftool.new file.to_s
  # p photo.date_time_original
  photo.date_time_original = time
  photo.date_time = time
  photo.date_time_digitized = time

  begin
    photo.save!
    puts "done to rewrite exif timestamp : (#{file})"
  rescue => e
    puts e
  end
end

# create filelist
def create_filename_list (param)
  ar = []
  # Dir class support upcase & lowercase letter on ruby v2.2. i.e: jpg, JPG.
  Dir::glob(param){|f|
    next unless FileTest.file?(f)
    #ar << "#{File.basename(f)} : #{File::stat(f).size}"
    ar << f
  }
  return ar
end

# set start time.
ARGV[0] ? date = ARGV[0] : date = Time.now.to_s

# extension
ext = "jpg"
location = Dir::pwd

# pick up files
param_of_search = location + "/**/*" + ext

filelist = create_filename_list param_of_search

localtime = Time.parse(date)

filelist.each { |f|
  set_timestamp_to_exiftags file:f, time: localtime
  File::utime(localtime, localtime, f)
  localtime = localtime + 10
}

```


理由は忘れてしまったのですが、画像のPixelサイズをセットしておきたくなって時間以外の情報をセットしたものを以下におきました。

[apply_Exif_timestamp_and_more_from_specific_date.rb](https://github.com/maple/ruby_tools/blob/master/photos/apply_Exif_timestamp_and_more_from_specific_date.rb)

### 参考

exif 情報を書き換えるのはexifrではできなかったため、以下のサイトを参考にmini_exiftool を利用しました。

> [RubyでEXIFを見る。mini_exiftoolを入れる。 - それマグで！](http://takuya-1st.hatenablog.jp/entry/20120327/1332847944)

## 結果

Amazon Photosに実際にアップロードして確認してみました。

![Amazon Photos 画像プロパティ]({{site.baseurl}}/images/AWS_timestamp_info.png)


ちゃんと反映されました。よかった。

### 躓いたこと

Mac向けDesktop Toolを使うと同じ名前のファイルはコンフリクトを起こしてエラーになってしまいました。
写真を閲覧してるとファイル名など分からないし、検索機能は提供されていないので大量にアップロードしたあとに当該ファイルを削除するのは非現実的。

せっかくタイムスタンプ補正したのに。。。とがっかりしつつ調べたら、Webコンソール上から
アップロードすれば重複したファイルが有った場合、対応方法を尋ねられるとのこと。
実際やってみましたが、尋ねられず、そのまま上書きされました。まぁいいか。。


### おまけ

Google Photos も「この日の思い出」という形で表示されるようになりました。
最初にも少し書きましたが、ノハナというフォトアルバム作成サービスがは書き込んだExif情報は影響せず、利用している日時情報が分かりませんでした。
ファイルのタイムスタンプを利用しているわけではなさそうなので、Exif のタグのどれを使ってるのかなぁ。


## 最後に

これを機に、いままでPDF化していたものも、JPGなど画像ファイル化してExif情報を付加することで
Amazon Photosのようなサービスではうれしい表示方法となるのではと感じています。
PDF自体の時間情報から「このファイルは3年前の今日作成されました」というようなことも可能ですが全然うれしくないので。

具体的には子どもの手形や学校での制作物(画集、お絵かきなど)などを想定しています。いままでPDF化を勤しんできましたが
文字列検索とかほぼしないので、JPEGの方が今後はよいと考えています。


実はこれまで特定のイベントに関する写真は関連する文書やパンフレットと合わせて、
PDFとしてイベント単位でPDF化していたのですが、
スキャナで何でもかんでもPDFではなく、JPEGが適しているというものもあるのだなと今回気づきました。



