---
layout: post
title:  "Django_35.(SNS 이미지 앱 만들기)Photo form 생성 / login 템플릿 수정"
comments: true
description: "Django_part35"
author: SeungHyeon Tak
date:   2019-05-21 20:13:54 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.35 웹 개발
<br>
* 이미지 업로드 계속해서 진행
* `forms.py`

```python
from .models import Photo
...
# 이미지 업로드 폼 만들기
class UploadForm(forms.ModelForm):
    comment = forms.CharField(max_length=255)
    class Meta:
        model = Photo # 내부 클래스에다 Photo 모델과 연결 시켜준다.
        exclude = ('thumnail_image', 'owner')
        # 포함 하지 않는 필드를 쓸 수 있다(exclude)
        # owner는 왜 안쓸까? 내 이미지이기 때문에 따로 지정안해도됨(코드로 설정)
```
<br>

* `urls.py`

```python
from . import views

app_name = 'insta'

urlpatterns = [
	...
    path('upload/',views.upload, name = 'upload'),
]
```
<br>
* `views.py`

```python
from .forms import CreateUserForm, UploadForm

def upload(request):
    form = UploadForm() # 기본 form 처리
    return render(request, 'insta/upload.html', {'form':form})
# 이후 경로를 insta/upload.html로 해줬으니 template을 만들어준다.
```
<br>

* `templates/insta/upload.html`<br>
<a href="https://github.com/SeungHyeonTak/HTML-study/blob/master/insta/upload.html">upload code</a>
<br>
* 이제 insta/upload로 접속해보면 upload창이 뜬다.
<br>

*****

#### POST 처리

* `views.py`

```python
from django.shortcuts import render, redirect
from django.contrib.auth.decorators import login_required
...

@login_required
def upload(request):
    if request.method == "POST":
        form = UploadForm(request.POST, request.FILES)
        # 대용량파일(이미지)을 처리해야하기 때문에 request.POST와 request.FILES를 매개변수로 넣어줘야한다.
        if form.is_valid():
            photo = form.save(commit=False) # 이렇게 하면 Photo 객체를 가져오긴 하는데 DB에 저장되진 않는다.
            photo.owner = request.user
            form.save() # 이때 제대로 DB에 저장
            return redirect('insta:index')
    form = UploadForm()
    return render(request, 'insta/upload.html', {'form':form})

```
<br>

* 이제 정상적으로 업로드 되는걸 관리자 페이지에서 확인 가능하다.
<br>

* 마지막으로
* 로그아웃 하고 upload 페이지로 가보면 이상하게 upload가 된다.
* 이를 고치려면

```python
from django.contrib.auth.decorators import login_required
# 밑의 이부분만 추가 해주면 가능
@login_required
def upload(request):
    if request.method == "POST":
        form = UploadForm(request.POST, request.FILES)
        if form.is_valid():
            photo = form.save(commit=False)
            photo.owner = request.user
            form.save()
            return redirect('insta:index')
    form = UploadForm()
    return render(request, 'insta/upload.html', {'form':form})
```
<br>
* `login.html` 부분에서 value값만 추가
<br>
![21](https://user-images.githubusercontent.com/46446165/58098162-eb92cc80-7c13-11e9-8437-73fc39b1d05b.png)

