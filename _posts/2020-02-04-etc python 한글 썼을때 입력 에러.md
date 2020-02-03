---
layout: post
title:  "(etc). python 한글 입력후 에러 메세지"
comments: true
description: "Non-ASCII character"
author: SeungHyeon Tak
date:   2020-02-04 00:47:00 +0700
categories: [etc]
tags: [etc]
keywords: "Non-ASCII character"
---
## etc. python 한글 입력 에러

**python에서 주석이든 코드든 한글을 입력했을 경우 `SyntaxError: Non-ASCIIcharacter`에러 발생**

아래와 같은 에러를 확인 할 수 있다.

```
SyntaxError: Non-ASCII character '\xeb' in file test.py on line 33, but no encoding declared; see http://python.org/dev/peps/pep-0263/ for details
```

이럴땐 간단히 해결하는 방안으로 파일 상단에 코드 두줄을 추가해주면 해결 가능하다.

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
```


