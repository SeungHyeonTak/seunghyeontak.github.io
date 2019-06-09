---
layout: post
title:  "Django_기본인증_04_Logout 기능구현"
comments: true
description: "Django"
author: SeungHyeon Tak
date:   2019-06-09 12:52:11 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.로그인 인증 프로젝트 구성

#### 로그아웃 구현
<br>
#### logoutview 추가

```python
# accounts/urls.py

from django.urls import path
from django.contrib.auth.views import LoginView,LogoutView
from . import views

app_name = "accounts"

urlpatterns =[
    path('login/', LoginView.as_view(template_name="accounts/login.html"), name='login'),
    path('logout/', LogoutView.as_view(), name='logout'),
    path('profile/', views.profile, name='profile'),
]
```

<br>

#### base.html/logout 추가

![1qcsa2](https://user-images.githubusercontent.com/46446165/59154769-0939b000-8ab5-11e9-82d9-b871fa5b0add.png)

* onclick는 로그아웃을 할때 javascript 기능을 추가하여 마지막으로 다시 물어보는 과정을 추가 하였다.