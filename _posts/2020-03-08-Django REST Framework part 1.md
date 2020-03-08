---
layout: post
title:  "Django REST Framework"
comments: true
description: "Django REST Framework"
author: SeungHyeon Tak
date:   2020-03-08 23:21:00 +0700
categories: [Django]
tags: [Django]
keywords: "Django REST Framework"
---
## Django REST Framework

DRF tutorial을 보고 생성한 간단한 API 작성

**macOS 환경에서 제작**

### 프로젝트 설정

**(가상환경 설정)**

```bash
# pyenv 설치
$ brew install pyenv 

# 환경변수 설정 (zsh을 사용할 경우 ~/.zshrc로)
$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile 
$ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile
$ echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
$ source ~/.bash_profile

# pyenv용 virtualenv 설치하기
$ brew install pyenv-virtualenv 
$ echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bash_profile

# pyenv와 virtualenv로 가상환경을 제어하는 예
$ pyenv virtualenv <python version> <virtualenv-name> # 가상환경 생성
$ pyenv activate <virtualenv-name> # 가상환경 activate
$ pyenv deactivate
$ pyenv uninstall <virtualenv-name> # 가상환경 삭제

# 예) python 3.6.4버전의 환경을 추가한 후, 이를 사용하는 drf(virtualenv-name)이라는 이름의 가상환경을 생성할 것이라면
$ pyenv install 3.6.4
$ pyenv virtualenv 3.6.4 drf

# 프로젝트 디렉토리에 가상환경 autoenv 적용하기
# autoenv는 특정 디렉터리에 진입하면 해당 디렉터리에서 필요한 가상환경을 자동으로 activate해주는 툴이다.
# pyenv local 명령어를 사용하여 해당 디렉터리에 해당하는 가상환경을 설정 

# 이렇게 한번 설정 해놓으면 다음에 새로 pyenv를 설정 하려할때 밑의 두 구문만 입력해주면 된다.
$ pyenv virtaulenv <python version> <virtualenv-name>
$ pyenv local <virtualenv-name>
```

틈새 궁금증) command에서 source는 무엇을 뜻할까?

-> 스크립트 실행이 현재의 프로세스에서 실행하도록 해주는 명령어 이다.

**(django install)**

* Django및 Django REST Framework를 가상환경에 설치

```bash
$ pip install django
$ pip install djangorestframework
```

* 새 프로젝트 설정

```python
$ django-admin startproject config .
$ python manage.py startapp tutotial
```

**(DB 동기화)**

DataBase는 sqlite3 사용

* 첫 DB 동기화

```bash
$ python manage.py migrate
```

* 관리자 계정 생성

```bash
$ python manage.py createsuperuser

# 본인이 원하는 계정 id / pw 생성 - email은 안만들어도 무방
```

**serializer 정의**

startapp으로 만든 프로젝트 폴더에 `serializers.py`를 생성한다. 이후 밑의 코드를 입력

```python
from django.contrib.auth.models import User, Group
from rest_framework import serializers

class UserSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = User
        fields = ['url', 'username', 'email', 'groups']


class GroupSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = Group
        fields = ['url', 'name']
```

`HyperlinkedModelSerializer`클래스는 `기본 키가 아닌` 관계를 나타내는 데 하이퍼 링크를 사용한다는 점을 제외하고 ModelSerializer 클래스와 유사합니다.

기본적으로 시리얼 라이저는 `기본 키 필드 대신 URL 필드를 포함`합니다.

url 필드는 `HyperlinkedIdentityField serializer` 필드를 사용하여 표현되며 모델의 모든 관계는 HyperlinkedRelatedField serializer 필드를 사용하여 표현됩니다.

**views**

views.py에서 코드를 작성한다.

```python
from django.contrib.auth.models import User, Group
from rest_framework import viewsets
from rest_framework import permissions
from tutorial.quickstart.serializers import UserSerializer, GroupSerializer


class UserViewSet(viewsets.ModelViewSet):
    """
    API endpoint that allows users to be viewed or edited.
    """
    queryset = User.objects.all().order_by('-date_joined')
    serializer_class = UserSerializer
    permission_classes = [permissions.IsAuthenticated]


class GroupViewSet(viewsets.ModelViewSet):
    """
    API endpoint that allows groups to be viewed or edited.
    """
    queryset = Group.objects.all()
    serializer_class = GroupSerializer
    permission_classes = [permissions.IsAuthenticated]
```

여러개의 뷰를 작성하는 대신 모든 일반적인 동작을 viewsets라는 클래스로 그룹화 한다.

필요한 경우 이러한 뷰를 개별 뷰로 쉽게 세분화 할 수 있지만 뷰 세트를 사용하면 뷰 논리를 깔끔하게 정리하고 간결하게 유지할 수 있다.

**urls**

```python
from django.contrib import admin
from django.urls import path, include
from rest_framework import routers
from tutorial import views

router = routers.DefaultRouter()
router.register(r'users', views.UserViewSet)
router.register(r'groups', views.GroupViewSet)

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include(router.urls)),
    path('api-auth/', include('rest_framework.urls', namespace='rest_framework')),
]
```

뷰 대신 뷰 세트를 사용하므로 라우터 클래스에 뷰 세트를 등록하여 API에 대한 URL conf를 자동으로 생성 할 수 있다.

**Pagination**

페이지 매김을 사용하면 페이지 당 반환되는 개체 수를 제어 할 수 있다.

이를 활성화하려면 settings.py에 다음 줄을 추가 하면 된다.

```python
REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 10
}
```

**settings**

settings.py에 INSTALLED_APPS에 `rest_framework`를 추가하자

```python
INSTALLED_APPS = [
	...
	'rest_framework',
]
```

### Testing API

runserver를 돌려서 확인해본다.

이후 브라우저 오른쪽 상단의 로그인이 되어있는지 확인하고 로그인을 해야한다.
