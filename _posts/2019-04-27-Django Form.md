---
layout: post
title:  "Django Form 생성"
comments: true
description: "Django Form 생성"
author: SeungHyeon Tak
date:   2019-04-27 16:12:00 +0700
categories: [Django]
tags: [Django]
keywords: "Django Form"
---
#### Django Form이란?

데이터 입력받는 Form

* app 폴더 밑에 `forms.py`를 생성해준다.

```
from django import forms
from .models import GuessNumbers

class PostForm(forms.ModelForm):

    class Meta:
        model = GuessNumbers

	# admin 페이지에서 입력받았던걸 web페이지에서 입력하기 받기 위해서
        fields = ('name', 'text', 'lottos',)
```

* `views.py` 추가

```
from .models import Numbers
from .forms import PostForm


def post(request):
	form = PostForm()
	return render(request, 'mysite/form.html', {"from":from})
```


* `urls.py`에서 경로 생성

```
path('mysite/new/',views.post, name = "now_lottos"),
```

* `form.html` 작성하기
<https://github.com/SeungHyeonTak/HTML-study/blob/master/form.html>

* POST 처리
   * `views.py`에서 post를 생성

```
def post(request):
    if request.method == "POST":
        # save data
        form = PostForm(request.POST)
        if form.is_valid():
            lotto = form.save(commit=False)
            lotto.generate()
            return redirect('index')
    else:
        form = PostForm()
        return render(request, 'mysite/form.html',{"form":form})
```
