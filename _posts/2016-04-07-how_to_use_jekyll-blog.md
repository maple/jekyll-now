---
layout: post
date: 2016-04-07
title: "jekyll now を使って変更したところ"
categories: Tools Infrastracture Installed Jekyll
---

jekyll nowを使い出してから1年経過しました。と言ってもあまり更新できなかったのですが。
これを使い始めた理由は[一番最初のページ]({% post_url 2015-1-25-introduction %})で書きましたが、今後もjekyll now を使い続けるかどうかちょっとだけ考察しました。

## result

がんばって継続しますよ

## reason

[一番最初のページ]({% post_url 2015-1-25-introduction %})で書いた理由に加えて、このサイトを立ち上げる理由の最大のきっかけは下記記事を読んだからです。

ref.) [**Kazuho's Weblog: なぜHTTPSはHTTPより速いのか**](http://blog.kazuhooku.com/2014/12/httpshttp.html) 

去年、HTTP2.0 がpublishされましたがこの動きに感化され、HTTPSが使えるブログを利用したいという気持ちが高まりました。それは今も変わらないのでこのままgithub 使っていこうかと。広告もでないし。

## points for improvement

これまでのjekyll nowを使う過程で修正したことや学習したTipsはいくつかあるのですがあたまに浮かんだものをこちらに記載しておきます。


#### カテゴリや検索機能をつける

今後記事が増えてきた時に分類して探しやすくするようにしたい、または検索して見つけられるようにしたいと思って追加した機能です。Google 検索でもできますがそれを簡単にできるようにしたもの。
下記のページを大変参考にさせていただきました。

ref.) [Jekyll Part 07: Adding a custom Google search › Justin James](http://digitaldrummerj.me/blogging-on-github-part-7-adding-a-custom-google-search/)


#### ページ切り替え機能を追加する

記事が増えるとひたすら縦に長いホームページになってしまうので適当な量の記事を表示したらあとは**「古い記事へ」「新しい記事へ」**というボタンをクリックすることで読み進められる形にしたいと思いました。

ref.) [JekyllのPagination設定（2013年11月30日追記） — Genji App Blog](http://genjiapp.com/blog/2013/11/17/jekyll-pagination.html)

思ったより修正箇所が多くちょっと時間かけてしまいました。
index.html の中で以下の部分を site.posts -> paginator.posts に変える必要があります。

```sh
  {\% for post in paginator.posts %}
```


#### 生成URLに日付を含める

jekyllの各記事はファイル単位で作成するのですがファイル名は日付+タイトルで初期設定ではURLにはタイトルだけ使われるようになっていました。例えば、ファイル名が **2016-3-31-Hello-World.md**だとすると、URLは**https://example.com/Hello-World/** となります。今後大量に記事を作った場合日付以降のファイル名がおなじになる可能性があり、その場合どうなるんだろうと試したら上書きされてしまいました。。。
そのため、日付をURLに含めることで日付以降の名称が同じであっても別ページになるようにしようというのが今回の修正です。

_config.yml の以下の項目(permalink)を修正して日付を追加します。

```sh
   permalink: /:year/:month/:day/:title/
```

ref.) <https://github.com/sunnadimpalli/sunnadimpalli.github.io/blob/master/_config.yml>



#### インターナルリンクを利用する

いまは上の日付をURLに含めることで解決しましたが、ファイル名を何かの理由で変更した場合に過去の内部リンク参照が切れてしまうのは嫌だなと思ったので調べたらこの方法で解決しました。
外部サイトからのリンクは対応できませんがそれは仕方ないですね。

ref.) <http://stackoverflow.com/questions/4629675/jekyll-markdown-internal-links>

```sh
    [Some Link]({\% post_url 2010-07-21-name-of-post %})
```

※ markdownのタグ解釈を防ぐため%の前にバックスラッシュを入れています。回避方法が分からない。。


