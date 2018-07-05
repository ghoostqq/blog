---
title: "Twitter Oauth"
date: 2018-06-30T18:07:31+09:00
draft: true
---

Twitter OAuth認証のまとめ。
認証にはApplication OnlyとAccess Tokenを利用する2種類がある。
<!--more-->

# URLs
[https://apps.twitter.com](https://apps.twitter.com)
自分のアプリ管理

## OAuth tools
**Using Python**
- Python OAuth2
[**Wiki Example**](https://github.com/joestump/python-oauth2/wiki/Logging-into-Django-w--Twitter)
主にこれを参照して認証を組んだ。

- Python rauth


[Authenticate vs Authorize](https://stackoverflow.com/questions/9613455/twitter-api-authenticate-vs-authorize)


# Web login with Twitter

デフォルトだとPIN認証になっている
`Callback_url`をapp.twitter.comの設定で追加する必要がある。
[Twitter/Callback Url](https://developer.twitter.com/en/docs/basics/callback_url.html)
デフォルトだとこの値は`oob`（意味はよくわからない）になっていて、oobだとPIN認証。

さらに、`access_token`に`?oauth_callback`を同じurlで投げる必要がある。[ref](http://pronama.azurewebsites.net/2018/06/13/twitter-oauth-callback-url/)
そうすることで
```json
{
  'oauth_callback_confirmed': 'true',
  'oauth_token': '-dKLcAAAAAAA625TAAABZFBVOsU',
  'oauth_token_secret': 'aA6JPg0A4BHZnnL8bWUOGFLcdkaEmWcO',
}
```
のような結果が帰ってくる。
