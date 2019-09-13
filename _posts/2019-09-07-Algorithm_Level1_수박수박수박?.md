---
layout: post
title:  "Algorithm. Level 1 수박수박수박?"
comments: true
description: "Algorithm"
author: SeungHyeon Tak
date:   2019-09-09 01:20:20 +0700
categories: [Algorithm]
tags: [Algorithm]
keywords: "Algorithm"
---
#### Algorithm Level 1 수박수박수박?
##### 프로그래머스 문제

#### 문제
> 길이가 n이고, 수박수박수박수....와 같은 패턴을 유지하는 문자열을 리턴하는 함수, solution을 완성하세요. 
> 예를들어 n이 4이면 수박수박을 리턴하고 3이라면 수박수를 리턴하면 됩니다.

```python
def solution(n):
    return ('수박'*n)[:n]


if __name__ == "__main__":
    n = 1
    print(solution(n))

```
