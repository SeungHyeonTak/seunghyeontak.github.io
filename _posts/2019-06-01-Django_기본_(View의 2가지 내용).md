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

#### 함수형 뷰(FBV) 및 URL 패턴 사용법

* HttpResponse

```python
# blog/urls.py

from . import view

urlpatterns =[
	path('detial/<int:pk>/', views.postdetail, name='postdetail'),
]
# 뒷부분의 name은 views.py에 함수 name과 같게 하는것이 관용적이다.

# blog/views.py

from django.http import HttpResponse

def postdetail(request):
	return HttpResponse("""<h2>HttpResponse</h2><p>FBV 내용</p>""")
```

* render
 * render()함수는 django.shortcuts 패키지에 포함되어있는 함수이다.
 * 동적으로 template을 이용하여 html을 생성 후 httpresponse에 포함해 반환가능
 * reqeust와 템플릿 파일명이 필수

```python
# postdetail.html에서 내용 작성 후

# blog/urls.py

urlpatterns = [
	path('detail/<int:pk>/', views.postdetail, name='postdetail'),
]

# blog/views.py

def postlist(request):
	post_list = Post.objects.all() # queryset
	return render(request, "blog/postlist.html", {'post_list':post_list})

def postdetail(request,pk):
	post_detail = get_object_or_404(Post,pk=pk)
	return render(request, "blog/postdetail.html", {'post_detail':post_detail})
```

* get_object_or_404
 * 왜 쓰는가? - try-except 블록으로 예외처리를 할 필요가 없다.
 * View에서만 사용
 * get_object_or_404(모델_클래스, 검색 조건)
   * 몇번 데이터 : pk (ex) pk=detail_id or pk=pk)

* redirect
 * redirect() 함수는 django.shortcuts 패키지에 포함되어 있는 도우미 함수이다.
 * 다른 url로 변경해주는 역할을 함

```python
# blog/urls.py

urlpatterns = [
	path('detail/<int:pk>/',views.postdetail),
]

# blog/views.py

def postdetail(request):
	return redirect('/blog/create/')
```




