---
title: "Chainer meetup 3"
date: 2018-06-09T13:46:07+09:00
draft: true
tags: 
- Chainer
- Chainer Meetup
categories: 
- Chainer
series: []
---

# 1st Presentation.

- CuPy
- Caffe
- iDeep -> めちゃ速くなる

```python
chainer.Sequential(
  layer1,
  layer2,
  )
```
というイメージで実行できる。Kerasぽい操作らしい？
[cf.](http://musyoku.github.io/2017/10/15/chainer-nn/)

Caffeにモデルぶっこんで出力するのかな。

@cupy.fuse()
足し算とかけ算を一つの演算で行えるようになる。

# 2nd Presentation.

[Chainer UI](https://github.com/chainer/chainerui)

# 3rd Presentation.

3つの仕事を行う
- 人の言葉から意味を理解する自然言語処理
- 現実世界の状況に基づいて行動
- ?

# 休憩
# 4th? Presentation
by　レトリバ

音声認識

Kaldiが用いられてきたが、DL登場によりKaldi+Chainerの魔改造など行われる
だが、Encoder-decoderモデルの登場
ワークフローは「Kaldiで特徴量抽出→Chainer, Pytorchで深層学習」

# 5th Presentation
by　アルパカ
金融情報からの予測

リアルタイムに処理するためにはChainerに処理可能な形に整理することがボトルネックになりうる。
そこで、DBを自社開発しており、Numpyデータフレーム配信サーバを作っている。
[MarketStore](https://github.com/alpacahq/marketstore)


# LT
合成MNIST
人間の学習は複数者のを見比べて行われるのではないか。
