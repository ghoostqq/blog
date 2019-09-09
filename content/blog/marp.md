---
title: "MarpでMarkdownによるスライド制作のススメ"
date: 2019-06-28T04:56:29+09:00
draft: false
tags: 
- Markdown
- slides
- OSS
categories: 
- Markdown
keywords:
- keynote
- powerpoint
series: []
---

# MarpでMarkdownによるスライド制作のススメ

＊この記事は書きかけです

スライドを作るのにまだスタイルをイジイジしてるの？　という煽りタイトルを思いついたけど思いとどまった自分を褒めたい。

スライド制作の本質は聴衆にいかにうまくアイデアを伝えるかであり、見た目のちょっとした違いではないはずだ。小さい字がびっしりで見えないなどというよほどひどいものでない限り、フォントサイズが26ptか28ptかなどは枝葉末節であり、そんなことに時間を使ったり思考を邪魔されるのは本末転倒である。

Markdownはテキストライティングのスタイリングから人々を解放してくれる。Markdownで、スライドを制作できるツールが存在する。Marpはそのようなツールの一つで、Markdownの文章をコンパイルして、あらかじめ用意されたスタイルのスライドにしてくれる。

Marpは元々デスクトップアプリとして公開されていたが、現在はMarp Nextとしてライブラリ構造を意識しつつCLIツール、VSCode extensionなどとして展開している。

# Useful links

https://marpit.marp.app/directives



### Slide Size

https://marpit.marp.app/theme-css?id=slide-size

スライドの比率設定方法

デスクトップ版は`$size`が使えるが、VSCode版ではこちらを使う必要がある。