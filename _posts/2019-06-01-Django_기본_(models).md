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


#### Django Project (MTV)
* Model - 데이터베이스에 저장되는 데이터
* template - 사용자(user)에게 보여지는 부분
* view - 실질적으로 프로그램로직이 동작하여 템플릿에 전달


#### Django Project 모델링 설계방법
* 어떤 필드가 필요한지 생각해보기
* 생각이 되면 적어보기
* 각 필드는 어떤 타입이 되어야하는지 생각 혹은 논의 한다.

```text
주문(Order) 모델링
 - 주문번호 - pk
 - 고객-고객번호 - relation -pk
 - 총 금액 - int
 - 주문 상태 - choice

포스트(Post) 모델링
 - 글 번호 - pk
 - 유저-글쓴이 번호 - relation -pk
 - 글 내용 - TextField
 - 글 쓴 날짜 - DateTimeField
 - 글 수정한 날짜 - DateTimeField
```

> Primary Key(기본키, pk)
>> 테이블에 저장된 각각의 데이터를 유일하게 구분하는 키
>> 바꾸어 말하면, 데이터베이스 테이블에 각 행의 식별 기준인 기본키가 필요하다.
>> Django에는 기본키로 id가 디폴트로 지정되어 있다.
>> id = models.AutoField(primary_key=True)

> Foreignkey (외래키, fk)
>> 각 테이블 간에 연결하기 위해서 특정 테이블에서 다른 테이블의 참조되는 기본키를 외래키라고 한다.

```text
Users
-id
-user_name
-phone_number

Orders
-id
-product_name
-user_name
-total_price
-status

Products
-id
-product_name
-Product_price
```

> 여기서 외래키를 찾으라하면
> Orders의 product_name과 user_name을 고를 수 있다.

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
