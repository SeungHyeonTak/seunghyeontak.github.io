---
layout: post
title:  "Django test code 작성 해보기"
comments: true
description: "Django test code"
author: SeungHyeon Tak
date:   2019-04-24 13:30:31 +0700
categories: [Django]
tags: [Django]
keywords: "Django test code 작성해보기", "Django 테스트 코드 작성해보기"
---
#### Django test code

먼저 우리가 생성한 앱폴더로 가보면 test.py가 있다.
test 코드는 test.py에서 작성을 하는데 우리가 models.py에서 만든 Numbers의 generate 메소드를 테스트하기 위해 쓰이는 방법이다.

```
from django.test import TestCase #TestCase를 상속 받기 위해 쓴다.
from .models import Numbers

#test code 작성
class GuessNumbersTestCase(TestCase):
    def test_generate(self):
        g = GuessNumbers(name = 'apple', text = 'pineappne')
        g.generate()
        print(g.update_data) # 현재시간으로 업뎃
        print(g.lottos) # Lotto의 내용
        self.assertTrue(len(g.lottos) > 20) # 글자수가 20글자수보다 크면 통과한다.
        # 성공인지 실패인지 확인할때 assertTrue로 확인
```

* 실행방법

```
$ python manage.py test
```
