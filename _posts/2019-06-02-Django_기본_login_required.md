---
layout: post
title:  "Django_기본.@login_required"
comments: true
description: "Django"
author: SeungHyeon Tak
date:   2019-06-02 19:39:00 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.기본 웹 개발
<br>
<br>
#### @login_required

* Django에서 사용자 인증 여부를 확인하는 2가지 방법
 - 데코레이터(Decorator)를 이용한 구현 방법 : `함수형 뷰`에서 사용
   - 간편하게 @login_required를 써주는것만으로도 모든 작업이 끝난다.(클래스뷰로는 사용 불가)

```python
from django.contrib.auth.decorators import login_required
 
@login_required # 사용자 인증 여부 확인 (로그인 한 사람만 인증)
def my_view(request): 

```

- 믹스인(Mixin)을 이용한 구현 방법 : `클래스형 뷰`에서 사용

```python
from django.contrib.auth.decorators import login_required

# 믹스인 구현
class LoginRequiredMixin(object):
    @classmethod
    def as_view(cls, **initkwargs):
        view = super(LoginRequiredMixin, cls).as_view(**initkwargs)
        return login_required(view)
 
# 구현한 믹스인 가져다 쓰기
class PhotoCreateView(LoginRequiredMixin, CreateView):
    model = Photo
    fields = ['album', 'title', 'image', 'description']
    success_url = reverse_lazy('photo:index')
```

원래는 이렇게 사용했는데, `Django 1.9`부터는 가져다 쓸 수 있게되었다.

```python
from django.contrib.auth.mixins import LoginRequiredMixin
 
#### 바로 위 코드와 비교해보자. 아주 간단해졌다.
class MyView(LoginRequiredMixin, View):
    login_url = '/login/'
    redirect_field_name = 'redirect_to'
```
<br>
