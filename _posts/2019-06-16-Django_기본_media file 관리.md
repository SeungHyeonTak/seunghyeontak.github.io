---
layout: post
title:  "Django_기본.media file 관리하기"
comments: true
description: "Django"
author: SeungHyeon Tak
date:   2019-06-16 23:47:00 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.기본 웹 개발

#### Media files
* 유저가 업로드한 모든 정적인 파일(image, etc)
  * 프로젝트 단위로 저장/서빙

#### 관련 settings 예시
뷰에는 HttpRequest.FILES를 통해 파일이 전달되고, 뷰에서는 이를 적절히 검증한 후, settings.MEDIA_ROOT 디렉토리 하단에 저장
<br>
* MEDIA_URL = '/media/'
* MEDIA_ROOT = os.path.join(BASE_DIR, 'media')

#### FileField
File Storage API를 통해 파일을 저장 해당 필드를 옵션필드로 두고자 할 경우, blank=True옵션 적용

* ImageField(FileField 상속)
  * Pillow를 통해 이미지 width/height 획득
  (Pillow : 이미지 처리 라이브러리)

```
$ pip install pillow
# 이 명령어로 설치
```

* 이후 models.py에 ImageField를 추가 하고 makemigrations를 해준다.
* admin 사이트에서 사진을 추가 하고 정상적으로 올라간 사진을 보려고 클릭하면 아직 사진은 보여지지 않는다.

#### 개발환경에서의 media 파일 서빙
* static files와 다르게 개발서버에서 서빙 미지원
* 개발 편의성 목적으로 직접 서빙 Rule 추가 기능

```python
# config/urls.py

from django.conf import settings
from django.conf.urls.static import static

...

urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

* 이제 다시 새로고침하면 이미지가 정상적으로 보일것이다.

#### 파일 업로드 시의 form enctype
* form method는 필히 POST : GET method는 enctype이 'application/x-www-form-urlencoded'로 강제됨
* form enctype은 필히 "multipart/form-data" : "application/x-www-form-urlencoded"로 지정될 경우, 파일명만 전송
* views.py 에서는 request.POST, request.FILES 를 꼭 추가 해야한다.(순서틀리면 안됨)
* 타이핑시 오타나면 안됨
<br>

#### 파일 저장경로 커스텀 by upload_to
한 디렉토리에 파일들을 너무 많이 몰아둘 경우, OS 파일찾기 성능 저하 디렉토리 Depth가 깊어지는것은 성능에 큰 영향이 없음

```python
# models.py

photo = ImageField(upload_to('blog/post/%Y/%m/%d'))
# 시 / 분 / 초 까지도 되지만 너무 세분화 할 필요는 없기 때문에 년/월/일 까지만 해줘도 무방하다.
```

#### 템플릿 내에서 각 media 파일 URL 처리
* FileField/ImageField의 .url 속성
<br>
![popop](https://user-images.githubusercontent.com/46446165/59565614-cec1ab80-9090-11e9-9040-625ed60e2b57.png)
<br>
이런식으로 템플릿에 적용 시켜줄 수 있다.
