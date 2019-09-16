---
layout: post
title:  "Algorithm. Level 1 예산"
comments: true
description: "Algorithm"
author: SeungHyeon Tak
date:   2019-09-16 18:10:42 +0700
categories: [Algorithm]
tags: [Algorithm]
keywords: "Algorithm"
---
#### Algorithm Level 1 예산
##### 프로그래머스 문제

#### 문제
> 본 문제는 지문을 처음 지문을 보면 조합이라는 개념으로 접근을 하여 금액 부분을 빼서 결과를 구하려고 하지만
> 단순하게 남는 금액에 상관없이 주어진 금액으로 최대지원가능한 부서의 갯수를 구하시오 라고 생각하면 쉽게 풀 수 있는 문제였다.

```python
# 예산
def solution(d, budget):
    answer = 0
    result = 0
    d.sort()
    for i in d:
        if result + i <= budget:
            result += i
            answer += 1
        else:
            break

    return answer


if __name__ == "__main__":
    d = [1, 3, 2, 5, 4]  # 부서별 신청 금액
    budget = 9  # 지원 예산 금액
    print(solution(d, budget))

```
