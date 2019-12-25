---
title: "Swift Location"
date: 2018-12-16T00:26:13+09:00
draft: true
categories:
- Swift
tags:
- Swift
- Location
- GPS
keywords:
- スウィフト
- iPhone
- iOS
- 位置情報
---

Swift 4.2
Xcode 10.1



3種類あるよう．

| Service                             | Description                                                  |
| ----------------------------------- | ------------------------------------------------------------ |
| Visits location service             | 電池効率が良い．そのまま使うより，例えば位置をもとに推薦を行うなど，他機能に転用することが推薦されている．常に位置情報を使用する許可が必要． |
| Significant-change location service | GPSによる位置情報を電池効率よく取得する．常に位置情報を使用する許可が必要． |
| Standard location service           | 汎用的な位置情報取得の選択肢．他二つより電池消耗が激しい．常にまたはアプリ使用中に位置情報を使用する許可が必要． |

参照：https://developer.apple.com/documentation/corelocation/getting_the_user_s_location



https://developer.apple.com/documentation/corelocation/getting_the_user_s_location/using_the_standard_location_service





https://www.youtube.com/watch?v=WPpaAy73nJc

https://qiita.com/mchan3in/items/b4e07144b40c3efa2441

https://i-app-tec.com/ios/mapkit-userlocation.html