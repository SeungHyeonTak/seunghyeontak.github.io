---
layout: post
title:  "Algorithm. Level 1 자연수 뒤집어 배열로 만들기"
comments: true
description: "Algorithm"
author: SeungHyeon Tak
date:   2020-04-17 21:14:42 +0700
categories: [Algorithm]
tags: [Algorithm]
keywords: "Algorithm"
---
#### Algorithm Level 1 자연수 뒤집어 배열로 만들기
##### 프로그래머스 문제

#### 문제
자연수 n을 뒤집어 각 자리 숫자를 원소로 가지는 배열 형태로 리턴해주세요. 

예를들어 n이 12345이면 [5,4,3,2,1]을 리턴합니다.

```python
def solution(n):
    answer = []

    n = str(n)

    for i in n[::-1]:
        i_n = int(i)
        answer.append(i_n)

    return answer


if __name__ == "__main__":
    n = 12345
    print(solution(n))  # [5,4,3,2,1]

```
