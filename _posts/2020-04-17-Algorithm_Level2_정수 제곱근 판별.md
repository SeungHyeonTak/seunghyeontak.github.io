---
layout: post
title:  "Algorithm. Level 1 정수 제곱근 판별"
comments: true
description: "Algorithm"
author: SeungHyeon Tak
date:   2020-04-27 18:00:42 +0700
categories: [Algorithm]
tags: [Algorithm]
keywords: "Algorithm"
---
#### Algorithm Level 1 정수 제곱근 판별
##### 프로그래머스 문제

#### 문제
임의의 양의 정수 n에 대해, n이 어떤 양의 정수 x의 제곱인지 아닌지 판단하려 합니다.

n이 양의 정수 x의 제곱이라면 x+1의 제곱을 리턴하고, 

n이 양의 정수 x의 제곱이 아니라면 -1을 리턴하는 함수를 완성하세요.

```python
import math


def solution(n):
    answer = 0
    num = int(math.sqrt(n))
    if num > 0 and num*num == n:
        i = num + 1
        answer = i*i
    else:
        return -1

    return answer


if __name__ == "__main__":
    n = 121
    print(solution(n))
```
