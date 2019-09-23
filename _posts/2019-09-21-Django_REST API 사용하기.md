---
layout: post
title:  "Django_REST Framework"
comments: true
description: "Django REST Framework, DRF"
author: SeungHyeon Tak
date:   2019-09-21 21:18:09 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django REST Framework


#### REST API란?
> 간단하게 어떤일을 할 때 훨씬 쉽고 간단하게 일을 진행할 수 있는 방법이라고 이해하면 된다.
> 우리가 웹 서버를 만들 때 카테고리를 나타낸다고 가정하면

```text
/show/categoryname  -> 카테고리 이름을 나타내기
/add/categoryname  -> 카테고리 이름을 추가
/change/categoryname  -> 카테고리 이름을 변경
/delete/categoryname  -> 카테고리 이름을 삭제
```

> 이 외에도 포스트로도 나타낼 수 있다.

```text
/showpost/categoryname/postname  -> 포스트 이름을 나타내기
/addpost/categoryname/postname  -> 포스트 이름을 추가
/changepost/categoryname/postname  -> 포스트 이름을 변경
/deletepost/categoryname/postname  -> 포스트 이름을 삭제
```

> 이렇게 만들어줄 수 있지만 그에대한 문제점을 알 수 있다.
> 하고자하는 일이 많이 생길수록 url동작 이름이 늘어나고 수정이 힘들게 된다.

*****

> 그것들을 보완하는것이 REST API라고 할 수 있다.
> 예를 들어 보면
* `show/categoryname` 에서
> show는 어떤 일에 대한 표현이고
> categoryname은 어떤 리소스에 관한 이름이다.
> 이것들을 REST API에서 분리 시켜준다.
> 그렇게 되면 훨씬 간결하게 짜여진다.

```text
http method: GET	url:	/categoryname  -> 카테고리 이름을 나타내기
http method: POST	url:	/categoryname  -> 카테고리 이름을 추가
http method: PUT	url:	/categoryname  -> 카테고리 이름을 변경
http method: DELETE	url:	/categoryname  -> 카테고리 이름을 삭제

http method: GET	url:	/category/post	-> 포스트 이름을 나타내기
http method: POST	url:	/category/post	-> 포스트 이름을 추가
http method: PUT	url:	/category/post	-> 포스트 이름을 변경
http method: DELETE	url:	/category/post	-> 포스트 이름을 삭제
```

> 이 동작을 http method로 정의 하게되면 깔끔해진걸 볼 수 있다.
> REST API를 이해하는데 약간 도움이 된다.

*****

#### Serializers

> 우리는 일반적인 Django만 접해왔다면 이 부분은 생소 할 수 있다.
> Serializer란 model객체와 queryset 같은 복잡한 데이터를 JSON, XML과 같은 native데이터로 바꿔주는 역할을 한다.
> 기존 Django를 이용한 웹 개발에서 Django ORM의 Queryset은 Django template로 넘겨지며, HTML로 랜더링 되어 Response로 보내지게 된다.

> 하지만 JSON으로 데이터를 보내야하는 RESTful API는 HTML로 렌더링 되는 Django template를 사용할 수 없다.
> 그러므로 우리는 Queryset을 Nested한 JSON으로 매핑하는 과정을 거쳐야 한는데, 이 작업을 지금 작성할 Serializer로 작성 하게 된다.

```text
* Django template
Queryset<POST>  <-->  <title>제목</title> <body>...</body>

* Serializer
Queryset<POST>  <-->  {"user":1, "title":"제목", "subtitle":"부제목", ..}
```

#### Django REST Framework 설치

> $ pip install django-rest-framework

```python
settings.py

...
INSTALLED_APPS = [
	...
	rest_framework,
	...
]
...
```

> app에 serializers.py 생성

```python
from rest_framework import serializers
from .model import Product

class ProductSerializer(serializers.ModelSerializer):
	class Meta:
		model = 'Product'
		field = '__all__'
```

> 이런식으로 Meta class를 이용하여 생성
> 이후 views.py에서 GenericAPIView / mixin을 이용해 API를 생성할 수 있다.
> 자세한건 코드 참조 [Github](https://github.com/SeungHyeonTak/shoppingmall)