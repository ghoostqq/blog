---
title: "Django staticの構成 scssと共に"
date: 2018-11-29T10:52:53+09:00
draft: true
tags: 
- Django
- Sass
categories:
- Django
keywords: 
- static
- Django 構成
---



https://docs.djangoproject.com/en/2.1/howto/static-files/

```python
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, "static"),
    '/var/www/static/',
]
```

など



https://docs.djangoproject.com/en/2.1/ref/contrib/staticfiles/

設定周りとか詳しく書かれている．

[`STATIC_ROOT`](https://docs.djangoproject.com/en/2.1/ref/settings/#std:setting-STATIC_ROOT)

[`STATIC_URL`](https://docs.djangoproject.com/en/2.1/ref/settings/#std:setting-STATIC_URL)

[`STATICFILES_DIRS`](https://docs.djangoproject.com/en/2.1/ref/settings/#std:setting-STATICFILES_DIRS)

これは`STATIC_ROOT`と同じになってはいけない．ここからファイルを集めて`STATIC_ROOT`に置くのだから．

[`STATICFILES_STORAGE`](https://docs.djangoproject.com/en/2.1/ref/settings/#std:setting-STATICFILES_STORAGE)

[`STATICFILES_FINDERS`](https://docs.djangoproject.com/en/2.1/ref/settings/#std:setting-STATICFILES_FINDERS)





# Django sass processor

https://github.com/jrief/django-sass-processor



---

# 1. [`STATIC_ROOT`と`STATIC_URL`を用いる設定](https://docs.djangoproject.com/ja/2.1/howto/static-files/deployment/#serving-the-site-and-your-static-files-from-the-same-server)

最も基本的な設定．

`STATIC_ROOT`に設定したディレクトリに，`collectstatic`コマンドでファイルが整備されるので，そのディレクトリを配信するようにサーバを設定する．そのためのURLは`STATIC_URL`で設定する．

- `STATIC_ROOT`: 静的ファイル用のサーバへ移動する、収集 (collect) された静的ファイルのディレクトリ



# 2. [クラウドサービスやCDNを用いる](https://docs.djangoproject.com/ja/2.1/howto/static-files/deployment/#serving-static-files-from-a-cloud-service-or-cdn)

`STATIC_STORAGE`を使うことになる．

