---
layout: post
title:  "Django MTV"
comments: true
description: "Django MTV"
author: SeungHyeon Tak
date:   2019-04-26 19:13:59 +0700
categories: [Django]
tags: [Django]
keywords: "Django MTV"
---
## Django MTV란?

M - Model
T - Templates
V - View

* `urls.py`에서 빈 url을 하나 만들어 index method 와 연결한다.

```
urlpatterns = [
	path('',views.index, name = 'index'),
]
```

* 'views.py`에서 models의 Numbers의 전체를 불러온다.

```
from .models import Numbers

def index(request):
	lottos = Numbers.object.all() # Numbers의 전체를 읽어온다.
	...
```

* `template.py` 파일에서 반복문 추가하기
<https://github.com/SeungHyeonTak/HTML-study/blob/master/default2.html>
