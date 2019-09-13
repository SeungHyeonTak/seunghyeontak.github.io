---
layout: post
title:  "Algorithm. Level 1 두 정수의 합"
comments: true
description: "Algorithm"
author: SeungHyeon Tak
date:   2019-09-09 01:17:20 +0700
categories: [Algorithm]
tags: [Algorithm]
keywords: "Algorithm"
---
#### Algorithm Level 1 두정수의 합
##### 프로그래머스 문제

#### 문제
> 두 정수 a, b가 주어졌을 때 a와 b 사이에 속한 모든 정수의 합을 리턴하는 함수, solution을 완성하세요.
> 예를 들어 a = 3, b = 5인 경우, 3 + 4 + 5 = 12이므로 12를 리턴합니다.

```python
def solution(a, b):
    answer = 0
    if a < b:
        for i in range(a, b + 1):
            answer += i
    elif a > b:
        for i in range(b, a+1):
            answer += i
    else:
        answer = a
    return answer


if __name__ == "__main__":
    a = 3
    b = 3
    print(solution(a, b))

```
