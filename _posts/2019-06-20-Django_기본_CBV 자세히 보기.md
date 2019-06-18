---
layout: post
title:  "Django_기본.CBV 자세히 보기"
comments: true
description: "Django"
author: SeungHyeon Tak
date:   2019-06-20 08:12:21 +0700
categories: [Django]
tags: [Django]
keywords: "Django, CBV"
---
#### Django.기본 웹 개발

#### Class Based View (CBV)

* 뷰에서 호출가능한 객체(Callable Object)
* CBV의 정체는 FBV를 만들어주는 클래스
  * as_view클래스 함수를 통해 함수뷰를 생성
* Django 기본 CBV 팩키지 위치 : Django.views.generic
  * CBV는 범용적(generic)으로 쓸 뷰


* 함수형 뷰에 대해 알아보기

```python
from django.shortcuts import get_object_or_404, render

def post_detail(request,pk):
    post = get_object_or_404(Post, pk=pk)
    return render(request, 'blog/post_detail.html', {'post':post})
```

* 함수를 통해 뷰 생성 버전

```python
def generate_view_fn(model):
    def view_fn(request, id):
	instance = get_object_or_404(model, pk=pk)
	instance_name = model._meta.model_name
	template_name = f'{model._meta.app_label}/{instance_name}_detail.html'
	return render(request, template_name, {instance_name: instance}
    return view_fn
```

<br>

* CBV형태로 컨셉 구현

```python
class DetailView(object):
    def __init__(self, model):
	self.model = model

    def get_object(self, *args, **kwargs):
	return get_object_or_404(self,model, pk=kwargs['id'])

    def get_template_name(self):
	return f'{self.meta.app_label}/{self.model._meta.model_name}_detail.html'

    def dispatch(self, request, *args, **kwargs):
	return render(request, self.get_object_name(),{
			self.model._meta.model_name: self.get_object(*args, **kwargs),
	})

    @classmethod
    def as_view(cls, model): # python에서 cls는 자체를 표현하는것이다.
	def view(request, *args, **kwargs): # python 문법
	    return self.dispatch(request, *args, **kwargs)
	return view


post_detail = DetailView.as_view(Post) # 클래스를 통해서 as_view를 호출 가능 / 첫번째인자로 모델을 지정
```

* 간단하게 구현하기

```python
from django.views.generic import DetailView

post_detail = DetailView.as_view(model=Post, pk_url_kwarg='pk')
```

* CBV는 가져다쓸때는 심플하고 좋으나 정해진 시나리오를 따를 때만 심플하다.
(FBV랑 섞어서 적절하게 써라)

* 기본 제공 CBV
  * Generic Display View
    * ListView, DetailView
  * Generic Editing View
    * FormView, CreateView, UpdateView, DeleteView
  * Generic DateViews
    * DateDetailView, ... 등등 많이 있다.

* ListView (get 요청에 포커스가 맞춰져 있다.)
  * paginate_by 옵션 : 페이지당 갯수 지정
  * 디폴트 템플릿 경로 : '앱이름/모델명소문자_list.html'
  * 디폴트 context object name : '모델명소문자_list'
    * ex) post_list, comment_list, tag_list

```python
# sample code
# views.py

from django.views.generic import ListView

post_list = ListView.as_view(model=Post) # 페이징 없이 전체 노출
post_list = ListView.as_view(model=Post, paginate_by = 10) # 페이지당 10개씩 노출

# 현재는 다음 페이지 적용을 안하였기 때문에
# /list/?page=2/ <- 이런식으로 확인 가능함(paginate_by를 적용했을때)
```
<br>

* Templates에서의 적용 방법
<br>
![asdad112](https://user-images.githubusercontent.com/46446165/59703632-429cb900-9235-11e9-9d6f-0ee8315e440b.png)
<br>

* DetailView (get 요청 / pk 혹은 slug의 모델 인스턴스에 대한 Detail)
  * 디폴트 템플릿 경로 : '앱이름/모델명소문자_detail.html'
  * 디폴트 context object name : '모델명 소문자'
    * ex)post, comment, tag

```python

post_detail = DetailView.as_view(model=Post, pk=pk)
```

<br>

* Editing (CeateView / UpdateView)
(get/post 요청) - modelform을 통해 model instance를 생성/수정
  * model 옵션 (필수)
  * GET 요청 : 입력 Form을 보여주고, 입력이 완료되면 같은 URL로 POST 요청
  * POST 요청 : 입력받은 POST데이터에 대해 유효성 검사를 수행하고
    * invalid 판전 시 : Error가 있으면 다시 입력 Form을 보여준다.
    * valid 판전 시 : 데이터를 저장하고, success_url로 이동
  * field옵션 : form_class 미제공 시, 지정 field에 대해서만 Form 처리
  * success_url 옵션 : 제공하지 않을 경우, model_instance.get_absolute_url() 획득을 시도
    (가장 자연스러운건 success_url이 DetailView로 이동하는것이다.)
  * 디폴트 템플릿 경로 : "앱이름/모델명소문자_form.html"

```python
# get_absolute_url 쓰는법 (Detail에 쓰기)

def get_absolute_url(self):
    return reverse('blog:post_detail', args=[self.id])
```
<br>

* Create / Update - success_url 잘쓰기


* DeleteView(get/post 요청)
  * model 옵션 (필수)
  * success_url 옵션 (필수)
  * 디폴트 템플릿 경로 : "앱이름/모델명소문자_confirm_delete.html"
  * GET 요청 : 삭제의사를 물어본다.
  * POST 요청 : 삭제를 수행하고, 지정된 success_url로 이동
  * 단순한 삭제 의사만 표현하기 때문에 form은 크게 쓸필요 없다.
<br>
* reverse_lazy() -> 평가가 지연된다.(게으르다?) 좀 더 알아보고 잘 활용하기
generic view에서만 쓰임 reverse(x) / reverse_lazy(o)
url 문자열로 쓰지말고 reverse_lazy() 활용
타이밍 로딩 문제때문에 사용
success_url에 사용

