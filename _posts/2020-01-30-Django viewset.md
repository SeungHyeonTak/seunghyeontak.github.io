---
layout: post
title:  "Django ViewSet"
comments: true
description: "Django, ViewSet"
author: SeungHyeon Tak
date:   2020-01-30 22:55:00 +0700
categories: [Django]
tags: [Django]
keywords: "Django, ViewSet"
---
## Django ViewSet

Django REST 프레임 워크를 사용하면 ViewSet이라는 단일 클래스의 관련 뷰 세트에 대한 논리를 결합 할 수 있습니다. 

다른 프레임 워크에서는 'Resources'또는 'Controllers'와 같은 개념적으로 유사한 구현도 찾을 수 있습니다.

ViewSet 클래스는 `.get () 또는 .post ()`와 같은 메소드 핸들러를 제공하지 않고 

`.list () 및 .create ()`와 같은 조치를 제공하는 클래스 기반보기 유형입니다.

ViewSet의 메소드 핸들러는 `.as_view () 메소드를 사용하여 뷰`를 마무리 할 때 해당 조치에만 바인드됩니다.

일반적으로 urlconf의 뷰셋에 뷰를 명시 적으로 등록하는 대신 라우터 클래스에 뷰셋을 등록하면 자동으로 urlconf가 결정됩니다.

```python
from django.contrib.auth.models import User
from django.shortcuts import get_object_or_404
from apps.serializers import USerSerializer
from rest_framework import viewsets
from rest_framework.response import Response

class UserViewSet(viewsets.ViewSet):
    """
    사용자를 나열하거나 검색하기 위한 간단한 ViewSet이다.
    """
    def list(self, request):
        queryset = User.objects.all()
	serializer = UserSerializer(queryset, many=True)
	return Response(serializer.data)

    def retrieve(self, request, pk=None):
	queryset = User.objects.all()
	user = get_object_or_404(queryset, pk=pk)
	serializer = UserSerializer(user)
	return Response(serializer.data)
```

필요한 경우이 뷰 세트를 두 개의 개별 뷰에 바인딩 할 수 있습니다.

```python
user_list = UserViewSet.as_view({'get': 'list'})
user_detail = UserViewSet.as_view({'get': 'retrieve'})
```

일반적으로이 작업은 수행하지 않지만 대신 라우터에 뷰 세트를 등록하고 urlconf가 자동으로 생성되도록합니다.

```python
from myapp.views import UserViewSet
from rest_framework.routers import DefaultRouter

router = DefaultRouter()
router.register(r'users', UserViewSet, basename='user')
urlpatterns = router.urls
```

자체 뷰셋을 작성하는 대신 기본 동작 집합을 제공하는 기존 기본 클래스를 사용하는 것이 좋습니다. 

예를 들면 다음과 같다.

```python
class UserVierSet(viewsets.ModelViewSet):
    """
    사용자 인스턴스를보고 편집하기위한 뷰 셋이다.
    """
    serializer_class = UserSerializer
    queryset = User.objects.all()
```

일반적으로 view class를 사용하는것보다 viewset 클래스를 사용하는것의 두가지 주요 이점이 있다.

- 반복 된 논리를 단일 클래스로 결합 할 수 있습니다. 위의 예에서는 쿼리 세트를 한 번만 지정하면 여러 뷰에서 사용됩니다
- 라우터를 사용하면 더 이상 URL conf를 직접 배선 할 필요가 없습니다.

이 두 가지 모두 trade-off와 함께 제공된다. 

정기적 인보기와 URL conf를 사용하는 것이보다 명확하고 더 제어 할 수 있다.

ViewSets는 빠르게 시작하고 실행하거나 큰 API가 있고 전체적으로 일관된 URL 구성을 적용하려는 경우에 유용합니다.

REST framework에 포함된 기본 라우터는 아래와 같이 create/retrieve/update/destory가 있다.

