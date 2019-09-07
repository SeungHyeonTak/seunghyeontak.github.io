---
layout: post
title:  "Django_기본. allauth 사용"
comments: true
description: "Django, Openssl, runsslserver"
author: SeungHyeon Tak
date:   2019-09-07 17:54:01 +0700
categories: [Django]
tags: [Django]
keywords: "Django, Openssl, runsslserver"
---
#### Django.기본 웹 개발

#### Openssl 인증서 생성 및 ssl runserver 사용하기 (social login 연동)

* facebook / naver / google 등 소셜 로그인을 하기위해 사용되는 기술

* django-allauth install

```
$ pip install django-allauth
```

* settings.py에서 INSTALLED_APPS 추가

```
INSTALLED_APPS = [
    ...
    'django.contrib.sites',
    'allauth',
    'allauth.account',
    'allauth.socialaccount',
]
```

* settings.py에 AUTHENTICATION_BACKENDS 추가

```
AUTHENTICATION_BACKENDS = [
    'django.contrib.auth.backends.ModelBackend',
    'allauth.account.auth_backends.AuthenticationBackend',
]
```

* 사용하려는 소셜 로그인 프로바이더 추가

```
INSTALLED_APPS = [
    ...
    'allauth.socialaccount.providers.facebook',
    'allauth.socialaccount.providers.naver',
    ...
]
```

* settings.py 에 SITE_ID 추가

```
SITE_ID = 1
```

* urls.py에 allauth 관련 라우팅 추가

```
urlpattterns = [
    ...
    path('accounts/', include('allauth.urls')),
]
```

* migrate 명령 실행

```
$ python manage.py migrate
```

* 이번 포스팅은 페이스북만 알아볼 예정이다.
* 페이스북 개발자 사이트로 이동
  * https://developers.facebook.com/?locale=ko_KR
  * 로그인 후 `새 앱 추가` 클릭
* 앱 이름과 이메일 입력 후 `앱 ID 만들기` 클릭
* 앱 기능 중, Facebook 로그인 통합에 체크 및 `확인` 클릭
* 화면 하단 `내 제품` 목록에서 Facebook 로그인의 `설정` 클릭
* 유효한 Oauth 리디렉션 URL에 서버 주소 입력 및 `변경 내용 저장` 클릭
  * https://127.0.0.1:8000
  * https://127.0.0.1:8000/accounts/facebook/login/callback/
* 앱 기본 설정 페이지로 이동하여 앱 ID와 시크릿 코드 확인
* 관리자 페이지 Social application에 새로운 설정 추가
  * Provider : Facebook
  * 이름 입력
  * Client ID / Secret Key : 페이스북 앱 화면에서 복사 붙여넣기
  * site: `Available sites`의 example.com을 우측 `Chosen site`로 이동
* 로그인 페이지에 접속해 Facebook 버튼 클릭
  * ./accounts/login 페이지 이동 Facebook 로그인 후
    * ./accounts/profile 페이지로 이동될것이다. settings.py에서 코드 한줄 추가하면 문제 해결
    * LOGIN_REDIRECT_URL = '/'
* allauth를 사용하면 따로 accounts 앱을 만들어줄 필요 없다.
*****
* allauth를 사용할때 tip
  * ./accounts/register 를 입력하면 accounts에서 사용할 수 있는 url을 볼 수 있다.
  * 기본 UI는 이쁘지 않기 때문에 커스텀해줄 수 있는데 관련 내용은 밑의 url을 참고 하기 바람
  * [Youtube](https://www.youtube.com/watch?v=dXZim_jgaiI)
  * github에서 allauth 소스코드 참고 https://github.com/pennersr/django-allauth

* 다른 분 Blog 참고
  * [Django Allauth Login Page Custom](https://ngee.tistory.com/2161)
  * [Django Allauth Profile 적용](https://ngee.tistory.com/2160)
