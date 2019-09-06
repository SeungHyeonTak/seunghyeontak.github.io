---
layout: post
title:  "(etc). Heroku 배포하기"
comments: true
description: "Django, Heroku"
author: SeungHyeon Tak
date:   2019-09-07 02:03:20 +0700
categories: [etc]
tags: [etc]
keywords: "Django, Heroku"
---
#### etc. Heroku 배포하기

#### Hroku로 배포한 사이트
* [배포사이트](http://seunghyeonproject.herokuapp.com/)

#### Heroku 설치
* ubuntu: $ sudo snap install -classic heroku
* macOS: $ brew tap heroku/brew && brew instal heroku

#### 모듈 설치
* pip install

```text
Django - pycharm에서 설치
-> $ pip install dj-database-url (데이터베이스 관련 옵션을 변수로 쉽게 접근하게 해준다.)
-> $ pip install gunicorn (wsgi용 미들웨어->웹 서버와 장고 사이의 다리 역할)
-> $ pip install whitenoise (static file 서빙용 미들웨어)
-> $ pip install psycopg2-binary (postgreSQL용 드라이버)
```

#### 의존성 패키지 목록 작성

```text
$ pip freeze > requirements.txt
```

#### 설정

* 전체폴더 / project_name / settings.py
```python
import dj_database_url
DEBUG = False
ALLOWED_HOSTS = ['*']
MIDDLEWARE = [
	...
	'whitenoise.middleware.WhiteNoiseMiddleware',
]
DATABASE['default'].update(dj_database_url.config(conn_max_age=500))
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
```

#### Procfile 텍스트 파일 생성 및 코드 작성 / runtime.txt 파일 생성 및 코드 작성

```text
- Procfile 생성
위치 : ./Procfile

web : gunicorn config.wsgi

위의 내용 입력 후 저장

- runtime.txt 생성
위치 : ./runtime.txt

python-3.7.0

위의 내용 입력 후 저장
```

#### Heroku Login

* heroku login

#### git 연동
* 제일 상위 폴더에 생성
* .gitignore 생성

```text
*.pyc
*~
/venv
__pycache__
db.sqlite3
/static
.DS_Store
```

```text
* git 연동방법
$ git init
$ git add -A .
$ git commit -m "Heroku start"
$ heroku create [app_name] (heroku에 app 생성 하는 동작)
$ git push heroku master (heroku에 업로드)
```

#### DB 초기화 / 관리자 계정 생성

```text
$ heroku run python manage.py migrate
$ heroku run python manage.py createsuperuser
```

#### heroku를 통한 사이트 확인

```text
$ heroku open
```

*****

* heroku는 git과 연동이 안되면 사이트가 동작 하지 않는다. 개인적인 바램으로는 Heroku와 git(개인 git 사이트를 같이 관리하면 좋겠다라고 생각한다.) - 관리자는 git을 Heroku에만 push 해서 마지막 부분이 빠져있다.
