---
layout: post
title:  "Django pythonanywhere"
comments: true
description: "Django pythonanywhere"
author: SeungHyeon Tak
date:   2019-04-29 23:21:10 +0700
categories: [Django]
tags: [Django]
keywords: "Django", "Pythonanywhere"
---
#### Django pyothonanywhere

* pythonanywhere 사용법

시작하기 전에 우리가 사용한 python project안에

`genkey.py`를 넣어두자

```
import string
import random

chars = ''.join([string.ascii_letters, string.digits, string.punctuation]).replace('\'','').replace('"','').replace('\\','')

SECRET_KEY = ''.join([random.SystemRandom().choice(chars) for i in range(50)])

print(SECRET_KEY)
```

* github 계정을 만든다음 새로운 repositories를 생성해준다.
  (※ 올릴파일 하나만 있어야 함 그래서 다른 폴더에 있더라도 새로운 repositories를 생성 해줘야 하는 이유이다.)
  git에 만든 파일 upload 하기

* 그리고 <https://www.pythonanywhere.com> 에서 회원가입 후 로그인하기

* Web으로 들어가서 Add a new web app - Manual configuration - 자신의 버전에 맞는 python 선택

* Consoles - Start a new console의 bash로 들어간다.

```
$ git clone "git의 repositories 주소를 가져온다."

# virtualenv 설치
$ virtualenv --python=python3.6 venv
#venv로 접속
$ source venv/bin/activate 

$ pip install django2.1 whitenoise
# whitenoise? - 배포할때 static 파일들을 쉽게 관리 해주는 것들이다.

$ pip freeze
# 설치 확인

# 이제 manage.py가 있는 폴더 안으로 들어간다.
$ python manage.py collectstatic
>> yes #yes를 해준다.

$ python manage.py migrate
$ python manage.py createsuperuser
```

* bash가 띄워진 웹은 놔두고 새창을 하나 열어 다시 pythonanywhere를 띄운다.

* Web으로 들어가서 WSGI와 virtualenv의 경로를 입력해준다.

```
>>> virtualenv 부분에 경로 입력
/home/'git_id'/'virtualevn_name' 을 입력해주고 버튼을 누르고 아무 이상 없으면 잘된거
이상이 있으면 경로를 다시 확인 하면 된다.

>>> WSGI - web서버와 django를 연결 시켜주는것
들어가면 많은 내용이 있는데 그 내용을 다지우고 밑의 내용을 복붙한다.
```

```
import os
import sys

path = '/home/<your-username>/<lotto-web>'  # 여러분의 유저네임과 폴더 경로를 여기에 적어주세요.
if path not in sys.path:
    sys.path.append(path)

os.environ['DJANGO_SETTINGS_MODULE'] = 'mysite.settings' # 이 부분에 <project>.settings를 적어주시면 됩니다.

from django.core.wsgi import get_wsgi_application
from whitenoise.django import DjangoWhiteNoise
application = DjangoWhiteNoise(get_wsgi_application())
```

이후 web으로 다시 들어가서 Reload를 해준다.
그러면 그 위에 도메인이 뜰건데 그 주소로 들어가서 잘 되었는지 확인 가능하다
아직 설정이 덜 되서 안될것이다.

다시 bash로 들어가서 genkey.py라는 파일을 찾고 vim으로 접속을 해볼것이다.

```
$ python genkey.py #를 입력하면 알수없는 문자열이 뜰건데 그것들을 복사 해놓고
$ nano config/settings.py #로 접속을 한다.
```

그리고 계속 내려가다보면

```
SECRET_KEY = '아까 복사한 내용을 붙여 넣기'
DEBUG = False # True를 False로 변경해주기
ALLOWED_HOSTS = ['나의 도메인 사이트 입력']
```

이후 빠져나오려면
ctrl+x - y - enter 으로 빠져나온다.

그리고 다시 Reload를 돌리고 도메인에 접속 해본다.
