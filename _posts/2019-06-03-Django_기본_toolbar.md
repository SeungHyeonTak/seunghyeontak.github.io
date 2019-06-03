---
layout: post
title:  "Django_기본.django_toolbar"
comments: true
description: "Django"
author: SeungHyeon Tak
date:   2019-06-03 17:49:11 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.기본 웹 개발

* django_toolbar 적용법


```
# toolbar 설치
$ pip install django-toolbar
```
<br>

```
# settings.py에 추가
INSTALLED_APPS = [
    # ...
    'debug_toolbar',
]

MIDDLEWARE = [
    # ...
    'debug_toolbar.middleware.DebugToolbarMiddleware',
]

INTERNAL_IPS =['127.0.0.1']

```
<br>

```
# urls.py에 추가
from django.conf import settings

if settings.DEBUG:
    import debug_toolbar
    urlpatterns = [
        path('__debug__/', include(debug_toolbar.urls)),

    ] + urlpatterns
```
