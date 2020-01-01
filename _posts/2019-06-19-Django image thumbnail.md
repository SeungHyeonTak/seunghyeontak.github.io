---
layout: post
title:  "Django Image Thumbnail"
comments: true
description: "Django Image Thumbnail"
author: SeungHyeon Tak
date:   2019-06-19 23:46:21 +0700
categories: [Django]
tags: [Django]
keywords: "Django, Image 썸네일, Image Thumbnail"
---
## Django Image Thumbnail

#### 이미지 썸네일 만들기(PILKit)
* JPEG : 손실압축 포맷, 파일 크기가 작아 웹에서 널리 사용
  * 압축률을 높이면 파일크기는 더 작아지나 이미지 품질은 떨어짐(1~100%, 대개 60~80%가 적정선)
  * 투명채널은 지원되지 않으며, 사진 이미지에 유용
* PNG : 투명채널 지원하며, 24비트 색상 지원
  * 투명이 필요하거나, 문자가 있는 이미지는 PNG포맷이 유리
* GIF : 256색까지 지원, 애니메이션 지원
<br>
#### python 이미지 처리 라이브러리
* PIL(python image library)
* Pillow
* PILKit
* Wand
<br>
#### PILKit

```
# 설치 명령어
$ pip install pilkit
```

<br>
#### 이미지 썸네일 Helper
* django-imagekit 설치

```
$ pip install pillow
$ pip install django-imagekit
```
<br>
```python
INSTALLED_APPS = [
    ...,
    'imagekit', # 추가
]
```

* ImageField로부터 썸네일 Field 생성

```python
# 방법 1
# models.py
class Post(models.Model):
    	...
	photo = models.ImageField(blank=True, upload_to='blog/post/%Y/%m/%d')
	photo_thunbnail = ImageSpecField(source='photo',
		                         processors=[Thumbnail(300,300)],
		                         format='JPEG',
		                         options={'quality':60})


# 방법 2
class Post(models.Model):
    ...
    photo = models.ImageField(blank=True, upload_to='blog/post/%Y/%m/%d',
                              processors=[Thumbnail(300,300)],
                              format='JPEG',
                              options={'quality':60})
```

<br>
#### thumbnail Template Tag 활용
<br>
![qwcs1](https://user-images.githubusercontent.com/46446165/59698885-8f7b9200-922b-11e9-9988-f850b8e72f6f.png)
