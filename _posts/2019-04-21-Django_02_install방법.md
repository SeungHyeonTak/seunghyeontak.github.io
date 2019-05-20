---
layout: post
title:  "Django_02.(로또만들기)install"
comments: true
description: "Django_part02"
author: SeungHyeon Tak
date:   2019-04-21 22:06:23 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.02 웹 개발

* Django는 왜 사용하나?
   * https://www.djangoproject.com
   * 쉽고?(쉬운건 잘모르겠습니다. ㅜㅜ) 강력하고 안전하고 확장 가능하다.

※앞서 들어가기 전 이 과정은 전부 리눅스(ubuntu)-pycharm으로 진행되오니 참고 바랍니다.

* pycharm 설치하기
   * https://www.jetbrains.com/pycharm/download/#section=linux 에서 pycharm 설치
     둘중에 Community 다운 (이유는 공짜라서)

###### ubuntu python 가상환경(virtualenv) & django 설치 방법 

```
# pycharm에서 프로젝트 만들고 pycharm 터미널을 이용하기 바람!
# 먼저 pip설치
$ sudo apt-get install python3-pip python3-dev

# pip 설치가 완료 되었는지 확인
$ pip3 -v

# pip3을 사용하여 virtualenv를 설치
$ pip3 install virtualenv virtualenvwrapper
※pip3 명령이 먹지 않는다면 아래 명령어로 대체할 수 있다.
(sudo pip3 install로 실행 해주자)
-시스템 root 권한으로 pip를 설치한 겨웅 sudo권한이 필요할 수 있어서-

# 가상환경 만들기
$ virtualenv --python=파이썬버전 가상환경이름
ex) $ ...=python3.6 venv
(가상환경 이름은 어떠한 이름을 사용가능하니 (참고))

# 가상환경 활성화
$ source 가상환경이름/bin/activate


---(부록)---
# 가상환경 비활성화
$ deactivate

# 가상환경 지우기
$ rmvirtualenv 가상환경이름
-----------

# Django install 방법

# 설치
$ pip install Django

# 프로젝트 만들기
$ django-admin startproject config .

# DB 초기화
$ python3 manage.py migrate

# 관리자 계정 생성(슈퍼유저 생성)
$ python3 manage.py createsuperuser

# 기본 앱 만들기
$ python3 manage.py stratapp app_name
# app_name은 사용자가 원하는 이름으로 변경

# 테스트 서버 구동
$ python3 manage.py runserver
(자신이 localhost와 port를 지정하고 싶다면 
python3 manage.py runserver 0.0.0.0:8080)
이런식으로 변경 가능함
```
