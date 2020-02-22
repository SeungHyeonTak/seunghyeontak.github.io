---
layout: post
title:  "Django custom user model"
comments: true
description: "Django custom user model"
author: SeungHyeon Tak
date:   2019-10-09 19:55:13 +0700
categories: [Django]
tags: [Django]
keywords: "Django custom user model"
---
## Django User 모델 확장(AbstractUser 사용)

* 장고 내장 User 모델을 사용해오다가 User 모델을 확장해야하는 이유??
  - Django의 내장 User모델이 제공하는 필드 외에 다른 필드 등을 추가하고 싶을 때를 위해서
     - 내 입맛대로 User모델 커스텀하기 위해서 (ex. 성별, login할때 username말고 email이나 다른 방법으로 로그인 하고싶을때)


### 커스텀 유저 모델 생성 하기

기본적으로 account라는 앱을 만들고 models.py에서 수정해보겠다.

일단 커스텀 유저모델을 만들기 위해서는 두 클래스를 구현해야한다.

`(AbstractBaseUser, BaseUserManager)`를 불러 와야 한다.

```python
from django.contrib.auth.models import (BaseUserManager, AbstractBaseUser)
```

BaseUserManager 클래스는 유저를 생성할때 사용하는 헬퍼 클래스([helper class](#^helper class-1))이며, 

실제 모델은 AbstractBaseuser를 상속 받아 생성하는 클래스 이다.

여기서 헬퍼 클래스인 `class UserManager(BaseUserManager)`는 두 가지 함수를 가지고 있다.

* create_user(*username_field, *password=None, **other_fields)

* create_superuser(*username_field, *password=None, **other_fields)

첫 번째 파라미터는 username파라미터이다.

우리는 username 대신 email을 사용할 수 있다.

그럼 예를 들어 email을 사용해보겠다.

그러기 위해선 AbstractBaseUser가 구성되어 있어야한다.

```python
# models.py

class UserManager(BaseUserManager):
    def create_user(self, email, date_of_birth, password=None):
        if not email:
            raise ValueError('Users must have an email address')

        user = self.model(
            email=self.normalize_email(email),
            date_of_birth=date_of_birth,
        )

        user.set_password(password)
        user.save(using=self._db)
        return user

    def create_superuser(self, email, date_of_birth, password):
        user = self.create_user(
            email,
            password=password,
            date_of_birth=date_of_birth,
        )
        user.is_admin = True
        user.save(using=self._db)
        return user

class User(AbstractBaseUser):
    email = models.EmailField(verbose_name='email address',max_length=255, unique=True)
    birthday = models.DateField()
    is_active = models.BooleanField(default=True)
    is_admin = models.BooleanField(default=False)

    objects = UserManager()
    USERNAME_FIELD = 'email'

    def has_perm(self, perm, obj=None):
	return True

    def has_module_perms(self, app_label):
	return True

    def is_staff(self):
	return True
```

**모델 필드에 대해선 설명하지 않겠음.**

모델 필드 밑에 objects 부터에 대한 설명이다.

User 모델을 생성하기 위해 꼭 필요한 부분이다.

헬퍼 클래스를 사용하도록 설정 하였고`(objects=UserManager())`

username field를 email로 사용하도록 설정하였다. `USERNAME_FIELD = 'email'`

커스텀 유저 모델을 기본 유저 보델로 사용 하기위해서 구현해야하는 부분 도 있다. 

AbstractBaseUser로 만든 class 안에 작성해야한다.

* def has_perm(self, perm, obj=None):
  -> True를 반환하여 권한이 있음을 알려준다. object를 반환하는 경우 해당 Object로 사용 권한을 확인하는 절차가 필요하다.
  
* def has_module_perms(self, app_label):
  -> True를 반환하여 주어진 앱의 모델에 접근 가능하도록 한다. 
  
* def is_staff(self):
  -> True가 반환되면 django의 관리자 화면에 로그인 할 수 있다.
  
### 관리자 페이지 수정

> django의 관리자 페이지를 통해 유저를 관리하기 위해 관리자 페이지를 수정하도록 한다.

**Form 생성**

(상황에 따라 admin.py에 form을 적용 시켜도 된다. 하지만 따로 구분 해주자)

```python
# forms.py

from django import forms
from django.contrib.auth.forms import ReadOnlyPasswordHashField
from .models import User

class UserCreationForm(forms.ModelForm):
    password1 = forms.CharField(label='Password', widget=forms.PasswordInput)
    password2 = forms.CharField(label='Password confirmation', widget=forms.PasswordInput)

    class Meta:
	model = User
	fields = "__all__" # ('email', 'birthday')

    def clean_password2(self):
	password1 = self.cleaned_data.get("password1")
        password2 = self.cleaned_data.get("password2")
        if password1 and password2 and password1 != password2:
    	    raise forms.ValidationError("Passwords don't match")
	
        return password2

    def save(self, commit=True):
	user = super().save(commit=False)
	user.set_password(self.cleaned_data["password1"])
	if commit:
	    user.save()

	return user

class UserChangeForm(forms.ModelForm):
    password = ReadOnlyPasswordHashField()

    class Meta:
  	model = User
  	fields = ('email', 'password', 'birthday', 'is_active', 'is_admin')

	def clean_password(self):
	    return self.initial["password"]
```

사용자 생성 폼과 수정 폼을 만들어야 한다.

생성폼은 `password1`과 `password2`를 가지고 있으며

기본적으로 우리가 만든 User model의 `email`과 `birthday`를 가지고 있다.

그리고 `clen_password2(self)`를 통해 password1과 password2가 일치 하는지 검증 해야한다.

그리고 `def save(self, commit=True):`를 통해 데이터를 저장한다.

수정폼은 사용자 암호를 ReadOnlyPasswordHashField()로 가져와서 화면에 표시 해줄 예정이다.

`password = ReadOnlyPasswordHashField()`

또한 사용자의 모델과 모델 필드를 가져오고, 저장할때 clean_password(self):를 통해 password를 그대로 다시 저장하도록 한다.

### 관리자 페이지에 적용

form에서 만들었던것을 토대로 django admin 페이지에 적용할 수 있다.

```python
# admin.py

from django.contrib import admin
from django.contrib.auth.admin import UserAdmin as BaseUserAdmin
from .forms import UserChangeForm, UserCreationForm
from .models import User

@admin.register(User)
class UserAdmin(BaseUserAdmin):
    form = UserChangeForm
    add_form = UserCreationForm

    list_display = ('email', 'date_of_birth', 'is_admin')
    list_filter = ('is_admin',)
    fieldsets = (
	(None, {'fields': ('email', 'password')}),
	('Personal info', {'fields': ('date_of_birth',)}),
	('Permissions', {'fields': ('is_admin',)}),
    )

    add_fieldsets = (
		(None, {'classes': ('wide',),
		'fields': ('email', 'date_of_birth', 'password1', 'password2')
		}),
	)
    search_fields = ('email',)
    ordering = ('email',)
    filter_horizontal = ()
```

* class UserAdmin(BaseUserAdmin):
	* form = UserChangeForm
	* add_form = UserCreationForm
   (관리자 화면의 사용자 변경 폼과 사용자 추가 폼을 우리가 생성한 폼으로 설정함)

이후 list_display ~ add_fieldests까지는 커스텀유저 모델이 관리자 화면에 어떻게 표시 할지 설정하는 구간이다.

### 커스텀 유저 모델 등록

settings.py에 `AUTH_USER_MODEL = 'account.User'`라고 수정해야함

### 테이블 생성

`$ ./manage.py makemigrations account`
`$ ./manage.py migrate`

위 두개의 명령으로 테이블을 생성한다.

### superuser생성

`$ ./manage.py createsuperuser`

기존에 알던 superuser생성 방식이 달라진걸 볼 수 있을 것이다.

