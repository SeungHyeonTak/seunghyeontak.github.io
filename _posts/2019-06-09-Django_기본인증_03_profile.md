---
layout: post
title:  "Django_기본인증_03_Profile 구현"
comments: true
description: "Django"
author: SeungHyeon Tak
date:   2019-06-09 12:44:11 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.로그인 인증 프로젝트 구성

#### Profile 만들기

#### Profile url 생성

```python
# accounts/urls.py

from . import views

app_name = "accounts"

urlpatterns =[
    path('login/', LoginView.as_view(template_name="accounts/login.html"), name='login'),
    path('profile/', views.profile, name='profile'),
]
```

<br>

#### view 추가

```python
# accounts/view.py

from django.shortcuts import render
from django.contrib.auth.decorators import login_required

@login_required
def profile(request):
    return render(request, 'accounts/profile.html')

# login_required는 로그인 여부를 검사하여 접근을 통제할 수 있게 해준다.
# 위에서(함수형 뷰) 사용한 방법이 데코레이터를 이용한 방법이고
# 믹스인이라는걸 이용해서 구현하는 방법이 있는데 그건 클래스형 뷰에서 사용가능하다
```

<br>
#### Template 추가

![12314141](https://user-images.githubusercontent.com/46446165/59154666-9f200b80-8ab2-11e9-9e62-2beed5dda7e7.png)
