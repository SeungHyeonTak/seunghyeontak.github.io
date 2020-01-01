---
layout: post
title:  "Django WYSIWYG 사용하기"
comments: true
description: "Django WYSIWYG"
author: SeungHyeon Tak
date:   2019-06-20 09:45:21 +0700
categories: [Django]
tags: [Django]
keywords: "Django WYSIWYG"
---
## Django WYSIWYG

#### WYSIWYG
What you see is what you get의 약어로 웹 브라우저에서 사용할 수 있는 HTML편집기를 의미한다.
일반적으로 웹에서 긴 내용의 텍스트를 입력받을 때는 textarea태그를 사용한다.
쉽게 말해 블로그나 게시판에서 볼 수있는 텍스트 편집기라고 볼 수 있다.
<br>
* django-ckeditor 모듈을 다운받아 설치한다.

```
$ pip install django-ckeditor
```

* settings.py 파일에 있는 `INSTALLED_APPS`에 `ckeditor`를 추가한다.

```python
INSTALLED_APPS=[
	...
	'ckeditor',
]
```

* 모델에 새로운 필드를 추가한다.

```python
from django.db import models
from ckeditor.fields import RichTextField

class Post(model.Model):
	content = RichTextField()
```

* 이후 makemigrations - migrate 진행 
* 관리자 페이지에서 해당 필드가 에디터 형태로 나타나는지 확인
* 우리가 보는 프론트 페이지에서 나타나게 하기
<br>
![forms](https://user-images.githubusercontent.com/46446165/59713991-8ef2f380-924b-11e9-929d-b5cf501b43c6.png)
<br>

#### CKEditor 이미지 파일 업로드 설정하기
ckeditor에서 이미지 버튼으로 이미지 업로드를 할 수 있는 부분을 설정할 수 있다.

* django-ckeditor를 설치

```
$ pip install django-ckeditor
```
<br>
* settings.py의 INSTALLED_APPS항목에 추가 내용

```python
INSTALLED_APPS = [
	...
	'ckeditor',
	'ckeditor_uploader',
]
```
<br>
* settings.py에 MEDIA_ROOT를 꼭 지정해야함 (S3를 사용하는 경우에는 따로 지정할 필요 없다.)

```python
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```
<br>
* 만약 media_root 하위 폴더에 에디터용 전용 경로를 설정하고 싶다면 CKEDITOR_URLOAD_PATH 값을 추가한다.

```
CKEDITOR_UPLOAD_PATH = 'wysiwyg/'
```
<br>
* 에디터를 사용하는 유저별로 자신이 올린 파일들만 사용할 수 있게하려면 

```
CKEDITOR_RESTRICT_BY_USER = True
```
<br>
* 에디터를 설정할 필드를 모델에 만들어준다.

```python
from ckeditor_uploader.fields import RichTextUploadingField

class Post(models.Model):
	content = RichTextUploadingField()
```

* 이후 변경사항이 생겼으므로 마이그레이션 작업을 한다.
* 업로더가 잘 실행되려면 urls.py에 필요한 내용을 등록해야 한다.

```python
urlspatterns = [
	...
	path('ckeditor/',include('ckeditor_uploader.urls')),
]
```

* 이제 잘되는지 확인해본다.
