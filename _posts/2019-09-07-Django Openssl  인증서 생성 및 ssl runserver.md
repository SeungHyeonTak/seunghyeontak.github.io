---
layout: post
title:  "Django Openssl 인증서 생성 및 ssl runserver"
comments: true
description: "Django, Openssl, runsslserver"
author: SeungHyeon Tak
date:   2019-09-07 17:54:01 +0700
categories: [Django]
tags: [Django]
keywords: "Django, Openssl, runsslserver"
---
## Django Openssl

#### Openssl 인증서 생성 및 ssl runserver 사용하기

* openssl 버전 확인

```text
$ openssl version
(아무 반응이 없으면 설치해야함)
```

* 키 파일 생성

```text
$ openssl genrsa 1024 > django.key
```

* cert 파일 생성

```text
$ openssl req -new -x509 -nodes -sha256 -days 365 -key django.key > django.cert
```

* 해당 파일을 프로젝트 폴더에 붙여넣기
* django-sslserver 설치

```text
$ pip install django-sslserver
```

* settings.py INSTALLED_APPS 추가

```text
INSTALLED_APPS = [
    ...
    'sslserver',
]
```

* 명령어 실행

```
$ python manage.py runsslserver --certificate django.cert --key django.key

$ python manage.py runsslserver
```
