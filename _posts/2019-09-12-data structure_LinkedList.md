---
layout: post
title:  "data structure. Linked List"
comments: true
description: "data structure, Linked List"
author: SeungHyeon Tak
date:   2019-09-12 01:12:20 +0700
categories: [data structure]
tags: [data structure]
keywords: "data structure, Linked List"
---
#### data structure. Linked List

#### 단방향 연결 리스트
> 연결리스트란? 각원소를 줄줄이 엮어서 관리하는 방식으로 볼 수 있다. (앞에 있는 것이 뒤에 것을 가르킫록 데이터를 늘여 놓은것!)
> 배열과의 차이점은 빠른 시간내에 처리할 수 있다는 점이다.

![node1](https://user-images.githubusercontent.com/46446165/64715986-73bb1b80-d4fc-11e9-83a7-7246acd484f2.png)

> 여기서 Node에 대해 알아 볼 수 있는데
>
> 빨간 동그라미 부분을 data
>
> 파란 동그라미 부분을 next 이렇게 data와 next가 하나의 Node이다.
>
> next는 다음 node의 data를 이어주는 역할을 한다고 생각하면 된다.
>
> 그리고 제일 앞의 Node와 뒤에 Node는 각각 head와 tail이라고 불린다.


> 연결리스트는 안의 Node가 몇개인지 알아야함으로 Count나 length로 표시 해서 node를 세어주면 편리하다.
>
> 그리고 우리가 Linked List를 만들기전에 비어 있는 node를 만들어 줘야하는데
>
> 앞서 말한 head / tail / count를 초기 setting 해준다.
>
> head와 tail은 None으로 선언하고 count는 0으로 선언 해주면 비어 있는 Linked List가 된다.
>
> 그리고 index는 0이 아닌 1부터 사용 하는걸 권장 한다. 0은 나중에 다른 목적으로 다뤄 보기 위해 사용될 예정이다.


#### 특정 원소 찾기
> list에서 특정원소를 찾아 보려 한다. 그러기 위해선 get이라는 메서드를 쓸 수 있다.
>
> 쉽게 보기 위해 그림을 하나 준비하였다.

![node2](https://user-images.githubusercontent.com/46446165/64717076-8f272600-d4fe-11e9-85fc-25a61310510b.png)

> 위 그림은 아직 우리가 직접 넣진 않았지만 넣었다는 가정하에 만들어본 Linked List이다.
>
> 우리는 index로 통해 List의 data를 찾아야 한다.
>
> 처음 비교 할 대상은 index가 get메서드에 일치 하느냐를 따지는 부분이다.
>
> 일단 index는 1부터 시작해야 한다. 그리고 Node의 갯수 보다 크면 안된다는걸 알 수 있다.
>
> (index가 node 갯수 보다 크면 Error 나야한다고 생각하는게 당연하죠?)
>
> 그러므로 첫번째 비교에 적합하게 되면 None을 return 해버리면 된다.
>
> 그리고 두번째 비교를 하게 되면
>
> ※특정 원소 찾기는 시간복잡도에서 O(n)이기 때문에 하나하나 찾아가야한다.
>
> 일단 내가 파라미터로 받아온것이 index이기 때문에 어느 몇번째 위치의 원소를 찾는걸 알기 위해 i를 선언해주고 
>
> 그리고 curr = self.head로 지정해준다. 왜냐? 처음 부터 돌아야 하기 때문에!!
>
> 이제 반복문으로 돌리며 내가 찾는 원소 번째까지 돌려야 한다.
>
> 꼭 curr.next를 써야 다음 원소로 넘어 갈 수 있다.



#### List 순회

> 우리가 list를 추가하면서 잘 추가 되었는지 삭제 하면 잘 삭제가 되었는지 display할 메서드가 필요하다.
>
> 그러므로 새로운 list를 선언하고 data를 list에 추가 하고 보여주는 형식으로 return 하면 마무리가 된다.
>
> 순회 부분은 쉬으므로 여기까지 하고 넘어간다
>
> 한가지 팁을 주자면 돌리고자 하는 curr이 None이 되면 출력을 시키면된다. 제일 끝이 None으로 끝나기 때문에 더이상 반복문을 돌릴 필요가 없기 때문이다.



#### Node Add(삽입)

> 우리가 Node를 추가 하기 위해 꼭 필요한 부분이다. 
>
> 중요한 부분이다.
>
> 첫번째로 우리는 add의 파라미터를 index, newnode로 설정 해준다는 사실을 잊지 말자

![node3](https://user-images.githubusercontent.com/46446165/64718507-50469f80-d501-11e9-9c79-d2e41c0d9391.png)

> 위처럼 추가 하고자 하는 Node가 있는데 대체 어떻게 해야 할지 모르겠다고 생각 할 수도 있다.
>
> 하지만 이제 부터 천천히 보면 알 수 있다.
>
> 처음으로 우리가 만들어준 get메서드를 쓸 수 있는데 get(index-1)을 하면 선택하고자 하는 바로 뒤 Node를 선택할 수 있다. 이렇게 선택하게 되면 우리는 get(index-1)의 next 가 어디로 가는지 알 수 있다. 그러면 newnode.next를 get(index-1)의 next로 보내고 get(index-1)의 next는 newnode로 연결 시켜 주면 밑의 그림 처럼 되는걸 알 수 있다.
>
> 아! 이때 count를 +1을 해줘서 늘려 나아가면 된다.

![node4](https://user-images.githubusercontent.com/46446165/64719219-c4357780-d502-11e9-9ae6-109e9350cfbd.png)

> 이 부분을 원할하게 구현 하였으면 응용  단계가 있는데 직접 해보길 바란다.
>
> 1. 삽입하려는 리스트 위치가 맨앞일 경우
> 2. 삽입하려는 리스트 위치가 맨뒤일 경우

#### Node Delete(삭제)

> add부분과 마찬가지로 순서대로 하면 어려운 부분은 없을 것이다.
>
> 일단 이 부분은 파라미터가 index만 들어 가게된다.
>
> 우선 index-1 부분을 prev라고 선언한다면 prev의 next는 우리가 삭제 할 노드가 된다.
>
> 우리가 삭제 할 Node를 curr이라고 칭한다면
>
> prev.next = curr.next 이렇게 코드 구현 하면 

![node5](https://user-images.githubusercontent.com/46446165/64719811-04492a00-d504-11e9-9e17-5f07f7845ce4.png)

> 위의 그림 처럼 바뀌게 될 것이다. 그럼 우리는 count를 -1하고 진행 하고 display 메서드를 실행하면 정상 적으로 나올것이다.
>
> 이번건도 응용해야할 부분이 있다. 위와 마찬가지로 풀어보길 바란다.
>
> 1. 삭제하려는 리스트 위치가 맨앞일 경우
> 2. 삭제하려는 리스트 위치가 맨뒤일 경우



```
자료구조는 직접 써가며 공부하는게 정말 도움이 된다고 생각한다. 물론 저도 그렇게 공부 중이라서 말씀드리는겁니다 ㅎㅎ
```


[github](https://github.com/SeungHyeonTak/data_structures/blob/master/Linked%20Lists.py)

Linked List code는 위 링크에 걸려 있으니 가서 참고 가능하다.

