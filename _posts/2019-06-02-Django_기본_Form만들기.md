---
layout: post
title:  "Django_기본.Form만들기"
comments: true
description: "Django"
author: SeungHyeon Tak
date:   2019-06-02 21:56:00 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.기본 웹 개발

* Form 만들기
 - models.py로 부터 Document를 받아온다.
<br>
* `forms.py`

```python
from django import forms
from .models import Document
class DocumentForm(forms.ModelForm):
    class Meta:
        model = Document
        fields = ['category','title','slug','text','image']
```

* `urls.py` 파일에서 form 경로를 지정한다.

```python
from django.urls import path
from .views import document_list, document_detail, document_create, document_update
app_name = 'board'

urlpatterns = [
    path('update/<int:document_id>',document_update, name = 'update'),
    path('create/',document_create, name = 'create'),
    path('detail/<int:document_id>', document_detail, name = 'detail'),
    path('',document_list, name = 'list')
]
```

* `views.py`

```python
from .forms import DocumentForm

def document_create(request):
    # Document.object.create() - 실행과 동시에 DB에 삽입
    # 분기 - post, get
    if request.method == "POST":
        # 처리
        # request.POST : 폼에서 입력한 텍스트 데이터
        # request.FILES : 파일

        form = DocumentForm(request.POST, request.FILES)
        form.instance.author_id= request.user.id
        if form.is_valid():
            document = form.save()
            return redirect(reverse('board:detail', args=[document.id]))
    else:
        # 입력 창
        form = DocumentForm()

    return render(request, 'board/document_create.html',{'form':form})
```

request.method == 'post' -> bool값 True이다.
사용자의 현재 요청이 HTTP "POST"메서드를 사용하여 수행되었거나 그렇지 않으면 False이다.
