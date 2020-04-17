---
layout: post
title:  "Algorithm. Level 1 정수 내림차순으로 배치하기"
comments: true
description: "Algorithm"
author: SeungHyeon Tak
date:   2020-04-17 21:39:42 +0700
categories: [Algorithm]
tags: [Algorithm]
keywords: "Algorithm"
---
#### Algorithm Level 1 정수 내림차순으로 배치하기
##### 프로그래머스 문제

#### 문제
함수 solution은 정수 n을 매개변수로 입력받습니다. 

n의 각 자릿수를 큰것부터 작은 순으로 정렬한 새로운 정수를 리턴해주세요. 

예를들어 n이 118372면 873211을 리턴하면 됩니다.

```python
def solution(n):

    nlist = sorted(list(str(n)), reverse=True)

    answer = [int(i) for i in nlist]

    ans = "".join(map(str, answer))

    return int(ans)


if __name__ == "__main__":
    n = 118372
    print(solution(n))
```