```python
class UserViewSet(viewsets.ViewSet):
    """
    라우터 클래스가 처리할 표준 동작을 보여주는 빈 뷰셋의 예

    형식 접미사를 사용하는 경우 각 작업에 대해 "format=None" 키워드 인수도 포함한다.
    """

    def list(self, request):
        pass

    def create(self, request):
        pass

    def retrieve(self, request, pk=None):
        pass

    def update(self, request, pk=None):
        pass

    def partial_update(self, request, pk=None):
        pass

    def destroy(self, request, pk=None):
        pass
```

### API Reference

**viewset**

ViewSet 클래스는 APIView에서 상속한다. 

viewset에서 API 정책을 제어하기 위해 `permission_classes`, `authentication_classes`와 같은 표준 속성을 사용할 수 있다. 

ViewSet 클래스는 동작 구현을 제공하지 않습니다. 

ViewSet 클래스를 사용하려면 클래스를 재정의하고 동작 구현을 명시 적으로 정의해야합니다.

**GenericViewSet**

GenericViewSet 클래스는 GenericAPIView에서 상속하며 

기본 `get_object`, `get_queryset` 메소드 및 기타 일반보기 기본 동작 세트를 제공하지만 기본적으로 조치는 포함하지 않습니다. 

GenericViewSet 클래스를 사용하려면 클래스를 재정의하고 필요한 믹스 인 클래스를 믹스 인하거나 액션 구현을 명시 적으로 정의해야합니다.

**ModelViewSet**

ModelViewSet 클래스는 GenericAPIView에서 상속되며 다양한 믹스 인 클래스의 동작을 혼합하여 다양한 조치에 대한 구현을 포함한다. 

ModelViewSet 클래스가 제공하는 조치는 .list (), .retrieve (), .create (), .update (), .partial_update () 및 .destroy ()가 있다.

예제)

ModelViewSet은 GenericAPIView를 확장하므로 일반적으로 최소한queryset 및 serializer_class 속성을 제공해야함

```python
class AccountViewSet(viewsets.ModelViewSet):
    """
    계정 보기 및 편집을 위한 간단한 ViewSet.
    """
    queryset = Account.objects.all()
    serializer_class = AccountSerializer
    permission_classes = [IsAccountAdminOrReadOnly]
```

GenericAPIView에서 제공하는 표준 속성 또는 메서드 재정의를 사용할 수 있습니다. 

예를 들어, 작동해야하는 쿼리 세트를 동적으로 결정하는 ViewSet을 사용하려면 다음과 같이 사용하면 된다.

```python
class AccountViewSet(viewsets.ModelViewSet):
    """
    사용자와 연결된 계정을 보고 편집하기 위한 간단한 ViewSet.
    """
    serializer_class = AccountSerializer
    permission_classes = [IsAccountAdminOrReadOnly]

    def get_queryset(self):
        return self.request.user.accounts.all()
```

그러나 ViewSet에서 Queryset 속성을 제거할 때 연결된 라우터는 자동으로 모델의 기본 이름을 얻을 수 없으므로 라우터 등록의 일부로 기본 이름 Kwarg를 지정해야 한다.

또한 이 클래스는 기본적으로 전체 create/list/retrieve/update/destroy를 제공하지만 표준 권한 클래스를 사용하여 사용 가능한 작업을 제한할 수 있다는 점에 유의

**ReadOnlyModeViewSet**

ReadOnlyModelViewSet 클래스도 GenericAPIView에서 상속됩니다. 

ModelViewSet과 마찬가지로 다양한 조치에 대한 구현도 포함하지만 ModelViewSet과 달리 `읽기 전용`조치 인 .list () 및 .retrieve () 만 제공한다.

ModelViewSet과 마찬가지로 일반적으로 최소한 queryset 및 serializer_class 속성을 제공해야한다.

```python
class AccountViewSet(viewsets.ReadOnlyModelViewSet):
    """
    accounts계정을 보기 위한 간단한 viewset
    """
    queryset = Account.objects.all()
    serializer_class = AccountSerializer
```
