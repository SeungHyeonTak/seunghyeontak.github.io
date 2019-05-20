---
layout: post
title:  "Django_19.(설문조사 만들기)Templates 연동하기"
comments: true
description: "Django_part19"
author: SeungHyeon Tak
date:   2019-05-03 12:15:11 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.19 웹 개발

* View에서 내용을 조금 바꿔서 templates와 연동 할 수 있게 한다.

```python
from .models import Question

def index(request):
    latest_question_list = Question.objects.order_by('pub_date')[:5]
    # order_by는 정렬 시키는것이다. 아무것도 없이 하면 오름차순 / '-'가 있으면 내림차순
    # [:5]는 최근에 5개의 목록만 보이게 할 수 있는것 (이런것도 할 수 있다 알고 있기)

    output = ",".join([q.question_text for q in latest_question_list])
    return HttpResponse(output)
    # templates를 연동하기전 확인해볼때 쓰이는 방법
```
* 정보가 보여지지 않는다면 관리자 페이지로 이동해서 Question에서 정보를 몇가지 더 추가 하고 확인하는 방법이 있음!!

##### templates와 연동하는 방법

* 일단 `polls` 폴더에 `templates` 라는 폴더를 만들고 그 밑에 `polls` 라는 폴더를 또 만들어주고 그 안에 `index.html`이라는 파일을 만들어준다.
내용은 github에서 확인 가능

<https://github.com/SeungHyeonTak/HTML-study/blob/master/templates.html>

* 다시 view로 돌아와서 수정해준다.

```python
def index(request):
    latest_question_list = Question.objects.order_by('pub_date')[:5]
    context = {'latest_question_list': latest_question_list}

    return render(request, "polls/index.html", context) # 마지막엔 object를 넘겨준다.

```