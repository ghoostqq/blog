---
title: Puppeteer 1.0対応 ヘッドレスChrome逆引きクイックスタート
layout: post
---
先日[Puppeteer 1.0.0][Puppeteer]がリリースされ、ちょっと話題になっていたのと、ちょうどHeadlessChromeが気になっていたので、触ってみました。
バージョンアップによってAPIがすでに数ヶ月前のものと相違が出ており、ここで最新版に基づいて使い方をざっくりまとめてみようと思います。
使い始める際に概要を掴むためのクイックスタートに役立てば幸いです。
<!--more-->

## レファレンス
[puppeteer API][puppeteer API]
[puppeteer release][puppeteer release]


## インストール
```shell
mkdir try-puppeteer
cd try-puppeteer
npm i --save puppeteer
```

## テンプレート
読み込みから終了までのだいたいのテンプレートです。

```js
const puppeteer = require('puppeteer');

// 非同期処理
(async() => {
  
  // 非同期処理で前後の順番を保つには、awaitをいちいち使う
  const browser = await puppeteer.launch();  // ブラウザをHeadlessで起動
  const page = await browser.newPage();  // このpageに

  // * * *
  // pageに対して操作する
  // * * *

  browser.close();  // ブラウザを閉じる
})();
```
「pageに対して操作する」の前後は起動から終了までのテンプレートだと考えていいと思います。
確認等のためHeadlessモードを解除したい場合は

```js
puppeteer.launch({headless: false});
```
と指定してあげればウィンドウを開いて描画してくれます。
次からは目的別の操作です。

## Page操作

### URLを指定して開く
```javascript
await page.goto('https://twitter.com/qiugits');
```

### キーボード入力する
```javascript
await page.type('input[name="code"]', 'Hello');
```
第一引数はセレクタです。古いバージョンではまずフォーカスしてからtypeしていましたが、1.0.0では一つのコマンドで実行できます。

### クリックする
```javascript
await page.click('input[name="button"]');
```

### 読み込みを待つ
```javascript
await page.waitFor('input');
```
waitForはセレクタ、秒数、スクリプトのいずれかを受け取ることができ、それぞれの個別のメソッドもあります。
この場合、inputタグが現れるまで待つ、ということになります。

#### 複数の読み込みを待つ
```javascript
await page.waitFor('.success, .error');
```
わかってみればなんということないですが、`.success`と`.error`のどちらかが読み込まれるのを期待する場合、このように書くことができます。賢い！
参照：[page.waitFor](https://github.com/GoogleChrome/puppeteer/issues/709)

### セレクタに当てはまる要素の有無を確認
```javascript
if (await page.$('div.errmsg')) { /* do something */ }
```
自分はif分に投げるために使ってました。
複数の場合はドルを二つにします`$$`。

### セレクタに当てはまる複数要素を処理
```javascript
if (await page.$$eval('input', inputs => inputs.length) == 2) { /* do something */ }
```
この場合、inputsが2つあることを確認するのに使えます。

### 要素の内容を文字列として取得
```javascript
const out = await page.evaluate(() => document.querySelector('.errmsg').innerText);
console.log(out);
```
この方法がなかなか見つけ辛くて苦労しました。
`.innerText`と`.textContent`という2通りの方法があります。
前者の方が基本的に読みやすいテキストが帰ってくるのに対して、後者は空白をそのまま返してくるので、適宜使い分けてください。
参照：[page.select](https://github.com/GoogleChrome/puppeteer/issues/489)

### スクリーンショット
```javascript
await page.screenshot({path: './screenshot.png', fullPage: true});
```
スクリーンショットを保存します。
できるだけ絶対パスを投げてあげると良いでしょう。


---
自分が使っていくに従ってアップデートはするつもりですが、含めて欲しい・含んだ方がいいよというもの、あればコメントください。  
初稿：[Qiita](https://qiita.com/qiugits/items/ad1e2de07237ced141c4)



[Puppeteer]: https://github.com/GoogleChrome/puppeteer
[puppeteer API]: https://github.com/GoogleChrome/puppeteer/blob/master/docs/api.md#pageselectselector-values
[puppeteer release]: https://github.com/GoogleChrome/puppeteer/releases
