---
layout: post
title:  "Algorithm. Level 1 같은 숫자는 싫어"
comments: true
description: "Algorithm"
author: SeungHyeon Tak
date:   2019-09-30 17:45:00 +0700
categories: [Algorithm]
tags: [Algorithm]
keywords: "Algorithm"
---
#### Algorithm Level 1 같은 숫자는 싫어
##### 프로그래머스 문제

#### 문제
> 배열이 주어지면 배열의 각 원소의 수 중에 연속적으로 나타나는 숫자는 하나만 남기고 전부 제거하려고 한다. 단, 제거 된 후 남은 수들을 반환할 때는 배열 arr의 원소들의 순서를 유지해야 합니다.
> ex) arr[1,1,1,2,3,4,4]  -> [1,2,3,4]

```python
def solution(arr):
    answer = []
    for i in range(len(arr)):
        if i == 0:
            answer.append(arr[i])
        elif arr[i] != arr[i - 1]:
            answer.append(arr[i])
    return answer


if __name__ == "__main__":
    arr = [4, 4, 4, 3, 3]
    print(solution(arr))
```
