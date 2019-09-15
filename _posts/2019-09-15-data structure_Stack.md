---
layout: post
title:  "자료구조. Stack"
comments: true
description: "Stack"
author: SeungHyeon Tak
date:   2019-09-15 19:04:20 +0700
categories: [자료구조]
tags: [자료구조]
keywords: "자료구조"
---
#### 자료구조. Stack


#### Stack?
> 자료를 보관 할 수 있는 (선형) 구조
>> 선형 : data item들이 일렬로 늘어져 있는 형태
> 단, 넣을때에는 한쪽 끝에서 밀어 넣어야 하고 꺼낼떄에는 같은쪽에서 뽑아 꺼내야 하는 제약이 있음
> 기본적으로 후입선출(LIFO)형식을 가지는 선형 자료구조라한다.

![stack1](https://user-images.githubusercontent.com/46446165/64919814-34d1e200-d7ea-11e9-85d2-44731e42536a.png)

> 이런식으로 push와 pop이 진행 되는걸 알 수 있다.

>> 반면에 꽉찬 스택에 데이터 원소를 넣으려 할때 또 넣으려 하면 stack overflow가 발생한다.
>> 그리고 더 이상 빼낼 원소가 없는데 빼내려고 하면 stack underflow가 발생한다.

#### 스택의 추상적 자료구조 표현
* 연산의 정의
  * size() - 현재 스택에 들어있는 데이터 원소의 수를 구함
  * isEmpty() - 현재 스택이 비어 있는지를 판단
  * push(n) - 데이터 원소 n을 스택에 추가
  * pop() - 스택의 맨위에 저장된 데이터 원소를 제거(or 반환)
  * peek() - 스택의 맨위에 저장된 데이터 원소를 반환 (제거하지 않음)

* 배열(Array)을 이용하여 구현
  * python list와 메서드들을 이용할 수 있다.

```python
class ArrayStack:
    def __init__(self):
        self.data = []

    def size(self):
        return len(self.data)

    def isEmpty(self):  # size 메서드를 이용해서 스택이 비어있는지 판단할 수 있음
        return self.size() == 0

    def push(self, item):
        self.data.append(item)

    def pop(self):  # 맨 끝에 있는 원소 제거
        return self.data.pop()

    def peek(self):  # 맨 끝에 있는 원소를 보여준다.
        return self.data[-1]


if __name__ == "__main__":
    s = ArrayStack()
    s.push(1)
    s.push(2)
    s.push(3)
    s.push(5)

    print(s.data)
    print(s.size())
    print(s.isEmpty())
    print(s.peek())
```

* 연결리스트(Linked List)를 이용하여 구현
  * 양방향 연결리스트를 이용하여 구현한 방법

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

    # n번째
    def get(self, pos):
        if pos < 0 or pos > self.count:
            return None

        if pos > self.count // 2:
            i = 0
            curr = self.tail
            while i < self.count - pos + 1:
                curr = curr.prev
                i += 1
        else:
            i = 0
            curr = self.head
            while i < pos:
                curr = curr.next
                i += 1
        return curr

    # 순방향
    def traverse(self):
        answer = []

        curr = self.head

        while curr.next.next:
            curr = curr.next
            answer.append(curr.data)
        return answer

    # 역방향
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

        prev = self.get(pos - 1)
        return self.insertAfter(prev, newnode)

    def insertAfter(self, prev, newnode):
        next = prev.next
        newnode.prev = prev
        newnode.next = next
        prev.next = newnode
        next.prev = newnode
        self.count += 1

        return True

    def popAt(self, pos):
        if pos < 0 or pos > self.count:
            raise IndexError

        prev = self.get(pos - 1)
        return self.popAfter(prev)

    def popAfter(self, prev):
        curr = prev.next
        prev.next = curr.next
        curr.next.prev = prev
        self.count -= 1
        return curr.data


class LinkedListStack:
    def __init__(self):
        self.data = DoubleLinkedList()

    def size(self):
        return self.data.count

    def isEmpty(self):
        return self.size() == 0

    def push(self, item):
        node = Node(item)
        self.data.insertAt(self.size() + 1, node)

    def pop(self):
        return self.data.popAt(self.size())
        # 현재 size에서 제일 위에 있는걸 삭제 해야하기 때문에 count를 그대로 가져다 쓰면 됨

    def peek(self):
        return self.data.get(self.size()).data


if __name__ == "__main__":
    L = LinkedListStack()
    L.push(11)
    L.push(22)
    L.push(33)
    L.push(55)

    print(L.data.traverse())
    print(L.size())
    print(L.peek())
    print(L.isEmpty())
    L.pop()
    print(L.size())
    print(L.data.traverse())
```
