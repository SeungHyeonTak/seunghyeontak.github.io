---
layout: post
title:  "Django_18.(설문조사 만들기)View / urls 파일 수정"
comments: true
description: "Django_part18"
author: SeungHyeon Tak
date:   2019-05-02 15:05:11 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.18 웹 개발

* `views.py`로 가서 내용 추가

```
def results(request, question_id):
    return HttpResponse("You're looking at the result of question : %s" % question_id)

def vote(request, question_id):
    return HttpResponse("You're voting on question : %s" % question_id)

def detail(request, question_id):
    return HttpResponse("You're looking at qeustion : %s." % question_id)
```

* 각각 페이지로 가서 확인할 수 있도록 설정해준다.

* `polls/urls.py`로 가서 url설정을 해준다.

```
urlpatterns = [
    ...,
    path('<int:question_id>/',views.detail, name = "detatil"),
    path('<int:question_id>/results/',views.results, name = "results"),
    path('<int:question_id>/vote/',views.vote, name = "vote"),
]
```

* 참고
   * 항상 경로 입력하고 마지막에는 / 로 닫아주는걸 습관화 하자
   * path() 입력을 다하고 마지막에 , 찍는것도 습관화 하자

그리고 각각 경로로 들어가서 잘 되어있는지 확인한다.
<127.0.0.1:8000/polls/1/results> 
바꿔가며 확인해보자!