---
layout: post
date: 2016-10-07
title: "jekyll で非公開ファイルを登録する"
categories: Jekyll Draft Post
tags: [jekyll, knowledge]
excerpt: this is a sample sentence to explain this post summary.
timezone: Asia/Tokyo
---

## preface / background

jekyll を使ってたまに雑文をPostしてますが、頭に浮かんだアイデアを書き留めておきたいときがあります。
「あ、○○をテーマにした記事を書きたい」とか、
「いまやってるこれがうまくいったらまとめよう」とか。
ただ、git でcommit してしまうと、そのまま外部に公開されてしまうため、中途半端な記事の状態ではgit 登録しづらいと感じていました。
ローカル内で管理するやり方をすると、本来のgit の良さを失うというか特定のマシン上でしか作業ができなくなります。

で、ドラフト版として作成する機能がないかとちょっと調べたらあっさり見つかったのでそれに関する記事です。

## ドラフト作成方法

### _draft フォルダの作成

ルートフォルダに"_draft" フォルダを作成します。
このフォルダの中に作ったファイルは基本的にjekyll の動作に影響を与えません

### ドラフト文書の確認

確認したいときにはオプションを付けて、jekyll を実行します。

````sh
$ jekyll serve --drafts
````


> 参考: [Example of using drafts in Jekyll]()
> 参考: [Working with drafts](https://jekyllrb.com/docs/drafts/)