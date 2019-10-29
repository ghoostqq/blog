---
title: "初めての自作PCレポ"
date: 2019-10-29T09:46:24+09:00
draft: false
categories: 
- 自作PC
tags:
- Linux
- NVIDIA
keywords:
- 自作PC
- GPU
- NVIDIA
footnotes: true
---

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">沼に両足ドボン！ <a href="https://t.co/FtJ5oFmT0G">pic.twitter.com/FtJ5oFmT0G</a></p>&mdash; qqhann🦊 (@qqhann) <a href="https://twitter.com/qqhann/status/1185025110983315456?ref_src=twsrc%5Etfw">October 18, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

朝の11時くらいに部品が届き、夜の2時まで作業を続けていました。

知らないことを調べながらということもあるのですが、GPU設定がわからず6時間くらい溶かしたというのが大きいです。

## 最小構成での動作確認

最初の数時間はこれをやりました。

まずCPUを取り付け、CPU付属のファンで簡易に取り付けを行います。

メモリとSSDを取り付けます。SSDはM.2規格なのでマザボに直接つけるタイプです。

電源を接続していざ！

電源ショートのスイッチを持っていなかったので手に持っていた工具の金属部分でショートさせました。

荒っぽいですが、BIOS起動成功です。

## Linuxのインストールイメージを作成

イメージ自体はダウンロードするだけですが、USBに書き出すのにすこし手間取りました。

ddコマンドでフォーマットしました．

https://linoxide.com/linux-how-to/create-bootable-ubuntu-usb-flash-drive-terminal/

## PCケースに最小構成を取り付ける

取り付けは結構楽しかったです。

虎徹Mark2にこのタイミングで付け替えです。

CPUグリスはどっちが効率いいかわからなかったですが、いちおう拭き取って新しくつけ直しました。量や塗り方がわからなくて迷いました。

虎徹Mark2は結構グリスつけちゃうと真空圧力？かなにかでぴったり離れなくて戸惑いました。

ちなみに温度は35〜36で多分普通ですね。

付け替えたことでメモリの物理干渉がなくなってつけ直しました。

AsrockにM.2のヒートシンク？が付属していたので、シールを剥がしてそれをつけるかどうかかなり悩みました。結局、必要を感じるまでは保証用シールを残しておくことにして裸のままにしました。

## GPUを取り付けて、認識させる

GPUが思ったより大きくてビビりました。

ケースのファンと物理干渉するので、後からファンの位置を調整しました．

imsのGPUとして買ったのですが、付属の簡易セットアップの説明書には6ピンと8ピンを電源に接続すると書いてありました。しかし実際にGPUが受け付けるピンは6/8/8でした。[^1][^2]

自分はこのPCIE電源に対する理解がなく、「あとでブリッジするために残りの受付口はあるのかな？」と勘違いして1つの給電口から伸びるPCIE電源を6/8ピンに指したのです。

6時間を溶かしたあとようやく、3つの給電口から6/8/8全てに指して無事GPUが正しく認識されるようになりました。（正しく指してない段階でLEDがなまじひかるのがわるい）

[^2]: 補助電源の参考サイトhttps://gpccoming.com/graphic-board/auxiliary-power-with-checking-pin-cable-and-supply-electric-energy.html
[^1]: NVIDIA User Guide https://www.nvidia.com/content/geforce-gtx/GEFORCE_RTX_2080Ti_User_Guide.pdf

### TL;DR

結論からいうと正しい（であろう）手順はこうです：

1. 最小構成でOSインストールまで済ませる。
2. BIOSで利用グラフィックボードをオンボード優先にする。
3. GPUを取り付ける。
4. OSを起動する（オンボードのグラフィックボード）
5. NvidiaのサイトでGPUに合ったnvidia-driverのバージョンを確認する（そこから落とせる.runのシェルスクリプトはうまく動かなかった）
6. 手順（後述）に従ってaptでnvidia-driverをインストールする。
7. 再起動して、nvidia-smiなどを確認
8. BIOSで優先グラフィックボードをPCIEに切り替える
9. HDMIなどをGPU出力に差し替えて、再起動
10. 動く！

以上です！（この記事はこれをメモするためだけに書いたようなものかもしれない）

### Nvidia-driverのインストール

NVIDIAドライバーのインストールは，公式の方法だとうまくいかないので，aptで入れました．

https://www.mvps.net/docs/install-nvidia-drivers-ubuntu-18-04-lts-bionic-beaver-linux/

## 配線整備などして完了

かんたんに書きましたがここでもかなり時間がかかっており大工事でした。初見なので事前計画ができてなくて、やり直しが大きかったという麺もあります。🍜

GPUの嵌りでは精神的に疲弊しましたが、こちらは肉体的にクタクタになりました。15時間作業してるしね。

蓋を占めたらLEDが一切見えなくて、ちょっと寂しかった。あのピカピカは何だったのか。中で今も光っている（ない）かもしれない。シュレディンガーのGPU。

箱は保証のためにすべて保存するのですが、中の部品の箱がすべてケースの箱の中にピッタリ収まったので、中身も箱もケースに収まったという謎の感動で終えることができました。

<blockquote class="twitter-tweet" data-conversation="none"><p lang="ja" dir="ltr">配線整備して…完成！！ <a href="https://t.co/ELrccVgXtI">pic.twitter.com/ELrccVgXtI</a></p>&mdash; qqhann🦊 (@qqhann) <a href="https://twitter.com/qqhann/status/1185240564075118592?ref_src=twsrc%5Etfw">October 18, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>