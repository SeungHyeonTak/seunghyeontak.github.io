---
layout: post
title:  "python split, join"
comments: true
description: "Python split, join"
author: SeungHyeon Tak
date:   2020-02-12 17:42:00 +0700
categories: [python]
tags: [python]
keywords: "python split, join"
---
## python split / join

### 문자열 자르기

간단하다 따로 설명 없이 예제로 보면서 이해하자

```python
# 문지열을 - 기호를 기준으로 자르는 경우

site = 'web-is-free'
site.split('-')

# ['web', 'is', 'free']
```

이렇게 자르면 배열 형식이 되는데 가져다 쓰고 싶은 부분을 가져다 사용하면 된다.

혹은 `join()`으로 문자열을 병합 할 수 도 있다.

### 문자열 병합

```python
site = [ "web", "is", "free" ]
"-".join(site)
# 앞에는 연결할 문자열을 써주면 된다.

# web-is-free
```