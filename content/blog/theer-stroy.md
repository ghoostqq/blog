---
title: "Hugoのテーマ「Theer」を作成しました"
date: 2018-07-05T21:55:18+09:00
draft: false
---

GitHubで使われているPrimerのスタイルに基づいて、Hugoのテーマ「Theer」を作成しました。
GitHubのReadmeのようにシンプルで読みやすく、さらにブログ的な要素としてヘッダとTOC（目次）が表示できます。
Hugoの使い方とともに紹介します。
<!--more-->

## ソースコード
こちらで公開しています：
https://github.com/ghoostqq/theer

このブログもTheerが使われています。

## Hugoとは
そもそもHugoとは、Go言語で書かれた静的Webサイトジェネレーターです。  
GitHubに静的サイトを公開する方法としてJekillなどが知られますが、Hugoもそれと同様のものになります。  
記事をMarkdown形式で簡単に編集することができ、コードブロックも自動で解釈して表示してくれます。

[公式チュートリアル（英語）](https://gohugo.io/getting-started/quick-start/)に則り、Hugoの使い方を紹介します。

### 1. Hugoのインストール

Homebrewでインストールします。
```terminal
brew install hugo
```

### 2. サイトの作成

好きな名前をつけましょう。
```terminal
hugo new site myWebSite
```

### 3. テーマを追加

手前味噌ですが、Theerを追加しましょう。
```terminal
cd myWebSite
git init
git submodule add https://github.com/ghoostqq/theer.git themes/theer

echo 'theme = "theer"' >> config.toml
```

### 4. コンテンツを追加する

ブログとして使うもよし、ポートフォリオとして使うもよし。
使い方は自由です。  
投稿を作成するには次のコマンドを使います。

```terminal
hugo new posts/my-first-post.md
```

サーバをローカルで起動して変更を確認できます。
`-D`オプションはDraft、つまり下書きを含めてプレビューするモードを指示します。
```terminal
hugo server -D
```
`Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)`などと表示されるはずなので、ブラウザで`http://localhost:1313/`にアクセスしましょう。

### 5. ...
以上で終わり！
自分だけのコンテンツを配信しましょう！
