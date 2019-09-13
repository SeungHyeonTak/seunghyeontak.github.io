---
layout: post
title:  "자료구조. Double Linked List"
comments: true
description: "자료구조, Linked List, double linked List"
author: SeungHyeon Tak
date:   2019-09-12 01:12:20 +0700
categories: [자료구조]
tags: [자료구조]
keywords: "자료구조"
---
#### 자료구조. Double Linked List


#### 양방향 연결리스트란?
> 단방향 연결리스트 처럼 한 쪽으로만 연결하지 않고 양쪽으로 연결된 리스트를 말한다.

![dl1](https://user-images.githubusercontent.com/46446165/64874004-2f0ebc00-d685-11e9-9f11-b27364795d33.png)

> 이런식으로 앞뒤로 link가 이어져 있는 형태이다.
> next와 prev의 진행도를 알아 보자

![dl2](https://user-images.githubusercontent.com/46446165/64874214-8c0a7200-d685-11e9-9462-699d0dce3569.png)

#### Node의 구조 확장

```python
class Node:
	def __init__(self, item):
		self.data = item
		self.next = None
		self.prev = None
		
class DoubleLinkedList:
	def __init__(self):
		self.head = Node(None)
		self.tail = Node(Node)
		self.head.prev = None
		self.head.next = self.tail
		self.tail.prev = self.head
		self.tail.next = None
```

> 빈 리스트형태가 만들어 졌다. 싱글 링크드 리스트와는 다르게 tail부분도 dummy node 처리를 해줬다.

![dl3](https://user-images.githubusercontent.com/46446165/64874943-fb349600-d686-11e9-9f75-e27c691626d7.png)

> 여기서 궁금한점?
> 양방향과 다르게 단방향은 왜 tail을 dummy node를 두지 않았나?
> -> 마지막 노드를 tail로 지정 해줌으로서 삽입 / 삭제를 효과적으로 처리 할 수 있어서이다.

#### 리스트 순회 (traverse)

> 단방향과 달라진 점이 있다면 tail로 지정된 dummy node 까지 생각해야 한다는 점이다.
> 그렇게 while 문에 적용 시키면 되고 나머지는 바뀔점이 없다.

#### 원소 찾기(n번째 list 찾기)

> 여기서는 좀 생각해야할 부분이다.
> list가 수없이 길어질수도 있는 상황을 생각하여 무조건 앞이 아닌 뒤에서 찾아가는 방법도 고려 하여 찾는것이 효율 적이다.
> 내가 찾는 index값의 중간값을 찾고 그 보다 높으면 뒤 낮으면 앞에서 찾아가는 방식으로 코드를 변형 시키자

#### 원소 삽입(insert)

> 앞의 단방향 리스트를 공부하고 왔다면 양방향 리스트의 원소 삽입은 쉽게 느껴질것이다.
> 따로 조건문을 쓰지 않고 원소간의 연결만 해주면 쉽게 접근 할 수 있다.
> 삽입을 하였다면 삭제도 스스로 해보자 (개념만 잡고 있다면 누구든 쉽게 가능하다.)


> 코드는 밑의 github에서 확인 할 수 있다.
[github](https://github.com/SeungHyeonTak/data_structures/blob/master/Double%20Linked%20List.py)