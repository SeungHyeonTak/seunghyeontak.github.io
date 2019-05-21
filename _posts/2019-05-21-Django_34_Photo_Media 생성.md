---
layout: post
title:  "Django_34.(SNS 이미지 앱 만들기)Photo 모델 생성 및 Media_url 설정"
comments: true
description: "Django_part34"
author: SeungHyeon Tak
date:   2019-05-21 20:13:54 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.34 웹 개발
<br>
* 바로 보이는 서비스가 아닌 디버깅으로 사진이 보이게 하기위한 방법을 먼저 해보겠습니다.
* 먼저 `settings.py`에서 추가해줄 부분

```python
# 제일 마지막 부분에 media url을 추가해준다.
MEDIA_URL = '/files/' # 이 경로로 이동하게 해준다.
MEDIA_ROOT = os.path.join(BASE_DIR,'uploads') # 어디에 저장하는지 경로 저장해줌 uploads를 만들어줌으로써 여기에 사진을 저장함
```
<br>
* `models.py`

```python
from django.db import models
from django.conf import settings

# 2 입력
# instance는 Photo가 되고 filename은 우리가 올린 사진 파일이 된다.
def user_path(instance, filename):
# 저장 제목을 무작위 문자열로 저장한다.
    from random import choice
    import string
    # choice는 무작위로 뽑아오는것
    arr = [choice(string.ascii_letters) for _ in range(8)]
    pid = ''.join(arr)
    extension = filename.split('.')[-1] # 파일 확장자 가져오기
    # admin/absdfer.png 이런식으로 저장됨
    return '%s/%s.%s' % (instance.owner.username, pid, extension)
    
## 중요!! 이미지를 쓰려면 밑의 부분이 필요
# runserver 하기 전에 pip install pillow 를 설치 해야함

# 1 입력
class Photo(models.Model):
    # upload_to 를 하지 않으면 무작위로 아무렇게 나저장됨
    # 함수를 지정해서 저장해본다. 위의 def 참조
    image = models.ImageField(upload_to= user_path)
    # 한명의 사용자가 여러 사진을 올릴 수 있다.
    # 1:N / N:1 로 Many TO owner 으로 구성된다.
    # Django의 os 모델을 쓰려면 밑의 Foreinkey 처럼 써라
    owner = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    thumnail_image = models.ImageField(blank=True) # blank를 쓰면 이미지 넣어줘도 되고 안넣어 줘도 되고
    comment = models.CharField(max_length=255)
    pub_date = models.DateTimeField(auto_now_add=True)
    # 업로드 하는 순간 저장됨
```
<br>

위의 함수쓰이는게 궁금하면 터미널창에서 직접 써볼 수 있다.

```vim
$ python
>>> import string
>>> string.ascii_letters
# 하면 'a소문자부터Z대문자까지 문자열이 나열된다.'
>>> arr = [choice(string.ascii_letters) for _ in range(8)]
>>> arr
# 하면 무작위 8글자가 나옴
>>> ''.join(arr)
# join으로 합쳐주면 이런식으로 나온다.
```
<br>

* `admin.py`에 등록

```python
from django.contrib import admin
from .models import Photo
# Register your models here.

admin.site.register(Photo)
```
<br>

* 이렇게 admin을 입력하고
* runserver를 진행하기전에

```
$ python manage.py makemigrations
$ python manage.py migrate
# 를 실행하고 runserver 한다.
```
<br>

* 관리자 페이지에서 Photo에서 사진을 일단 하나만 저장해본다. 그리고 그 저장한 부분을 들어가 사진을 저장한 경로로 들어가본다.
* 들어가기전에 터미널에서 경로로 잘들어갔는지 pycharm 터미널로 경로 확인해본다.

```vim
$ ll # 경로 확인
$ tree uploads # uploads의 경로 확인 가능
```
<br>

* admin 페이지 저장된 경로 로 들어가면 에러가 뜰것이다.
* 여기는 배포와 관련된 문제이다.
* `urls.py` 에서 설정 하나 해줘야 한다.

```python
from django.conf import settings
from django.conf.urls.static import static

urlpatterns =[
	...
] + static(settings.MEDIA_URL,document_root = settings.MEDIA_ROOT)
# 이제 이미지가 잘 뜰것이다.
# 이부분은 디버깅 용도로 쓰이고 실제 서비스에서는 안쓰인다.
```

