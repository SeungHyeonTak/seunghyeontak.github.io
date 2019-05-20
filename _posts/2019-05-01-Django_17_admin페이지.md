---
layout: post
title:  "Django_17.(설문조사 만들기)admin 페이지 작성하기"
comments: true
description: "Django_part17"
author: SeungHyeon Tak
date:   2019-05-01 13:05:12 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.17 웹 개발

admin.py 에서 작성 하기 전에

```
$ python manage.py createsuperuser #이 부분으로 계정을 만들었는지 기억하고 있기
```

admin은 관리자 계정 페이지에서 우리가 models.py에서 입력한 데이터를 눈으로 보여주기 위한 페이지이다.

`admin.py`

```
from .models import Question, Choice

admin.site.register(Question)
admin.site.register(Choice)
```

위의 내용을 추가 하고 관리자 페이지로 접속 한다.

```
$ python manage.py runserver
```

<127.0.0.1:8000/admin> 으로 접속해서

![1](https://user-images.githubusercontent.com/46446165/57468231-0a619c80-72bf-11e9-9ce5-1733a58ddbaf.png)

이 부분이 나타나면 성공

그리고 Question으로 들어가면 우리가 shell에서 작성 한 부분들이 보일것이다.
그 다음 Choice로 들어가면 아무것도 없을건데 Choice추가로 생성해보자

![2](https://user-images.githubusercontent.com/46446165/57468251-12b9d780-72bf-11e9-96a5-e395f19d596c.png)

이 부분에서 Question을 누르면 우리가 입력했던것들이 보일것이다.
왜 여기서 저장되어있는지 궁금할법 하지만 우리는 models.py를 만들때 ForeignKey를 만든 기억이 있을것이다.
ForeignKey는 Question의 id값을 가져와 Choice에 뿌려주는 작업을 하기 때문에 저렇게 나타나는것이다.
이렇게 계속 추가하고 저장까지 시켜주면 정상적으로 동작되는걸 볼 수 있다.
