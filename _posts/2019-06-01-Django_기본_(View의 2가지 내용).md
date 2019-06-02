---
layout: post
title:  "Django_기본.(Django에서 View란?)"
comments: true
description: "Django"
author: SeungHyeon Tak
date:   2019-06-01 14:13:54 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.기본 웹개발

* View 
  * 1개의 HTTP 요청에 대해 -> 1개의 뷰가 호출
  * 크게 2가지 형태의 뷰
    * 함수 기반 뷰(Function Based View) FBV - 장고 뷰의 기본(처음 공부 할땐 FBV를 먼저 써보는걸 추천)
    * 클래스 기반 뷰(Class Based View) CBV - 클래스.as_view()를 통해 객체 처리
<br>
* View의 HttpRequest 객체
  * 현재 요청에 대한 모든 내역을 담고 있다.

* View의 URL Captured Values
  * url/re_path를 통한 처리에서는 -> 모든 인자는 str 타입으로 전달
  * path를 통한 처리에서는 -> 매핑된 Converter의 to_python에 맞게 변환된 값이 인자로 전달

<br>
* Django에서 View는 2가지로 나뉘어진다.
<br>
* class형 view (클래스형 뷰)
  -Forms : 입력폼 생성, 입력값 유효성 검사 및 DB로의 저장
  -테스팅
  -국제화&지역화
  -캐싱
  -Geographic : DB의 Geo 기능 활용 (PostgreSQL 중심)
  -Sending Emails
  -Sysdication Feeds (RSS/Atom)
  -Sitemaps - 검색 엔진 최소화

  * Generic view
    - 위의 반복되는 공통부분을 패턴화 하여 사용하기 쉽게 추상화함
    - 아래와 같이 최소한 그 뷰가 어떤 모델을 사용할 것인지만 지정해주면 나머진 모두 상위 클래스가 모든걸 알아서 해줌

```
> form_class : 사용자에 보여줄 폼을 정의한 forms.py 파일 내의 클래스명<br>
> template_name : 폼을 포함하여 렌더링할 템플릿 파일 이름<br>
> success_url : MyFormView 처리가 정상적으로 완료되었을 때 리다이렉트시킬 URL<br>
> form_valid() 함수 : 유효한 폼 데이터로 처리할 로직 코딩, 반스시 super() 함수 호출.<br>
```

<br>

```python
from django.views.generic import ListView

class IndexView(ListView):
    model = Question
```

* def형 view (함수형 뷰)
  - HTTP 요청 처리
    - Models : 데이터베이스와의 인터페이스
    - Templates : 복잡한 문자열 조합을 보다 용이하게 주로 HTML문자열 조합 목적으로 사용하지만, 푸쉬 메세지나 이메일 내용을 만들 때에도 쓰면 편리
    - Admin 기초 : 심플한 데이터베이스 레코드 관리 UI
    - Logging : 다양한 경로로 메세지 로깅
    - Static files : 개발 목적으로 정정인 파일 관리
    - Messages framework : 유저에게 1회성 메세지 노출 목적

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




