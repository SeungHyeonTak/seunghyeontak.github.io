---
layout: post
title:  "Django_14.(설문조사 만들기)Setup 및 urls와 views.py 설정"
comments: true
description: "Django_part14"
author: SeungHyeon Tak
date:   2019-04-30 20:21:10 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.14 웹 개발

* `새로운 앱을 만들기에 앞서 기본적인 셋팅을 다 했다는 가정하에 진행 하겠습니다.`

```
$ pip3 install django # 장고 생성
$ django-admin startproject '생성한 프로젝트 name' # 프로젝트 생성
ex) django-admin startproject config .
$ python manage.py migrate # 데이터베이스 초기화 설정
$ python manage.py createsuperuser # DB 관리자 계정 생성
$ python manage.py startapp '앱 name' # app 생성
ex) python manage.py startapp polls
```


* `settings.py`부분  내용 추가 및 변경 하기

```
INSTALLED_APPS = [
	...,
    'polls', # 내용 추가 하기(사용자가 생성한 app을 추가)
]

LANGUAGE_CODE = 'ko-kr' # 한글로 보고싶다면 내용 변경하기 (※기존엔 영어)
TIME_ZONE = 'Asia/Seoul' # 한국 기준 시간으로 변경
```

* `Views.py`에서 보여질 내용 추가하기

```
from django.http import HttpResponse
def index(request):
    return HttpResponse("Hello, CodeSquad")
```

그리고 생성한 app에 `urls.py`라는 파일을 생성 해준다.

* 프로젝트에 urls.py가 기본적으로 있지만 app 부분에는 urls.py가 없다.

`polls/urls.py`

```
from django.urls import path, include
from . import views

urlpatterns = [
    path('',views.index, name = 'index'),
]
```

`config/urls.py`

```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('polls/',include('polls.urls'))
]
```

이후 접속해서 python manage.py runserver를 돌려 확인해본다.
127.0.0.1:8000/polls 로 들어가서 확인한다.