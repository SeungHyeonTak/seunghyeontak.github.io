---
layout: post
title:  "Django_27.class형 / def 형을 이용한 Views의 쓰임"
comments: true
description: "Django_part27"
author: SeungHyeon Tak
date:   2019-05-13 21:40:00 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.27 웹 개발
<br>
<br>
view를 쓸때 class형 / def형 둘다 쓰면서 능숙해지기

* 함수형 뷰를 쓸때 두가지 방법
 - HttpResponse를 이용한 함수형 뷰
   - template을 읽어 HttpResponse로 응답하는 과정 (번거롭고 복잡함 / 가장 기초 적인 부분) - 클래스뷰의 필요성과 원리를 이해할 때 도움됨

```python
def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    template = loader.get_template('polls/index.html')
    
    context = {
        'latest_question_list': latest_question_list,
    }
    
    return HttpResponse(template.render(context, request))
```

 - render를 이용한 함수형 뷰
   - shortcuts.render를 사용하여 보다 간결하게 표현한 모습이다. 위 코드의 template = ... 로 시작하는 부분이 사라짐 그리고 제일 하단에 HttpResponse로 리턴하던 것이 render함수로 대체함

```python
from django.shortcuts import render

def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]

    context = {
        'lastest_question_list': latest_question_list,
    }

    return render(request, 'polls/index.html', context)
```

* 클래스형 뷰
 - Generic view
   - 위의 반복되는 공통부분을 패턴화 하여 사용하기 쉽게 추상화함
   - 아래와 같이 최소한 그 뷰가 어떤 모델을 사용할 것인지만 지정해주면 나머진 모두 상위 클래스가 모든걸 알아서 해줌

> form_class : 사용자에 보여줄 폼을 정의한 forms.py 파일 내의 클래스명<br>
> template_name : 폼을 포함하여 렌더링할 템플릿 파일 이름<br>
> success_url : MyFormView 처리가 정상적으로 완료되었을 때 리다이렉트시킬 URL<br>
> form_valid() 함수 : 유효한 폼 데이터로 처리할 로직 코딩, 반스시 super() 함수 호출.<br>

<br>
<br>

```python
from django.views.generic import ListView

class IndexView(ListView):
    model = Question
```


※ Q. 클래스형 뷰가 함수형 뷰를 완전히 대체하는것인가??
A. 아니다
- 두가지의 뷰는 상황에 따라 선택하여 쓰는것이고 한쪽을 더 진보된 형태라고 말하기 힘들다
- 그때 상황에 따라 쓰자

두 뷰의 쓰임을 urls 에 적용 하는법도 다름 
class view는 <int:pk>
def view는 <int:main_id> 
이런식으로인것 같다.
