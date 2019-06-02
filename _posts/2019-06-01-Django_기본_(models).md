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

* 기본 지원하는 모델 필드 타입
  * Primary Key: AutoField, BigAutoField
  * 문자열: CharField, TextField, SlugField
  * 날짜/시간: DateTimeField, DurationField
  * 참/거짓: BooleanField, NullBooleanField
  * 숫자: IntegerField, SmallIntegerField, PositiveIntegerField, PositiveSmallIntegerfield, BigIntegerField, DecimalField, FloatField
  * 파일: FileField, ImageField, FilePathField
  * 이메일: EmailField
  * URL: URLField
  * UUID: UUIDField
  * 아이피: GenericIPAddressField
  * Relationship Types
    * ForeignKey
    * ManyToManyField
<br>
* 필드 공통 옵션
  * blank : 파이썬 validation시에 empty 허용 여부 (디폴트: False)
  * null (DB 옵션) : null 허용 여부 (디폴트: False)
  * db_index (DB 옵션) : 인덱스 필드 여부 (디폴트: False)
  * default : 디폴트 값 지정, 혹은 값을 리턴해줄 함수 지정
  * 사용자에게 디폴트값을 제공코자 할 때
  * unique (DB 옵션) : 현재 테이블 내에서 유일성 여부 (디폴트: False)
  * choices : select 박스 소스로 사용
  * validators : validators를 수행할 함수를 다수 지정
<br>

```
class형식으로 작성 방법
class Post(models.Model):
models.CharField(max_lenght=100) // 길이 제한있음 100자
models.TextField(blank=True) // 값이 없어도 저장가능
models.PositiveIntegerField() // 양수로 입력하겠다
models.DateTimeField(auto_now_add = True) // 현재 시간을 자동으로 저장
models.DateTimeField(auto_now = True) // 매번 저장될때마다 시간 자동 저장
...
등등 많은 내용들이 있다.
```
