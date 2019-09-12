---
layout: post
title:  "data structure. Dummy Linked List"
comments: true
description: "data structure, Linked List, dummy linked List"
author: SeungHyeon Tak
date:   2019-09-12 01:12:20 +0700
categories: [자료구조]
tags: [자료구조]
keywords: "data structure, Linked List, dummy linked List"
---
#### data structure. Dummy Linked List

> dummy node 란?
> 맨 앞에 head가 가르키고 있는 부분이다. 기능은 하지않고 자리만 잡혀 있는 부분이다.

![node1](https://user-images.githubusercontent.com/46446165/64792313-2ef2bb80-d5b4-11e9-90e0-32f8be16fec9.png)

> 저번 포스팅을 보면 index 0 부분을 비워둔 이유가 이러한 이유 때문이다.

```
연결리스트가 가장 유용하게 사용될때는 삽입 / 삭제가 유연하다는 것이 큰 장점이다.
링크만 조절해주면 빠르게 삽입 / 삭제가 가능하다.
하지만 우리가 앞에서 봐왔던 코드는 처음부터 시작해서 링크를 따라가면서 N번째에 있는 Node를 찾아가야 하기 때문에
새로운 메서드 두개를 작성할 수 있다.
- addAfter(prev, newnode)
- deleteAfter(prev)
이렇게 두개의 메서드를 준 이유는 어떤 Node를 주고서 그 뒤에 삭제해야 삽입 해라를 구현하기 위해 메서드를 만들어 준것이다.
dummy node를 만들어주기위한 새로운 클래스를 구현해야한다.
class LinkedList:
	def __init__(self):
		self.head = Node(None)
		self.tail = None
		self.head.next = self.tail
		self.count = 0
이렇게 dummy가 생긴 새로운 리스트가 만들어 졌다.

나머지는 앞서 봐온 포스팅 부분과 유사하기 때문에 (조금 달라짐) 위의 두개의 메서드를 추가 하여 풀어보면 된다.
```

[github](https://github.com/SeungHyeonTak/data_structures/blob/master/dummy%20Linked%20List.py)


