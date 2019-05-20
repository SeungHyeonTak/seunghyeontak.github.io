---
layout: post
title:  "Django_16.(설문조사 만들기)Shell로 모델 조작하기"
comments: true
description: "Django_part16"
author: SeungHyeon Tak
date:   2019-05-01 12:51:12 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.16 웹 개발

* shell 로 들어가는 방법

```
# 터미널 입력해야함
$ python manage.py shell
```

```
>>> from polls.models import Question, Choice
>>> Question.objects.all()
>>> from django.utils import timezone
>>> q = Question(question_text="What's new?", pub_date=timezone.now())
>>> q.save()
>>> q.id
>>> q.question_text
>>> q.pub_date
>>> q.question_text = "What's up?"
>>> q.save()
>>> Question.objects.all()
```

```
from django.db import models
from django.utils import timezone
import datetime

class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')

    def __str__(self):
        return self.question_text

    def was_published_recently(self):
        return self.pub_date >= timezone.now() - datetime.timedelta(days = 1)
    # 이 날짜가 최신일때만 true해준다.  현재시간에서 하루뺀것보다(어제) 크다면

class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    # ForeignKey('처음에 들어가는것') 누구의 ForeignKey인가를 알려준다.
    # on_delete=models.CASCADE는 어떤의미인가?
    # 질문을 삭제했을때 연관된 항목을 어떻게 할것인가? 연관된 항목이 없을경우 자동적으로 삭제시켜주는 역할
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)

    def __str__(self):
        return self.choice_text
```


```
>>> from polls.models import Question, Choice
>>> Question.objects.all()
>>> Question.objects.filter(id=1) # filter는 부분검색을 하기 위해
>>> Question.objects.filter(question_text__startswith='What') # __ 이거 뒤부터는 조건이 들어간다.
>>> from django.utils import timezone
>>> current_year = timezone.now().year
>>> Question.objects.get(id=2)
>>> Question.objects.get(pk=1)
>>> q = Question.objects.get(pk=1)
>>> q.was_published_recently()
```

Question 은 q = asd 됬는데
하지만 Choice는 Question에 대해 연관 지어지는 데이터라서 혼자서 진행 불가 따라서 
* q.choice_set.all() # 이런식으로 진행 가능하다.


```
>>> q.choice_set.all()
>>> q.choice_set.create(choice_text = 'pig')
>>> q.choice_set.create(choice_text = 'chiken')
>>> q.choice_set.create(choice_text = 'cow')
>>> from django.utils import timezone
>>> q2 = Question(question_text = '짜장? 짬뽕?', pub_date = timezone.now())
>>> q
>>> q2
>>> q2.save()
>>> q2.choice_set.create(choice_text='자장')
>>> q.choice_set.all() # q 만 보여줌
>>> q2.choice_set.all() # q2 만 보여줌
>>> Choice.object.all() #전체를 다보여줌
```

따로 설명 부분이 미흡 하지만 직접해보면서 하는게 제일 이해가 잘될듯 하다.
