---
layout: post
title:  "Django_기본.Rest Framework 사용하기"
comments: true
description: "Django"
author: SeungHyeon Tak
date:   2019-06-18 23:46:21 +0700
categories: [Django]
tags: [Django]
keywords: "Django, Rest Framework"
---
#### Django.기본 웹 개발

#### Django Rest Framework 사용하기

* django Rest Framework 설치

```
$ pip install djangorestframework
```
<br>

* INSTALLED_APPS에 rest_framework 추가

```python
INSTALLED_APPS = [
    'photo',
    ...,
    'rest_framework',
]
```
<br>

* accounts 앱을 미리 만들어준 다음 진행 하기
(차후 DB가 엉킬 가능성이 있기 때문에...)

```python
# models.py

from django.db import models
from django.contrib.auth.models import AbstractUser
# Create your models here.

class User(AbstractUser):
    message = models.CharField(max_length=200)
    profile = models.ImageField(upload_to='profiles/%Y/%m/%d', blank=True)

# 추가 해주기
```
<br>

* app 폴더에 serializers.py 생성 후 내용 입력

```python
# models.py
class Photo(models.Model):
    author = models.ForeignKey(get_user_model(), on_delete=models.CASCADE, related_name='photo')
    text = models.TextField()
    created = models.DateTimeField(auto_now_add=True)
    updated = models.DateTimeField(auto_now=True)


# serializers.py
from rest_framework import serializers
from .models import Photo

class PhotoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Photo
        fields = '__all__'
```
<br>

* 기본 뷰 작성 API 뷰도 함수형 뷰와 클래스 형 뷰를 만들 수 있다.
generics를 이용하면 뷰를 빠르게 작성가능하다. ListCeateAPIView는 모곡 확인

```python
from django.shortcuts import render
from .serializers import PhotoSerializer
from .models import Photo
from rest_framework import generics

class PhotoView(generics.ListCreateAPIView):
    queryset = Photo.objects.all()
    serializer_class = PhotoSerializer
```
<br>

* 만든 뷰를 urls.py에 추가하고 동작 시켜 보기

```python
# photo/urls.py

from django.urls import path
from .views import PhotoView

urlpatterns = [
    path('', PhotoView.as_view()),
]




# config/urls.py

from django.contrib import admin
from django.urls import path,include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('',include('photo.urls')),
    path('accounts/',include('accounts.urls')),
]
```
<br>

* 브라우저를 이용해 해당 뷰가 동작하는지 확인하기
* 목록 뷰 추가 후 수정, 삭제를 위한 뷰 추가하기

```python
# views.py

class PhotoDetail(generics.RetrieveUpdateAPIView):
    queryset = Photo.objects.all()
    serializer_class = PhotoSerializer


# urls.py
from django.urls import path
from .views import PhotoView,PhotoDetail

urlpatterns = [
    path('', PhotoView.as_view()),
    path('detail/<int:pk>/',PhotoDetail.as_view()),
]
```
<br>

* 지금 사용할 뷰는 일반 API 뷰 렌더링 페이지가 아닌, 개발 편의를 위해 제공되는 BrowsableAPIRenderer 페이지이다. 나오는 형식을 제한 하기 위해서는 주소에 형식을 기입 하거나 뷰에서 설정해주자

```python
from rest_framework.renderers import JSONRenderer

...


class PhotoDetail(generics.RetrieveUpdateAPIView):
    renderer_classes = [JSONRenderer]
```
<br>

* API 문서를 위한 Swagger를 설치하여 사용해보기

```
$ pip install django-rest-swagger==2.1.2
```
<br>

* INSTALLED_APPS에 rest_framework을 추가 / config/urls.py에 swagger view 추가

```python
# settings.pys

INSTALLED_APPS = [
    'photo',
    'accounts',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    'django_extensions',
    'rest_framework_swagger',
]

# config/urls.py

from rest_framework_swagger.views import get_swagger_view
schema_view = get_swagger_view(title='Photo API Document')

urlpatterns = [
    ...
    path('api/doc/',schema_view),
]

```
<br>

* 인증 과정 추가해보기, API를 만들 대 가장 많이 사용하는 방식이 토큰 방식이다.

```python
# config/settings.py

INSTALLED_APPS = [
    ...
    'rest_framework.authtoken',
]
```

이후 토큰 기능을 추가하면 데이터베이스에 추가 테이블이 필요하기 때문에 migrate 명령을 실행해야한다.
<br>
* 토큰 앱을 추가하면 토큰을 자동 생성하는 기능이 추가 되어있는데, 토큰을 발급받는 뷰만 추가해주면 토큰을 받아서 확인할 수 있다.

```python
# config/urls.py

from rest_framework.authtoken.views import obtain_auth_token

urlpatterns = [
    ...
    path('api/get_token/',obtain_auth_token),
]

```

<br>
* 인증 기능을 추가 하면 모든 API뷰가 로그인 해야만 동작 가능하니 추가로 관련 설정을 추가한다.

```python
# setting.py

...
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.lsAuthenticated',
    )
}
```

<br>

* POST맨 등에서 토큰을 전달해 접속해야해한다. 하지만 이런 테스트방법이 불편하기 때문에 Swagger에 Token 인증 방법을 추가하고 사용할 수 있도록 추가한다.

```python
# settings.py

REST_FRAMEWORK =
    ...
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework.authentication.TokenAuthentication',
    ),
}
```

<br>
* Swagger에서 토큰 인증 기능을 활성화

```python
# settings.py
SWAGGER_SETTINGS={
    'SECURITY_DEFINITIONS':{
        "api_key":{
            "type":"apiKey",
            "name":"Authorization",
            "in":"header"
        }
    }
}
```

