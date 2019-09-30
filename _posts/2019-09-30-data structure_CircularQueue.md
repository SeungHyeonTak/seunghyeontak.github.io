---
layout: post
title:  "자료구조. CircularQueue"
comments: true
description: "Queue"
author: SeungHyeon Tak
date:   2019-09-30 16:58:00 +0700
categories: [자료구조]
tags: [자료구조]
keywords: "자료구조"
---
#### 자료구조. Circular Queue


#### Circular Queue란?
> Queue 자료구조의 특성상 한쪽 방향에서 데이터가 들어가고 한쪽방향에서는 데이터가 나가면서 뒤에 저장된 데이터들을 한 칸씩 모두 이동해야 하는 한계가 있습니다.
> Circular Queue는 번거러운 데이터 이동을 발생시키지 않고 주어진 공간을 활용할 수 있다는 장점이 있습니다.
> 처음 생성한 리스트의 크기를 기준으로 환형이 일어나기 때문에 enqueue가 많이 발생하지 않아 쓰지 않는 공간에 대한 낭비가 발생할 수도 있으며, 큐로 활용되는 리스트의 크기를 늘려야 할 경우 큐의 원소의 순서를 유지하면서 크기를 늘려야 하기때문에 크기 조정이 어려움


#### Circular Queue 구현하기

```python
class CircularQueue:

    def __init__(self, n):
        self.maxCount = n
        self.data = [None] * n
        self.count = 0
        self.front = -1
        self.rear = -1

    def size(self):
        return self.count

    def isEmpty(self):
        return self.count == 0

    def isFull(self):  # 큐가 꽉차있는가?
        return self.count == self.maxCount

    def enqueue(self, x):
        if self.isFull():
            raise IndexError
        self.rear = (self.rear + 1) % self.maxCount
        self.data[self.rear] = x
        self.count += 1

    def dequeue(self):
        if self.isEmpty():
            raise IndexError('Queue empty')
        self.front = (self.front + 1) % self.maxCount
        x = self.data[self.front]
        self.count -= 1
        return x

    def peek(self):
        if self.isEmpty():
            raise IndexError('Queue empty')
        return self.data[(self.front + 1) % self.maxCount]


if __name__ == "__main__":
    Q = CircularQueue(8)

    Q.enqueue(1)
    Q.enqueue(2)
    Q.enqueue(3)
    Q.enqueue(4)

    print(Q.data)

    Q.dequeue()

    print(Q.peek())

    Q.enqueue(5)
    Q.enqueue(6)
    Q.enqueue(7)
    Q.enqueue(8)

    print(Q.data)

    Q.enqueue(9)
    print(Q.data)
```