---
title: "Flask Quick Start"
date: 2018-12-08T00:28:24+09:00
draft: true
categories:
- Web
tags:
- Flask
keywords:
- Quickstart
---

http://flask.pocoo.org



```python
from flask import Flask

app = Flask(__name__)


@app.route('/')
def index():
    return 'Hello world'

```



```shell
env FLASK_APP=app.py flask run
```

