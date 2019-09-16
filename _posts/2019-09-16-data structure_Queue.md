---
layout: post
title:  "자료구조. Queue"
comments: true
description: "Queue"
author: SeungHyeon Tak
date:   2019-09-16 23:00:00 +0700
categories: [자료구조]
tags: [자료구조]
keywords: "자료구조"
---
#### 자료구조. Queue


#### Queue란?
> 자료를 보관할 수 있는 선형구조(스택과 동일)
> 넣을때에는 한쪽 끝에서 밀어 넣기 -> enqueue
> 꺼낼때에는 반대쪽에서 뽑아 꺼내야 하는 제약이 있음 -> dequeue
> 선입선출(FIFO) 특징을 가지는 선형 자료구조
<br>
![queue](https://user-images.githubusercontent.com/46446165/64964674-7a6bd900-d8d6-11e9-89d1-679459335280.png)
<br>
![queue2](https://user-images.githubusercontent.com/46446165/64964785-b4d57600-d8d6-11e9-80e5-3c20ff413c6e.png)
<br>
> 그림을 보면 먼저 들어간게 먼저 나오는 형식으로 된것이 Queue이다.

#### Queue구현의 2가지 방법
* 연산정의 (stack이랑 똑같지만 몇가지 알아둬야할 사실 * 참고)
  * size()
  * isEmpty()
  * enqueue(n)
  * dequeue() - * queue의 맨앞에 저장된 데이터원소 제거
  * peek() - * 맨앞에 저장된 데이터원소 반환

* ArrayQueue (배열을 이용한 방법)
  * python list와 메서드를 이용

```python
class ArrayQueue:
    def __init__(self):
        self.data = []  # 빈 queue 초기화

    def size(self):
        return len(self.data)

    def isEmpty(self):
        return self.size() == 0

    def enqueue(self, item):
        self.data.append(item)

    def dequeue(self):  # 데이터 원소 삭제 (Queue는 제일 처음 원소 삭제)
        return self.data.pop(0)

    def peek(self):
        return self.data[0]


if __name__ == "__main__":
    Q = ArrayQueue()
    Q.enqueue(11)
    Q.enqueue(22)
    Q.enqueue(33)
    Q.enqueue(44)
    print(Q.data)
    Q.dequeue()
    print(Q.data)
```

* DoubleLinkedList (양방향 연결리스트를 이용한 방법)
  * 양방향 연결리스트를 이용

```python
class Node:
    def __init__(self, item):
        self.data = item
        self.next = None
        self.prev = None


class DoubleLinkedList:
    def __init__(self):
        self.head = Node(None)
        self.tail = Node(None)
        self.head.prev = None
        self.head.next = self.tail
        self.tail.prev = self.head
        self.tail.next = None
        self.count = 0

    def getAt(self, pos):
        if pos < 0 or pos > self.count:
            return None

        i = 0
        curr = self.head
        while i < pos:
            curr = curr.next
            i += 1

        return curr

    def traverse(self):
        answer = []
        curr = self.head
        while curr.next.next:
            curr = curr.next
            answer.append(curr.data)

        return answer

    def reverse(self):
        answer = []
        curr = self.tail
        while curr.prev.prev:
            curr = curr.prev
            answer.append(curr.data)

        return answer

    def insertAt(self, pos, newnode):
        if pos < 0 or pos > self.count + 1:
            return False

        prev = self.getAt(pos - 1)
        return self.insertAfter(prev, newnode)

    def insertAfter(self, prev, newnode):
        next = prev.next
        prev.next = newnode
        newnode.next = next
        next.prev = newnode
        newnode.prev = prev
        self.count += 1
        return True

    def popAt(self, pos):
        if pos < 0 or pos > self.count:
            return None

        prev = self.getAt(pos - 1)
        return self.popAfter(prev)

    def popAfter(self, prev):
        curr = prev.next
        prev.next = curr.next
        curr.next.prev = prev
        self.count -= 1

        return curr.data


class LinkedListQueue:
    def __init__(self):
        self.data = DoubleLinkedList()

    def size(self):
        return self.data.count

    def isEmpty(self):
        return self.size() == 0

    def enqueue(self, item):
        node = Node(item)
        self.data.insertAt(self.size() + 1, node)

    def dequeue(self):
        return self.data.popAt(1)

    def peek(self):
        return self.data.getAt(1).data


if __name__ == "__main__":
    Q = LinkedListQueue()
    Q.enqueue(11)
    Q.enqueue(22)
    Q.enqueue(33)
    Q.enqueue(44)

    print(Q.data.traverse())
    Q.dequeue()
    print(Q.data.traverse())
    print(Q.data.getAt(1).data)
    print(Q.data.count)

```
