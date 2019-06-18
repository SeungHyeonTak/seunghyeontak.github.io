---
layout: post
title:  "(etc). django_migrate DB 날리기"
comments: true
description: "django"
author: SeungHyeon Tak
date:   2019-06-19 23:25:00 +0700
categories: [etc]
tags: [etc]
keywords: "migrate DB"
---
#### django. 기본 웹 개발(etc)

가끔 django 프로젝트를 진행하다보면 migrate를 잘못해서 안되는 경우가 많다.
이럴때를 대비해서 각 app/migrate 디렉토리에 파일을 제거하고 
python manage.py reset_db 를 해준다음 다시 makemigrations - migrate를 해준다면 정상적으로 작동 할것이다.