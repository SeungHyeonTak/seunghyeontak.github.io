---
layout: post
title:  "Django id값으로 detail 만들기"
comments: true
description: "Django id"
author: SeungHyeon Tak
date:   2019-04-28 20:20:13 +0700
categories: [Django]
tags: [Django]
keywords: "Django detail"
---
#### Django detail 제작

* `urls.py` detail 내용 추가
* 정규표현식 사용 / url 부분에 입력된 해당 숫자를 mysitekey 변수에 argument parmeter로 담는다.
```
path('mysite/?P<mysitekey>([0-9]+)/detail/',views.detail, name = 'mysite_detail'),
```

* `views.py` 내용 추가

```
def detail(request, mysitekey):
    lotto = GuessNumbers.objects.get(pk = mysitekey)
    return render(request, "mysite/detail.html", {'lotto':lotto})
```

* `detail.html` 생성 및 내용 작성
<https://github.com/SeungHyeonTak/HTML-study/blob/master/detail.html>

