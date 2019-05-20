---
layout: post
title:  "Django_15.(설문조사 만들기)model 설정 하기"
comments: true
description: "Django_part15"
author: SeungHyeon Tak
date:   2019-04-30 22:54:45 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.15 웹 개발

다음으로 진행 할 내용은 `models.py`를 셋팅할 차례입니다.
하기전에 ForeignKey에 대해 보고 넘어가겠습니다.
예를 들어

* Question

|id(pk)|qtext|date|
|---|:---:|---:|
|1|고기|1970-1-1|
|2|짜장?짬뽕?|12-27|

* Choice

|id(pk)|qid(fk)|ctext|votes|
|---|:---:|---:|
|1|1|돼지|10|
|2|1|닭|20|
|3|1|소|255|
|4|2|짜장|99|
|5|2|짬뽕|98|
|6|2|탕수육|65535|

여기서 Choice(fk)는 Question의 id(pk)를 그대로 가져온다.
fk는 Foreignkey를 표현한다. pk는 PrimaryKey를 표현한다.

* `models.py`에서 내용 작성

```
from django.db import models

# Create your models here.

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')

class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    # ForeignKey('처음에 들어가는것') 누구의 ForeignKey인가를 알려준다.
    # on_delete=models.CASCADE는 어떤의미인가?
    # 질문을 삭제했을때 연관된 항목을 어떻게 할것인가? 연관된 항목이 없을경우 자동적으로 삭제시켜주는 역할
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

이렇게 작성한 후

```
$ python manage.py makemigrations
# 이렇게 하고 추가된 내용이 궁금하면 polls/migrations/0001_initial.py 에서 확인가능하다.

# 만약 더 자세히 보고싶다면 sql 명령어로 보여지는 방법도 있다.
$ python manage.py sqlmigrate polls 0001
# 관계형 데이터베이스로 보여준다.

$ python manage.py migrate
```
이렇게 작성을 마치면 models에 대한 작성은 끝났다.

