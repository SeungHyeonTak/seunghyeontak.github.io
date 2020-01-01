---
layout: post
title:  "Django AbstractUser 만들기"
comments: true
description: "Django AbstractUser"
author: SeungHyeon Tak
date:   2019-10-09 19:55:13 +0700
categories: [Django]
tags: [Django]
keywords: "Django AbstractUser"
---
## Django User 모델 확장(AbstractUser 사용)

* 장고 내장 User 모델을 사용해오다가 User 모델을 확장해야하는 이유??

> AbstractUser
- Django의 내장 User모델이 제공하는 필드 외에 다른 필드 등을 추가하고 싶을 때를 위해서
  - 내 입맛대로 User모델 커스텀하기 위해서 (ex. 성별, login할때 username말고 email이나 다른 방법으로 로그인 하고싶을때)

> AbstractUser Sample code

```python
# models.py

from django.db import models
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
	...
```

#### 앱 만들기 - users app 생성

```
$ python manage.py startapp users
```

#### Usermodel 추가하기

```python
# models.py

from django.db import models
from django.contrib.auth.models import AbstractUser, BaseUserManager

GENDER_CHOICES = (
    ('M', 'Man'),
    ('W', 'Woman'),
)


class UserManager(BaseUserManager):
    """
    전달된 데이터로 유저를 생성하고 저장한다.

    BaseUserManager 클래스를 상속받아 만드는 커스텀 매니저는
    create_user / create_superuser 두 개의 메소드를 구현하여야 한다.
    """

    # _ -> 클래스 내부에서만 사용하겠다는 의미
    def _create_user(self, email, username, password, **extra_fields):
        if not email:
            raise ValueError('The given email must be set')
        email = self.normalize_email(email)
        username = self.model.normalize_username(username)
        user = self.model(email=email, username=username, **extra_fields)
        user.set_password(password)
        user.save(using=self._db)
        return user

    def create_user(self, email, username='', password=None, **extra_fields):
        """
        일반 유저 생성
        """
        extra_fields.setdefault('is_staff', False)
        extra_fields.setdefault('is_superuser', False)
        return self._create_user(email, username, password, **extra_fields)

    def create_superuser(self, email, username, password, **extra_fields):
        """
        관리자 유저 생성
        전달된 데이터로 관리자를 생성하고 저장한다.
        처음은 manage.py를 이용해서 만듦(createsuperuser)
        """
        extra_fields.setdefault('is_staff', True)
        extra_fields.setdefault('is_superuser', False)

        if extra_fields.get('is_staff', True):
            raise ValueError('Superuser must have is_staff=True.')
        if extra_fields.get('is_superuser') is not True:
            raise ValueError('Superuser must have is_superuser=True.')

        return self._create_user(email, username, password, **extra_fields)


class User(AbstractUser):
    """
    USERNAME_FIELD : 유저 모델에서 필드의 이름을 설명하는 string이다. 유니크 식별자로 사용된다. (unique=True이 설정 되어있어야 한다.)
    REQUIRED_FIELDS : createsuperuser 커맨드로 유저를 생성할 때 나타나는 필드 이름 목록

    - Meta class -
    db_table : 해당 모델과 매핑되는 데이터베이스 테이블의 이름을 지정해준다.
    verbose_name : 모델자체 이름
    verbose_name_plural : 모델 이름의 복수형
    """
    email = models.EmailField(verbose_name='email', max_length=255, unique=True)
    username = models.CharField(max_length=30)
    gender = models.CharField(max_length=5, choices=GENDER_CHOICES)
    # models.SmallIntegerField(choices = GENDER_CHOICES)를 사용하면 앞 부분의 'M' / 'W' 을 숫자 0, 1로 사용할 수도 있다.

    objects = UserManager()  # 모델의 매니저로 UserManager로 쓸거라는걸 알려준다.
    USERNAME_FIELD = 'email'  # 원래 username으로 로그인해야한는데 email로 치환해줌
    REQUIRED_FIELDS = []  # 필수로 받고 싶은 필드를 넣으면 되지만 로그인을 email로 하기에 그냥 비워둠

    # 추가 참고 예를 들어 username으로 로그인하면 email을 써주는데 현재는 email로 로그인하니깐
    def __str__(self):
        return "<%d %s>" % (self.pk, self.email)

```

#### 회원가입 Form 만들기

```text
# forms.py

from django import forms
from .models import User


class RegisterForm(forms.ModelForm):
    """
    회원가입 form
    회원가입 양식을 위해 만듦
    widget은 django에서 HTML 입력요소를 가르키는 말이다.
    """
    password = forms.CharField(label='password', widget=forms.PasswordInput)  # 비밀번호
    confirm_password = forms.CharField(label='confirm password', widget=forms.PasswordInput)  # 비밀번호 확인

    class Meta:
        model = User
        fields = ['username', 'first_name', 'last_name', 'gender', 'email']
        # fields를 이용해서 입력받을 fields들을 사용할 수 있지만,
        # password같은 경우는 종류가 CharField이기 때문에 widget이라는 다른 옵션을 사용해서 password속성을 사용하려고 변수를 지정하여 사용

    # 유효성 검사
    def clean_confirm_password(self):
        cd = self.cleaned_data
        # 비밀번호 비교
        if cd['password'] != cd['confirm_password']:
            raise forms.ValidationError('비밀번호가 일치하지 않습니다.')

        return cd['confirm_password']

```

#### 회원가입 View 만들기

```python
# views.py

from django.shortcuts import render
from .forms import RegisterForm


# 회원가입
def register(request):
    """
    POST : http method중의 하나로 서버로 데이터를 전송할때 쓰이는 method이다.
    """
    if request.method == "POST":  # 이 정보가 서버로 전달되었다는 부분
        user_form = RegisterForm(request.POST)
        if user_form.is_valid():  # 유효성 검사
            user = user_form.save(commit=False)  # commit = False 로 인해 DB에 저장이되는게 아니라 메모리상에서 객체가 만들어진다.
            user.set_password(user_form.cleaned_data['password'])
            user.save()  # 이 부분에서 DB에 저장한다.
            return render(request, 'registration/login.html', {  # login화면으로 보낸 이유 : 회원가입을 끝마친 상태이기 때문에
                'user': user
            })
    else:
        user_form = RegisterForm()

    return render(request, 'registration/register.html', {
        'user_form': user_form
    })

```

#### 마무리 단계

> settings.py에서 추가해줄 부분 안하면 makemigrations 안됨

```python
# setting.py

...
INSTALLED_APPS = [
    ...
    'users'
]

...

AUTH_USER_MODEL = 'users.User'
```

