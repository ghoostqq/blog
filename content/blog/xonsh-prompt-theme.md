---
title: "Xonsh prompt theme"
date: 2018-11-25T01:54:01+09:00
draft: true
tags: 
- Xonsh
- Shell
- Python
categories: 
- Xonsh
---



## xonsh概観

Pros

インストールが簡単

デフォルトでリッチな機能



使用例

```python
$ for s in [i.lstrip() for i in $(git branch --merged).split('\n')]:
.     if (s != '') and (s[0] != '*') and (s != 'master'):
.         $(git branch -d @(s))
.
```

シェル芸は得意ではありませんが，Pythonなのでこんな処理がサラッと書けてしまいます．

（もっといいやり方があればこっそり教えてください）



Cons （わかりにくいところ）

historyが思ってる動きをしなくて最初わからない

xonshのhistoryは多数のセッション？をつなぐことを最初から想定しており，デフォルトでhistoryを叩いても接続中のセッションの履歴しか表示されない．

historyはhistory showのエイリアスで，history show allすると今までの全履歴を表示してくれる．

Cf: document https://xon.sh/tutorial_hist.html





---

fishで愛用しているディレクトリ表示方法．

ソースを見てみるとgitコマンドを利用していた．

```fish
set -l project_pwd (command git rev-parse --show-prefix ^/dev/null | string trim --right --chars=/)
set -l work_dir (command git rev-parse --show-toplevel ^/dev/null)
```

xonshはPythonで処理部分を書いてるっぽい．

`builtins.__xonsh__.env["PWD"]`とかの処理までは現時点で辿ってみていないが，`_collapsed_pwd`なんかはディレクトリ表示の処理をPythonで書いているのがわかる．

https://github.com/xonsh/xonsh/blob/master/xonsh/prompt/cwd.py





公式ドキュメントhttps://xon.sh/tutorial.html#customizing-the-prompt

`$PROMPT`で文字列を設定

`$PROMPT_FIELDS['your_field'] = lambda: 'foobar'`で`$PROMPT`に表示可能なフィールドを設定できる．



公式のpromptのソースコードhttps://github.com/xonsh/xonsh/tree/master/xonsh/prompt



ばんくしさんのxonshrcに使える環境変数まとめhttps://vaaaaaanquish.hatenablog.com/entry/2017/12/09/154201





一つ前のコマンドの結果をチェックする方法

https://github.com/xonsh/xonsh/issues/1146

`__xonsh_history__.rtns[-1]`との回答だが `__xonsh_history__.warn()`によれば `__xonsh__.history`に変更になっている．



