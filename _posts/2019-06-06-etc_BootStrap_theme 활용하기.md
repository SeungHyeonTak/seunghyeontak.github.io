---
layout: post
title:  "(etc). BootStrap_Theme 활용하기"
comments: true
description: "BootStrap"
author: SeungHyeon Tak
date:   2019-06-06 23:25:00 +0700
categories: [etc]
tags: [etc]
keywords: "Bootstrap, theme"
---
#### 부트스트랩 테마 적용하기

#### 1. 적용하고 싶은 테마 찾기

<https://startbootstrap.com/themes/>
여기서 찾거나 구글에서 awsome bootstrap theme를 검색해도 된다.

#### 2. 테마 압축 풀기

받은 파일의 압축을 풀어보면 css,img,js,font,vendor 등등 들어있을것이다

#### 3. Django setting하기
기본적으로 우리가 아는 프로젝트를 생성하는 명령어를 실행한다.

```
$ pip install django
$ django-admin startproject config .
$ python manage.py migrate
$ python manage.py createsuperuser
$ python manage.py startapp blog
```
<br>

```python
# config/settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog',
]
```

* 틀 만들기

```python
# blog/views.py
from django.views.generic.base import TemplateView

class MainpageView(TemplateView):
    template_name = 'blog/main.html'

# blog/urls.py
from django.urls import path

from .views import *
urlpatterns = [
    path('', MainpageView.as_view(), name='mainpage'),
]
```
<br>

```html
# blog/templates/blog/main.html

```

![123](https://user-images.githubusercontent.com/46446165/59048545-ff4f5b80-88c0-11e9-8fbd-b30650675fbe.png)

<br>

```python
# config/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog.urls'))
]

# config/settings.py
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'layout')],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

<br>
<br>

* 테마 적용

* 파일 복사
  * Django 폴더에서 layout, static 폴더 생성
  * index 파일을 layout에 나머지를 static로 넣는다
  * index 파일로가서 doctype 밑에 `load staticfiles` 삽입해준다.


```html
link href="vendor/bootstrap/css/bootstrap.min.css" rel="stylesheet"
이렇게 되어있는 코드를
link href="{% static 'vendor/bootstrap/css/bootstrap.min.css' %}" rel="stylesheet"
```

css / img/ js / font .. 등등 압축푼 폴더에 있는 모든것들을 바꿔줘야한다

이런식으로 변경시켜주면 원하는 테마를 볼 수 있다.

마지막으로 

```python
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static')

# 이후 터미널에서 python manage.py collectstatic
# 을 실행해주고 yes를 입력하면 파일이 정리되어 static폴더로 이동하게 된다.
```

