---
layout: post
title:  "Django_기본.(Django에서 models-ORM)"
comments: true
description: "Django"
author: SeungHyeon Tak
date:   2019-06-02 20:13:54 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.기본 웹개발
<br>
* 장고 모델 - ORM
<br>
* application의 데이터 저장 방법
  * 데이터 베이스 : RDBMS(장고는 여기에 의존성 있게 저장됨), NoSQL ...
  * 파일 : 로컬, 외부 정적 스토리지(AWS,...)
<br>
* 데이터 베이스와 SQL
  * 데이터베이스의 종류
  * RDBMS (관계형 데이터베이스 관리 시스템)
  *  PostgreSQL, MySQL, SQLite, MS-SQL, Oracle 등
  *  NoSQL : MongoDB, Cassandra, CouchDB, Google BigTable 등
  *  데이터베이스에 쿼리하기 위한 언어 -> SQL
  *  중요) ORM을 쓰더라도, 내가 작성된 ORM코드를 통해 어떤 SQL이 실행되고 있는 지, 파악을 하고 이를 최적화할 수 있어야 한다.
<br>
* 장고 ORM인 모델은 RDB 만을 지원
  * My-SQL, Oracle, PostgreSQL, Sqlite3
  * Ms-SQL은 별도 라이브러리를 설치해서 사용가능

* 장고의 강점은 Model과 Form(이렇게 안쓰면 장고를 왜 쓰는지 의문이라고 함)
  * SQL을 직접 실행하실 수 있음
    * 직접 SQL문자열을 조합하지말고, 인자로 처리 - SQLInjection 방지
<br>
<br>
* 장고 내장 ORM
  * <데이터베이스 테이블>과 <파이썬 클래스>를 1:1로 매핑
  * 모델 클래스명은 단수형으로 지정 - 예: Posts (X), Post (O)
    * 클래스이기에 필히 "첫 글자가 대문자인 CamelCase 네이밍"
  * 매핑되는 모델 클래스는 DB 테이블 필드 내역이 일치해야함.
  * 모델을 만들기 전에, 서비스에 맞게 데이터베이스 설계가 필수.

```python
from django.db import models
class Post(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```
<br>

* 모델명과 DB 테이블 명
  * DB테이블 명 : default "앱이름_모델명"
  ex) *blog App*
    Post 모델 -> "blog_post"
    Comment 모델 -> "blog_comment"
    *shop App*
    Item 모델 -> "shop_item"
    Review 모델 -> "shop_review"
<br>

* 모델 활용순서(장고 모델을 통해, 데이터베이스 형상을 관리할 경우)
  * 모델 클래스 생성
  * 모델 클래스로부터 마이그레이션파일 생성 -> python manage.py makemigrations
  * 마이그레이션 파일을 데이터베이스에 적용 -> python manage.py migrate
  * 이후 활용

    