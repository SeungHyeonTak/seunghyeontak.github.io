---
layout: post
title:  "Django urls과 view"
comments: true
description: "Django url, Django view"
author: SeungHyeon Tak
date:   2019-04-22 23:50:11 +0700
categories: [Django]
tags: [Django]
keywords: "Django url, Django view"
---
#### Django url과 view에 대해

Django urls.py & view.py

`mysite/views.py`

```
from django.shortcuts import render
from django.http import HttpResponse
# HttpRequest : 요청에 대한 메타정보를 가지고 있는 객체
# HttpResponse : 응답에 대한 메타정보를 가지고 있는 객체

# 만들고자 하는 객체를 생성 한다.
def index(request):
    return HttpResponse("<h1> Hello, World!<h1>")

def site(request):
    return HttpResponse("오늘 저녁은 돈가스!")
```

* django 1.x -> django 2.x 로 버전이 변경됨에 따라
* url 쓰는법도 변경되었습니다. (url -> path)
* 그러므로 생성한 앱에 urls.py를 생성하고 프로젝트에 있는 urls.py로 옮겨와 include()할 수 있는 방법이 생겼다.

`mysite/urls.py` mysite 폴더 안에서 urls.py는 없기 때문에 생성 해줘야 한다.

```
from django.urls import path
from . import views

urlpatterns = [
	path('',views.site, name = "site")
	# mysite의 post_list라는 views가 URL에 할당됨
	path('hello',views.index, name = "hello")
	# 뒤에 name을 붙이는 이유는 뷰의 이름이 같을 수도 있기 때문에 URL에 고유한 이름을 붙이는것이라고 합니다.
]
```

`config/urls.py`

```
# urls.py는 장고에서 특정 url에 대한 요청을 어떤 함수나 클래스로 매핑해서 연결시켜주는 역할을 함
from django.contrib import admin
from django.urls import path, include
from mysite import views # mysite안의 views를 쓰겠다.

urlpatterns = [
	path('admin/', admin.site.urls),
    	path('',include('mysite.urls')),
	# 빈문자열에 매칭됨
]
```
