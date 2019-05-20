---
layout: post
title:  "Django_23.(설문조사 만들기)generic view 적용하기"
comments: true
description: "Django_part23"
author: SeungHyeon Tak
date:   2019-05-07 20:21:12 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.23 웹 개발

#####  generic view 적용하기

* 기본적으로 url을 라우팅하는 방법이 몇가지가 있다.
   * Function views 방법
      * path('', views.home, name='home')
   * Class-based views 방법
      * path('', Home.as_view(), name='home')
   * Including another 방법
      * path('blog/', include('blog.urls'))

* Function views와 Class views의 차이
   * Function은 if-else 로 처리해야함
   * Class는 각각의 http메서드들을 클래스의 한 메서드랑 연결 시켜줄 수 있다.(객체지향의 장점을 이용하여 가독성이 좋게 구현 할 수 있다.)

* generic view
   * 자주사용하는 기능을 Django에서 미리 제공해준다.
코드로 보면

`polls/urls.py`

```python
urlpatterns = [
    path('',views.IndexView.as_view(), name = 'index'),
    path('<int:pk>/', views.DetailView.as_view(), name = "detail"),
    path('<int:pk>/results/', views.ResultsView.as_view(), name = "results"),
    path('<int:question_id>/vote/', views.vote, name = "vote"),
]
```

`views.py`

```
from django.shortcuts import render, get_object_or_404,redirect, HttpResponseRedirect

# Create your views here.
from django.http import HttpResponse
from django.urls import reverse

from .models import Question
from django.views import generic

class IndexView(generic.ListView):
    template_name = 'polls/index.html'
    context_object_name = 'latest_question_list' #어떤 object를 가져올거냐?
	# 파라미터를 무슨 이름으로 넘길 것이냐?
	
    def get_queryset(self): # 메소드 오버라이딩
        return Question.objects.order_by('-pub_date')[:5]

class DetailView(generic.DetailView):
	# 어느 모델과 연결해서 어느 템플릿으로 넘겨 줄지 정의한다.
    model = Question
    template_name = 'polls/detail.html'

class ResultsView(generic.DetailView):
    model = Question
    template_name = 'polls/results.html'

def vote(request, question_id):
    question = get_object_or_404(Question, pk = question_id)
    try:
        selected_choice = question.choice_set.get(pk = request.POST['choice'])
    except:
        return render(request, 'polls/detail.html', {
            'question':question,
            'error_message':"You didn't select a choice."
        })
    else:
        selected_choice.votes += 1
        selected_choice.save()
        # return redirect(reverse('polls:results', question_id = question_id))
        # 이 방법에 대한 오류가 떠서 밑의 방식으로 진행 하였다.
        return HttpResponseRedirect(reverse('polls:results', args=(question.id,)))
```