---
layout: post
title:  "Django_기본.static file 관리하기"
comments: true
description: "Django"
author: SeungHyeon Tak
date:   2019-06-16 22:46:21 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.기본 웹 개발

#### Static Files - css / javascript 파일 관리

* Static Files : 개발 리소스의 정적인 파일(js, css, image, etc)
  * 앱 단위로 저장/서빙
  * 프로젝트 단위로 저장/서빙

장고는 하나의 프로젝트에 멀티앱의 구조를 가지고 있다.
각 앱에서 쓰이는 정적인 파일들은 각 앱에서 관리하는게 좋다.
그런데 각 앱에서 쓰이는 정적인 파일이 아니라 전체에서 쓰이는 정적인 파일이 있다면
프로젝트 하위 디렉토리에 폴더를 하나를 만들어서 관리를 하는것이다.

#### 관련 settings 예시

* STATIC_URL = '/static/'

* STATICFILES_DIRS = [
	os.path.join(BASE_DIR, 'static') # 상위 디렉토리에서 관리
]
  * 프로젝트 단위로 서빙을 할때 쓰임(리스트 형식으로 여러 경로에 지정 할 수 있지만 하나만 설정 해줘도 문제 없다)

* STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
  * 개발 당시에는 의미가 없지만, 실서비스 배포 전에 static 파일들을 모아서 배포 서버에 복사
  * 각 디렉토리에 나눠져 있는 static파일들을 collectstatic 명령을 통해 지정한 경로로 복사
<br>

#### 템플릿 내에서 static 파일 url처리

* 방법1) 수동으로 settings.STATIC_URL prefix 붙이기 (비추)
※ settings.STATIC_URL 설정은 언제라도 변경될 수 있기때문에
<br>
![1](https://user-images.githubusercontent.com/46446165/59564534-f90c6c80-9082-11e9-9bf8-7d9208ff834f.png)

* 방법2) Template Tag를 통해 Prefix 붙이기(추천)
<br>
![2](https://user-images.githubusercontent.com/46446165/59564539-075a8880-9083-11e9-8124-2969b9202102.png)

#### 개발 환경에서의 static 파일 서빙
* 개발서버를 통해 settings.DEBUG = True 설정에 한해 서빙 지원
  * settings.py안에 DEBUG = True를 확인
* 프로젝트/urls.py에 Rule이 명시되어 있지 않아도 자동 Rule 추가
* 장고를 통해 직접 static파일 서빙하는 것은 개발 목적으로만 제공
<br>
#### 실 서비스 배포전 collectstatic
* STATIC_ROOT를 추가해야한다.
* 실 서비스 정적파일 서빙은 외부 웹서버 권장(nginx/apache/S3/CDN)
* 여러 디렉토리에 나눠 저장된 static 파일들의 위치는 장고 프로젝트만 알고 있지, 외부 웹서버는 알지 못한다.

ex)
blog/static/blog/style.css => settings.STATIC_ROOT / blog/style.css
static/style.css => settings.STATIC_ROOT / style.css

이렇게 한곳에(staticfiles) 모아준다.
<br>
#### 외부 웹서버에 의한 static/media 컨텐츠 서비스
* 정적인 컨텐츠는 외부 웹서버에서 직접 답을하면, 더 빠른 응답

<br>
#### static 파일 모으는 순서, 적용순서 (배포)
1. python manage.py collectstatic 명령을 통해 settings.STATIC_ROOT 경로 아래로 static 파일들을 모두 복사
2. settings.STATIC_ROOT 경로에 모아진 파일을 배포서버로 복사
3. settings.STATIC_URL 설정이 배포서버를 가르키도록 수정
4. 실서비스 static 서빙 웹서버 설정 (nginx/apache/S3/CDN 등)
