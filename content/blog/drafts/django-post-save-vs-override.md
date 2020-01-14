---
title: "Django post save vs override"
date: 2018-08-23T14:48:56+09:00
draft: true
tags: 
- Django
categories: ["django"]
series: []
---



## `post_save`
https://stackoverflow.com/questions/13014411/django-post-save-signal-implementation
https://docs.djangoproject.com/en/2.1/ref/signals/#post-save

## override `save()`
https://stackoverflow.com/questions/4269605/django-override-save-for-model


# Difference
When using `post_save`, you have to save again after doing your things.
The pros is it grants you a clearer vision when applying to multiple models (probably).

On the other hand, overriding `save()` has pros that you don't have to call save again.
