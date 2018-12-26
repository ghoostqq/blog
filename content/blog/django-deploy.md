---
title: "Djangoを本番環境にデプロイする注意点"
date: 2018-11-29T11:37:52+09:00
draft: true
tags: 
- Django
- Deploy
- デプロイ
categories:
- Django
series: []
---

Djangoを本番環境にデプロイする時に注意すべき設定をまとめました．

https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/#run-manage-py-check-deploy

```terminal
./manage.py check --deploy
```



https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/#static-root-and-static-url

> [`STATIC_ROOT`](https://docs.djangoproject.com/en/2.1/ref/settings/#std:setting-STATIC_ROOT) and [`STATIC_URL`](https://docs.djangoproject.com/en/2.1/ref/settings/#std:setting-STATIC_URL)[¶](https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/#static-root-and-static-url)
>
> Static files are automatically served by the development server. In production, you must define a [`STATIC_ROOT`](https://docs.djangoproject.com/en/2.1/ref/settings/#std:setting-STATIC_ROOT) directory where [`collectstatic`](https://docs.djangoproject.com/en/2.1/ref/contrib/staticfiles/#django-admin-collectstatic) will copy them.

> See [Managing static files (e.g. images, JavaScript, CSS)](https://docs.djangoproject.com/en/2.1/howto/static-files/) for more information.

https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/#templates

> Enabling the cached template loader often improves performance drastically, as it avoids compiling each template every time it needs to be rendered. See the [template loaders docs](https://docs.djangoproject.com/en/2.1/ref/templates/api/#template-loaders) for more information.

