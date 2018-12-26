---
title: "Django Restframework"
date: 2018-12-17T05:45:10+09:00
draft: true
categories:
- Django
tags:
- Django
- Django REST Framework
- DRF
- API
keywords:
- RESTful api
---

# Django REST framework





# View

## [Function Based Views](https://www.django-rest-framework.org/api-guide/views/?q=response#function-based-views)

```python
from rest_framework.decorators import api_view

@api_view()
def hello_world(request):
    return Response({"message": "Hello, world!"})
```



## [Response()](https://www.django-rest-framework.org/api-guide/responses/#response)

```python
from rest_framework.response import Response
```



## get_object_or_404 for DRF?

https://stackoverflow.com/a/42036696/8776028

https://www.django-rest-framework.org/api-guide/exceptions/

