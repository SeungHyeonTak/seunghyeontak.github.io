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
## 자료구조. Queue


#### Queue란?
> 자료를 보관할 수 있는 선형구조(스택과 동일)
> 넣을때에는 한쪽 끝에서 밀어 넣기 -> enqueue
> 꺼낼때에는 반대쪽에서 뽑아 꺼내야 하는 제약이 있음 -> dequeue
> 가장 먼저 넣은 데이터를 가장 먼저 꺼낼 수 있는 구조 즉, 선입선출(FIFO) 특징을 가지는 자료구조 선형
<br>
![queue](https://user-images.githubusercontent.com/46446165/64964674-7a6bd900-d8d6-11e9-89d1-679459335280.png)
<br>
![queue2](https://user-images.githubusercontent.com/46446165/64964785-b4d57600-d8d6-11e9-80e5-3c20ff413c6e.png)
<br>
> 그림을 보면 먼저 들어간게 먼저 나오는 형식으로 된것이 Queue이다.

#### 알아둘 용어

* Enqueue : 큐에 데이터를 넣는 기능
* Dequeue : 큐에서 데이터를 꺼내는 기능

#### python Queue 라이브러리 활용하기

Queue의 라이브러리에는 다양한 큐 구조로 Queue(), LifoQueue(), PriorityQueue()가 있다.

* Queue() : 가장 일반적인 큐 자료구조
* LifoQueue() : 나중에 입력된 데이터가 먼저 출력되는 구조
* PriortyQueue() : 데이터마다 우선순위를 넣어서 우선순위가 높은순으로 데이터 출력

#### LIFO Queue()

LIFO (Last-In First-Out) - 마지막에 넣은게 먼저 나옴

```python
import queue

data_queue = queue.LifoQueue()

data_queue.put("funcoding")
data_queue.put(1)
data_queue.qsize() # 데이터가 2개 들어갔으니 2가 출력
data_queue.get() # LIFO 방식이니 마지막에 넣은 1이 출력
```

#### Priority Queue()

```python
import queueu

data_queue = queue.PriorityQueue()

data_queue.put((10, "korea")) # 튜플 형식으로 저장됨 ((우선순위, "데이터")) 로 저장됨
data_queue.put((5, 1))
data_queue.put((15, "china"))

data_queue.qsize() # 당연히 데이터를 3개 넣었으니 3이 출력
data_queue.get() # (5,1)가 출력 될것이다. 이유) 위에서 말했듯이 이 형식은 튜플로 저장됨으로 앞에 들어가는 데이터는 우선순위로 적용되기 때문
```

#### Queue로 큐 만들기

```python
import queue

data_queue = queue.Queue() # queue의 라이브러리로 class Queue를 가져다 씀

data_queue.put('Hi') # 데이터를 넣을땐 put을 쓴다.
data_queue.put(1)

data_queue.qsize() # queue안에 몇개의 데이터가 있는지 확인

data_queue.get() 
# get은 데이터를 빼낼때 사용함, 인자를 넣지 않는 이유는 queue구조상 FIFO이기 때문에 인자를 넣을 필요 없이 제일 먼저 들어간 데이터가 나올꺼기 때문이다.

data_queue.qsize() # 데이터가 1r개 남은걸 알 수 있다.

```

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

#### 큐가 많이 쓰이는 부분

멀티태스킹을 위한 프로세스 스케줄링 방식을 구현하기 위해 많이 사용된다.(운영체제에서 나오는 내용)

큐의 경우에는 장단점보다 큐의 활용 예로 프로세스 스케줄링 방식을 함께 이해 해두는것이 좋다.
