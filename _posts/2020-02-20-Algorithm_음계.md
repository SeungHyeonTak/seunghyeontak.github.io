---
layout: post
title:  "Algorithm. 음계 문제 난이도 (하)"
comments: true
description: "Algorithm"
author: SeungHyeon Tak
date:   2020-02-20 16:21:00 +0700
categories: [Algorithm]
tags: [Algorithm]
keywords: "Algorithm"
---
## Algorithm

### 백준 알고리즘 2920 음계 문제

**난이도 (하)**

**문제)**

다장조는 c d e f g a b C, 총 8개 음으로 이루어져있다. 

이 문제에서 8개 음은 다음과 같이 숫자로 바꾸어 표현한다. 

c는 1로, d는 2로, ..., C를 8로 바꾼다.

1부터 8까지 차례대로 연주한다면 ascending, 

8부터 1까지 차례대로 연주한다면 descending, 둘 다 아니라면 mixed 이다.

연주한 순서가 주어졌을 때, 이것이 ascending인지, descending인지, 아니면 

mixed인지 판별하는 프로그램을 작성하시오.

*****

**입력)**

첫째 줄에 8개 숫자가 주어진다. 

이 숫자는 문제 설명에서 설명한 음이며, 

1부터 8까지 숫자가 한 번씩 등장한다.

*****

**출력)**

첫째 줄에 ascending, descending, mixed 중 하나를 출력한다.

<details>
<summary>정답 코드</summary>
<div markdown="1">

```python
a = list(map(int, input().split(' ')))

# 오름차순 ascending
# 내림차순 descending
# 섞여있을때 mixed
ascending = True
descending = True

for i in range(1, 8):
    if a[i] > a[i-1]:
        descending = False
    elif a[i] < a[i-1]:
        ascending = False

if ascending:
    print('ascending')
elif descending:
    print('descending')
else:
    print('mixed')
```

</div>
</details>
