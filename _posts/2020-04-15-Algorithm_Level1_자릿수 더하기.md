---
layout: post
title:  "Algorithm. Level 1 자릿수 더하기"
comments: true
description: "Algorithm"
author: SeungHyeon Tak
date:   2020-04-15 03:57:42 +0700
categories: [Algorithm]
tags: [Algorithm]
keywords: "Algorithm"
---
#### Algorithm Level 1 자릿수 더하기
##### 프로그래머스 문제

#### 문제
자연수 N이 주어지면, N의 각 자릿수의 합을 구해서 return 하는 solution 함수를 만들어 주세요.

예를들어 N = 123이면 1 + 2 + 3 = 6을 return 하면 됩니다.

```python
n = 1234


def solution(n):
    """
    자릿수대로 할 수 있게 하기
    n에 있는 숫자를 list로 변환 시켜보기
    """
    answer = 0
    ilist = []

    char = list(str(n))

    for i in char:
        ilist.append(int(i))

    for i in range(len(ilist)):
        answer += ilist[i]

    return answer


if __name__ == "__main__":
    print(solution(n))

```
