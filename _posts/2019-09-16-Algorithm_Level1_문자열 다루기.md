---
layout: post
title:  "Algorithm. Level 1 문자열 다루기"
comments: true
description: "Algorithm"
author: SeungHyeon Tak
date:   2019-09-16 16:47:42 +0700
categories: [Algorithm]
tags: [Algorithm]
keywords: "Algorithm"
---
#### Algorithm Level 1 문자열 다루기
##### 프로그래머스 문제

#### 문제
> 문자열 s의 길이가 4 혹은 6이고, 숫자로만 구성돼있는지 확인해주는 함수, solution을 완성하세요. 
> 예를 들어 s가 a234이면 False를 리턴하고 1234라면 True를 리턴하면 됩니다.

```python
def solution(s):
    if len(s) == 4 or len(s) == 6 :
        try :
            int(s)
            return True
        except ValueError :
            return False
    else :
        return False
```
