---
layout: post
title:  "Algorithm study 버블정렬"
comments: true
description: "버블정렬"
author: SeungHyeon Tak
date:   2020-04-29 15:46:00 +0700
categories: [Algorithm]
tags: [Algorithm]
keywords: "버블정렬"
---
## Algorithm

### 버블정렬

정렬되어있지 않은 리스트를 앞에서 부터 2개씩 순서대로 정렬하는 기법을 버블 정렬이라고 한다.

수를 계속 비교하여 정렬이 될때까지 반복 현재 index와 그 뒤의 index를 비교함

시간복잡도가 O(n^2)이므로 상당히 느리다.

```python
# 버블정렬

def bubblesort(arr):
    for i in range(len(arr)-1):
	swap = False
        for j in range(len(arr)-i-1):
            if arr[j] > arr[j+1]:
		arr[j], arr[j+1] = arr[j+1], arr[j]
		swap = True
	if swap == False:
	    break

if __name__ == __main__:
    import random

    arr = random.sample(range(100),50)
    bubblesort(arr)
    print(arr)
```
