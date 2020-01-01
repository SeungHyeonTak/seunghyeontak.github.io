---
layout: post
title:  "Django Shell 다루기"
comments: true
description: "Django Shell"
author: SeungHyeon Tak
date:   2019-04-25 22:10:00 +0700
categories: [Django]
tags: [Django]
keywords: "Django Shell"
---
## Django Shell 사용하기

* Pycharm으로 shell 이용하기
* 터미널

```
$ python manage.py shell
>>> from mysite.models import Numbers
>>> from django.utils import timezone
>>> Numbers.objects.all()
# 이렇게 입력하면 모델 클래스의 오브젝트를 리스트로 모두 가져올 수 있다.

# 변경 사항 후 DB에 저장할 수 있다.
>>> Numbers.objects.get(name = 'abcd')
>>> g = Numbers.objects.get(name = 'abcd')
>>> g.name='linux'
# 이렇게 하면 abcd로 저장되있던 name이 linux로 변경된 사실을 g로 출력해보면 알 수 있다.
>>> g.save()
# 마지막으로 save를 해야지 저장이 된다.
```
