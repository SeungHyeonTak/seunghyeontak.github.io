---
layout: post
title:  "Django_기본.(Django에서 models)"
comments: true
description: "Django"
author: SeungHyeon Tak
date:   2019-06-01 15:13:54 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.기본 웹개발
<br>
* 장고 커스텀 모델

```
class형식으로 작성
class Post(models.Model):
models.CharField(max_lenght=100) // 길이 제한있음 100자
models.TextField(blank=True) // 값이 없어도 저장가능
models.PositiveIntegerField() // 양수로 입력하겠다
models.DateTimeField(auto_now_add = True) // 현재 시간을 자동으로 저장
models.DateTimeField(auto_now = True) // 매번 저장될때마다 시간 자동 저장
...
등등 많은 내용들이 있다.
```

* 모델 등록 하는법
모델을 다 작성하고 바로 runserver를 돌려도 아직 DB 테이블이 생성이 안되어 우리가 원하는 작업이 안될것이다.
그러므로

```
$ python manage.py makemigrations
$ python manage.py migrate
```

이 명령어를 통해 앱 폴더 내부에 migration 폴더가 생성되고 DB에 테이블이 생성이 된다.
