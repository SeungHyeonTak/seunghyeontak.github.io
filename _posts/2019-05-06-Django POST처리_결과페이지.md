---
layout: post
title:  "Django POST처리 / 결과페이지 만들기"
comments: true
description: "Django POST 처리"
author: SeungHyeon Tak
date:   2019-05-06 17:22:12 +0700
categories: [Django]
tags: [Django]
keywords: "Django POST 처리"
---
## Django POST 처리

##### *POST 처리하기*

* 기본적으로 form에서는 데이터가 저장되지 않는다.
* 저번에는 detail.html 부분에서 vote라는 메서드로 보내줬던 내용이 있다.
* views.py의 vote부분에서 수정하면 된다.

```python
from django.shortcuts import render, get_object_or_404, redirect

def vote(request, question_id):
    question = get_object_or_404(Question, pk = question_id)
    try:
        selected_choice = question.choice_set.get(pk = request.POST['choice'])
    except:
        return render(request, 'polls/detail.html', {
            'question':question,
            'error_message':"You didn't select a choice."
        })
    else: #에러 안나고 정상적으로 처리될때 실행됨
        selected_choice.votes += 1
        selected_choice.save()
        return redirect('polls:results', question_id = question_id)
```

*****

##### *결과페이지 만들기*

* 계속 아껴두었던 results 페이지를 써야할 시간입니다.
* 투표의 결과를 확인할 페이지를 보여지기 위해선 results 부분을 수정해야 하는데

일단 templates에 results.html을 생성해주고 다음 내용을 입력한다.<br>
<https://github.com/SeungHyeonTak/HTML-study/blob/master/results.html>

그다음 `views.py` 의 results 메서드에서 다음과 같이 수정한다.

```
def results(request, question_id):
    question = get_object_or_404(Question, pk = question_id)
    return render(request, 'polls/results.html', {'question':question})
```

그리고 runserver를 돌려서 투표를 하면 results에 투표 결과가 나오게 된다.
※ 만약 에러가 난다면 분명 오타일 가능성이 크다(경험)
