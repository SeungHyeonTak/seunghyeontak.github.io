---
layout: post
title:  "Django_기본. Email Login 기능 추가하기"
comments: true
description: "Django"
author: SeungHyeon Tak
date:   2019-06-20 09:30:21 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.기본 웹 개발

#### Email Login 기능 추가하기
django는 기본적으로 username으로 로그인 하는 기능을 제공한다.
<br>
* accounts 앱에 `backends.py` 파일을 추가하고 코드를 입력한다.

```python
from django.contrib.auth.backends import MOdelBackend
from django.contrib.auth import get_user_model

class CutomUserBackend(ModelBackend):
	def authenticate(self,request,username=None, password=None, **kwargs):
		user = super().authenticate(request, username, password, **kwargs)
		if user:
			return user
		
		UserModel = get_user_model()
		if username is None:
			username = kwargs.get(UserModel.USERNAME_FIELD, kwargs.get('email'))
		try:
			uesr = UserModel._default_manager.get(email=username)
		except UserModel.DoesNotExist:
			UserModel().set_password(password)
		else:
			if user.check_password(password) and self.user_can_authenticate(user):
				return user
```

* 새로 만든 백엔드 클래스를 기본 인증 백엔드 클래스로 추가한다. settings.py에 내용 추가

```python
AUTHENTICATION_BACKENDS = [
	'accounts.backends.CustomUserBackend'
]
```