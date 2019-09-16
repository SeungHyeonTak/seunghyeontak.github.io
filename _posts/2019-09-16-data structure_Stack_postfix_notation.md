---
layout: post
title:  "자료구조. Stack_postfix_notation"
comments: true
description: "Stack"
author: SeungHyeon Tak
date:   2019-09-16 20:57:20 +0700
categories: [자료구조]
tags: [자료구조]
keywords: "자료구조"
---
#### 자료구조. Stack 후위표기법



> 중위 표기법 -> 후위 표기법
![stack11](https://user-images.githubusercontent.com/46446165/64956409-797e7b80-d8c5-11e9-87f8-68806e9c0c24.png)

> 이런식으로 연산자를 뒤로 빼주는것이 후위 표기법이다.
> 피연산자는 그대로 두고 연산자만 이동하는데
> 연산자끼리도 비교하며 Stack을 쌓아야한다.
> '*','/' > '+','-' 크기 순은 다음과 같이 되는걸 볼 수 있다.
> 만약 '*'가 먼저 쌓여있는데 '+'가 다음에 쌓여야할 data라면 stack에서 '*'을 pop()하고 그다음에 '+'을 push하면 된다.
> 연산중에 '(',')' 도 있는데 괄호는 제일 크기가 낮다.
> 괄호 연산도 중요한 부분중 하나인데 ')' (닫히는 괄호가 있으면) 지금까지의 stack은 전부 pop되어야 한다.
> stack에서 맨위에 있는 data랑 이제 들어가야할 data의 비교는 peek() 연산을 이용한다.

*****

#### Algorithm 설계

* 연산자 우선 순위 설정 (dict형태 사용)

```text
prec = {
    '*':3, '/':3,
    '+':2, '-':2,
    '(':1,
}
```
다음과 같이 설정 하고 prec에서 비교하며 쓰면 된다. <br>

* stack안에 높은data가 있으면 낮은data를 못넣는다.
![stack22](https://user-images.githubusercontent.com/46446165/64957140-36250c80-d8c7-11e9-9c3f-57d5d765bafa.png)

* 중위 표현식을 왼쪽부터 한글자씩 읽기
  * 피연산자이면 그냥 출력
  * '('이면 stack에 push
  * ')'이면 '('이 나올때까지 stack에서 pop

#### 후위표기법 코드

```python
# Arraystack 사용
class ArrayStack:
    def __init__(self):
        self.data = []

    def size(self):
        return len(self.data)

    def isEmpty(self):
        return self.size() == 0

    def push(self, item):
        self.data.append(item)

    def pop(self):
        return self.data.pop()

    def peek(self):
        return self.data[-1]


prev = {
    '*': 3, '/': 3,
    '+': 2, '-': 2,
    '(': 1
}


def solution(o):
    s = ArrayStack()
    answer = []
    for i in o:
        if i in prev:
            if i == "(":
                s.push(i)
            else:
                if s.isEmpty():
                    s.push(i)
                else:
                    if prev[i] <= prev[s.peek()]:
                        answer.append(s.pop())
                        s.push(i)
                    else:
                        s.push(i)
        elif i == ")":
            while s.peek() != '(':
                answer.append(s.pop())
            s.pop()
        else:
            answer.append(i)

    while not s.isEmpty():
        answer.append(s.pop())

    return ''.join(answer)


if __name__ == "__main__":
    o = '(A+B)*C'
    print(solution(o))
```

