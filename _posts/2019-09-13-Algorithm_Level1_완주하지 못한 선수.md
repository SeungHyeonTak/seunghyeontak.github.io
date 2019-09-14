---
layout: post
title:  "Algorithm. Level 1 완주하지 못한 선수"
comments: true
description: "Algorithm"
author: SeungHyeon Tak
date:   2019-09-13 16:23:11 +0700
categories: [Algorithm]
tags: [Algorithm]
keywords: "Algorithm"
---
#### Algorithm Level 1 완주하지 못한 선수
##### 프로그래머스 문제

#### 문제
> 수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.
> 마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 
> 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.
> hash 함수를 이용한 문제


```python
def solution(participant, completion):
    answer = ''
    temp = 0
    dic = {}
    for part in participant:
        dic[hash(part)] = part
        temp += int(hash(part))
    for com in completion:
        temp -= hash(com)
    answer = dic[temp]

    return answer


if __name__ == "__main__":
    participant = ["leo", "kiki", "eden"]  # 참가자 명단
    completion = ["eden", "kiki"]  # 완주자 명단
    print(solution(participant, completion))

```