<br>
* accounts에 기존 모델을 그대로 사용할 것이기 때문에 Serializer만 추가한다

```python
# accounts/serializers.py

from django.contrib.auth import get_user_model
from rest_framework import serializers

class UserListSerializer(serializers.ModelSerializer):
    class Meta:
        model = get_user_model()
        fields = ('id','username','first_name','last_name')
```

<br>
* 유저 생성을 위한 시리얼라이저를 하나더 추가한다.
유저를 생성할 때 필요한 필수 정보를 입력받기 위해 fields에 필요한 필드만 입렫하고 비밀번호를 해싱해서 저장하기 위해서 create 메서드에서 set_password 메서드를 이용해 비밀번호를 만들어 저장하도록 로직을 추가한다.

```python

from django.contrib.auth import get_user_model
from rest_framework import serializers

class UserCreateSerializer(serializers.ModelSerializer):
    class Meta:
        model = get_user_model()
        fields = ('username','password','first_name','last_name')

    def create(self, validated_data):
        user = get_user_model().objects.create(**validated_data)
        user.set_password(validated_data.get('password'))
        user.is_active = True
        user.save()

        return user
```
<br>

* 두 개의 시리얼라이저를 활용한 뷰를 생성해 보기

```python
# accounts/views.py

from django.shortcuts import render
from rest_framework import generics
from .serializers import *


class UserListAPI(generics.ListAPIView):
    queryset = get_user_model().objects.all()
    serializer_class = UserListSerializer


class UserCreateAPI(generics.CreateAPIView):
    queryset = get_user_model().objects.all()
    serializer_class = UserCreateSerializer
```

<br>

* 뷰를 앱 폴더의 urls.py에 등록

```python
from django.urls import path
from .views import *


urlpatterns = [
    path('create/',UserCreateAPI.as_view(), name='user_create'),
    path('list/',UserListAPI.as_view(), name = 'user_list'),
]
```

<br>
* get_queryset 메서드를 오버라이드 하면 관리자인 경우만 전체 목록을 반환하고 아닌 경우에는 자신의 정보만 반환하도록 수정한다.

```python
# accounts/views.py

class UserListAPI(generics.ListAPIView):
    queryset = get_user_model().objects.all()
    serializer_class = UserListSerializer

    def get_queryset(self):
        queryset = super().get_queryset()
        if not self.request.user.is_staff:
            queryset = queryset.filter(pk=self.request.user.id)
        return queryset
```

<br>

* 사진을 수정하는 뷰에도 권한을 추가
자신의 글이거나 관리자인 경우에만 글을 수정할 수 있도록 하는 로직을 추가해본다.
`photo/permissions.py`
permissions.py에는 커스텀 권한 클래스를 작성할 수 있다.

```python
# photo/permissions.py

from rest_framework import permissions

class IsOwnerOnly(permissions.BasePermission):
   def has_object_permission(self, request, view, obj):
        return obj.author == request.user

class IsOwnerAndAdminOnly(permissions.BasePermission):
    def has_object_permission(self, request, view, obj):
        return obj.author == request or request.user.is_superuser

class IsOwnerOrReadOnly(permissions.BasePermission):
    def has_object_permission(self, request, view, obj):
        if request.method in permissions.SAFE_METHODS:
            return True

        return obj.author == request or request.user.is_superuser
```

<br>

* 작성한 권한 클래스는 뷰에 추가하여 사용

```python
# photo/views.py

from rest_framework.permissions import IsAuthenticated
from .permissions import *

class PhotoDetail(generics.RetrieveUpdateDestroyAPIView):
    permission_classes = [IsAuthenticated, IsOwnerAndAdminOnly]
```
<br>

* 글을 올릴때 작성자를 자동으로 설정하려면 뷰의 메서드를 오버라이드 해야한다.
create메서드는 API를 통해 객체를 생성할 때 동작하는 메서드이다.
필요한 내용은 이 메서드 안에 작성하면 가능

```python
# photo/views.py

class PhotoView(generics.ListCreateAPIView):

    ...

    def create(self, request, *args, **kwargs):
        request.data['author'] = request.user.id
        return super().create(request, *args, **kwargs)
```

<br>

* 필터 기능을 활용하기 위해 장고 필터를 추가 설치

```
$ pip install django-filter
```

<br>

* INSTALLED_APPS에서 django_filters를 추가한다.
* 나머지 추가 작업

```python
# settings.py

INSTALLED_APPS = [
    ...
    'django_filters',
]

REST_FRAMEWORK = {
    ...
    'DEFAULT_FILTER_BACKENDS':(
        'django_filters.rest_framework.DjangoFilterBackend',
    ),
}
```

<br>

* 필터 기능을 동작시키고 싶은 뷰에 필드 추가

```python
class UserListAPI(generics.ListAPIView):
    ...
    filter_fields = ('id', 'username')
```

<br>
* SearchFilter를 필터 백엔드에 추가하기

```python
REST_FRAMEWORK = {
    'DEFAULT_FILTER_BACKENDS':(
	...
        'rest_framework.filters.SearchFilter',
    ),
}
```

<br>
* 검색하고자 하는 필드를 뷰에 추가한다.
Search_fields에 검색 대상 필드를 나열하면 되는데 ForeignKey로 묶인 필드의 경우
__(언더바 두개)를 쓰기 필드명을 작성하면 동작함

```python
# photo/views.py


class PhotoView(generics.ListCreateAPIView):
    queryset = Photo.objects.all()
    serializer_class = PhotoSerializer

    search_fields = ('author__username',)

    ...
```

* 디테일 넣기

```python

class UserDetailView(generics.RetrieveAPIView):
    queryset = get_user_model().objects.all()
    serializer_class = UserDetailSerializer

```
