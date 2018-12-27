---
title: "Xonshでモダンなターミナルプロンプトを1分で構築する"
date: 2018-11-25T01:54:01+09:00
draft: false
categories: 
- Xonsh
tags: 
- Xonsh
- Shell
- Python
- Advent Calendar
keywords:
- アドベントカレンダー
- コンシュ
- シェル
- プロンプト
- モダン
---



この記事は[Xonsh Advent Calendar 2018](https://qiita.com/advent-calendar/2018/xonsh) 24日分の記事です．（27日に完成版書いています．大遅刻すみません．）

記事の構成は：xonshの紹介，今回のコードを作る流れ，コードのまとめという風にしようと思います．

タイトルの通り「1分で構築」したい方は，まとめに飛んでxonshrcをコピペすると良いでしょう．



# xonsh概観

xonshはPython製のシェルです．
発音はコンシュと公式にありました．xontribはコントリブ，xonfigはコンフィグとなりしっくりきますね．

xonshを知ってからSSHして作業するときはxonshを入れるようにしています．今後も同じようにしていく気がするので，私の通った道（SSHした先）にはxonshが残されている...とか言えるようになるんじゃないでしょうか．😏

xonshのいいところ，わかりにくいところが何か，振り返ってみました．

## Pros（いいところ）

- Pythonでシェルが書ける

- インストールが簡単

- デフォルトでリッチな機能



シェルスクリプトは人類には早すぎると感じることがある中，PythonistaにとってPython文法でシェルが書けるという点だけでも，使わない手はないと思います．

`pip install xonsh`で入りますし，デフォルトで補完サジェストができたりして素晴らしいです．



### 使用例

マージ済みのブランチを削除する処理

```
$ for s in [i.lstrip() for i in $(git branch --merged).split('\n')]:
.     if (s != '') and (s[0] != '*') and (s != 'master'):
.         $(git branch -d @(s))
.
```

シェル芸は得意ではありませんが，Pythonなのでこんな処理がサラッと書けてしまいます．

（もっといいやり方があればコメントで教えてください）



## Cons（わかりにくいところ）

- historyが思ってる動きをしなくて最初わからない

xonshのhistoryは多数のセッション？をつなぐことを最初から想定しており，デフォルトでhistoryを叩いても接続中のセッションの履歴しか表示されない．

これは，`history`は`history show`のエイリアスだからです．

```
$ history show all
```

すると今までの全履歴を表示してくれます．

Cf: document https://xon.sh/tutorial_hist.html



## 総じて

使わない手はないんじゃないでしょうか．

私の運用方法としては，サーバ上ではbash上でxonshを起動して作業しています．

手元ではfishを使っていて，必要な時にxonshを立ち上げて作業します．



# モダンなプロンプトを実現する

煽れるタイトルなにかな〜と考えてひねり出したモダンなプロンプトというフレーズですが，見やすくてかっこいいプロンプトのことです．

今時はpowerlineがあり，xonsh-powerlineもあるので一発で入るのですが，見た目の細かい好みがあるかと思います．

xonshは手軽に高度なプロンプトのカスタマイズができます．その方法を見ていきましょう．

## xonshのプロンプト設定方法

`$PROMPT`で文字列を設定することでプロンプトを変更することができます．

```terminal
>>> $PROMPT = '{user}@{hostname}:{cwd} > '
qqhann@home:~ > #簡単に変更できました．
qqhann@home:~ > $PROMPT = "[ {short_cwd} ]\n$ "
[ ~/.c/xonsh ]
$ pwd
/Users/qqhann/.config/xonsh
```

このように，簡単に変更することができます．

特に`short_cwd`がおすすめです．手前味噌ですがこれの`.`で始まるディレクトリを2文字で表示するパッチを投げて，contributorになりました．OSSは楽しい．

公式ドキュメントhttps://xon.sh/tutorial.html#customizing-the-prompt



## さらに自由度の高い設定

さらに自由度の高い設定をしたい場合は，メソッドを登録して`$PROMPT`で使えるようにします．

`$PROMPT_FIELDS['your_field'] = lambda: 'foobar'`で`$PROMPT`に表示可能なフィールドを設定できます．



公式のpromptのソースコードhttps://github.com/xonsh/xonsh/tree/master/xonsh/prompt



## 目指すプロンプト

普段私が使用しているfishのプロンプトはこちら：[dollar-fish](https://github.com/qqhann/dollar-fish) （[bobthefish](https://github.com/oh-my-fish/theme-bobthefish)からアレンジしたものです）

gitディレクトリの中にいる時，base directoryまでのパスを短縮表示して，git directory内のパスをフルで表示する方式です．

このディレクトリ表示方法が気に入っているので，xonshでこれを実現したいと思います．



### どう実現するか

#### dollar-fishの方法：gitのtoplevelとprefix

ソースを改めて見てみるとgitコマンドを利用していました．

```fish
set -l project_pwd (command git rev-parse --show-prefix ^/dev/null | string trim --right --chars=/)
set -l work_dir (command git rev-parse --show-toplevel ^/dev/null)
```



#### それをどう書くか

調べたけど使わなかった雑めも．

xonshはPythonで処理部分を書いてるっぽい．

`builtins.__xonsh__.env["PWD"]`とかの処理までは現時点で辿ってみていないが，`_collapsed_pwd`なんかはディレクトリ表示の処理をPythonで書いているのがわかる．

https://github.com/xonsh/xonsh/blob/master/xonsh/prompt/cwd.py

`builtins`に`builtins.__xonsh__`が含まれている仕組みがよくわかってないので教えて強い人！



#### `$()`記法を使う

今回はこれ以上深掘りせず，xonshの`$()`記法を素直に利用することにしました．

xonshからgitコマンドを呼び出せば良さそうです．



### さらに，直前のコマンドの成否により色を変える

一つ前のコマンドの結果をチェックする方法

https://github.com/xonsh/xonsh/issues/1146

`__xonsh_history__.rtns[-1]`との回答だが `__xonsh_history__.warn()`によれば `__xonsh__.history`に変更になっている．これを使えば良さそうです．



色を変える方法で嵌ってしまいましたが，色を動的に変える場合，

`return 'GREEN'`ではなく`return '{GREEN}'`のようにするとうまくいきます．



# まとめ

完成形

```python
def _git_prefix():
    import xonsh.tools as xt

    prefix = $(git rev-parse --show-prefix).strip()
    sep = xt.get_sep()

    if len(prefix) == 0:
        return prefix
    else:
        return sep + prefix if prefix[0] != sep else prefix

$PROMPT_FIELDS['git_prefix'] = _git_prefix


def _git_toplevel_or_cwd():
    import xonsh.tools as xt
    from xonsh.prompt.cwd import _replace_home

    toplevel = $(git rev-parse --show-toplevel).strip()
    pwd = toplevel if toplevel else $PWD

    sep = xt.get_sep()
    pwd = _replace_home(pwd).split(sep)
    l = len(pwd)
    leader = sep if l > 0 and len(pwd[0]) == 0 else ""
    base = [i[0]
            if ix != l - 1 and i[0] != '.' else i[0:2]
            if ix != l - 1 else i for ix, i in enumerate(pwd) if len(i) > 0]
    return leader + sep.join(base)

$PROMPT_FIELDS['git_toplevel'] = _git_toplevel_or_cwd


def _success_color():
    if __xonsh__.history.rtns and __xonsh__.history.rtns[-1] != 0:
        return '{#cc6666}'  # red
    else:
        return '{#f0c674}'

$PROMPT_FIELDS['success_color'] = _success_color

$PROMPT = "{INTENSE_BLACK}[ {INTENSE_WHITE}{git_toplevel}{INTENSE_BLACK}{git_prefix} ]\n{success_color}${WHITE} "


```

このコードをxonshrcにコピーして快適なプロンプトを始めましょう♪



---

初めてアドベントカレンダーなるものを書きました．いかがだったでしょうか．どうぞ気軽にコメントください．

[Twitterもやっている](https://twitter.com/qqhann)のでフォロー頂けると嬉しいです．

それでは，クリスマス過ぎちゃったけど，みなさん良いお年を．

