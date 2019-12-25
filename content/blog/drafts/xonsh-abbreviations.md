---
title: "Xonsh Abbreviations"
date: 2018-12-12T10:43:43+09:00
draft: true
categories:
- Xonsh
tags:
- Xonsh
- Python
- Shell
- Prompt
- Fish
keywords:
- xonsh
- 省略
- OSS
---

https://vaaaaaanquish.hatenablog.com/entry/2018/11/23/000120

ptk (prompt toolkit)を理解することはxonshを理解することらしい．



ptkこれか．

https://python-prompt-toolkit.readthedocs.io/en/master/index.html



fishみたいなabbreviationが欲しいな．

スペースに反応できる必要がある．

https://en.wikibooks.org/wiki/Python_Programming/Input_and_Output

input()ではそういうのはなさそう？いや，これPython2系のinput()か．



issue出してる人がいた．

https://github.com/xonsh/xonsh/issues/2889



fishのabbreviation実装．参考までに．

https://github.com/fish-shell/fish-shell/blob/5bd04726824f97f03174438eb72b8be0b1274b85/share/functions/abbr.fish

得られたヒント：

省略形はスペースを含んではいけない



https://docs.python.org/3/library/sys.html

sys.stdin.read()が近いのかな．

EOFまで読むので，それをスペースと改行に反応するように改造する？？



cythonのソース見てもどこがそれっぽいかよくわからん

https://github.com/python/cpython/search?q=stdin+read+eof&unscoped_q=stdin+read+eof

stdin, read, eofで絞り込んでいるのだが...



https://github.com/prompt-toolkit/python-prompt-toolkit

prompt toolkitのGitHub





https://github.com/prompt-toolkit/python-prompt-toolkit/search?p=2&q=space&unscoped_q=space

https://github.com/prompt-toolkit/python-prompt-toolkit/blob/330ba727eceea98bc83dd4454d460ac4f05f86f8/examples/prompts/autocorrection.py

https://github.com/prompt-toolkit/python-prompt-toolkit/blob/7db93560cb1f10c96f4a22816774da63e67e467e/prompt_toolkit/layout/screen.py