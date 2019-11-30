---
layout: post
title:  "python_packing과 unpacking에 대해서"
comments: true
description: "Python"
author: SeungHyeon Tak
date:   2019-11-30 22:40:00 +0700
categories: [python]
tags: [python]
keywords: "python"
---
## python packing과 unpacking에 대해서

일단 글의 주제에 앞서 먼저 이해해야할 개념은 `*args`와 `**kwargs`이다. <br>

### args

보통 함수를 호출할 때 많은 경우 인자를 입력한다. <br>

args는 positional argument의 위치 인자이다. <br>

`help(print)`를 통해 print에 들어갈 수 있는 값을 확인해보면 <br>

`print(value, ..., sep=' ', end='\n', file=sys.stdout, flush=False)`이런식으로 나오는데 <br>

value처럼, 정해진 값이 없이 함수를 실행할 때 꼭 필요한 인자를 positional Argument라고 한다. <br>

그냥 value만 가진 값들을 넣는다고 보면 된다. <br>

```python
k = int('3')
args = print('a', 'b', 'c', '\', '', fp, True)
# 이렇게 들어갈 수 있는 인자들을 args로 입력할 수 있다.
```

### kwargs

kwargs는 키워드 인자이다. <br>

인자를 `value`만 넣는게 아닌 `key=value`형태로 넣는것이다. <br>

예시를 보여주면 `kwargs = print(flush='True')`에서 flush이라는 인자를 명시해서 그 값을 True로 지정해주는것 처럼 key=value형식으로 지정되어야한다. <br>


### Packing

packing은 함수를 정의할때 사용된다. 함수를 만든다고 하였을때, 많은 인자를 사용하여 만들고 싶은 상황이다. <br>

이때 packing은 받는 인자를 하나의 튜플과 하나의 딕셔너리로 묶어서 전달해주는 것을 말한다.

```python
# 예시 코드
def myprint(*args, **kwargs):
    ans = ''
    for i in args:
	ans += str(i)
    for key, value in kwargs.items():
	ans += str(value)
    return ans

myprint(1,2,3,4, k='?', l='ok')

>>> 1234?ok # 이렇게 출력될것이다.
```

### Unpacking

패킹이 함수를 정의할 때 사용된다면, Unpacking은 반대로 함수를 실행할 때 사용된다. <br>

```python
def myprint(*args):
    for i in args:
        print(i)

list = ['1', '2', '3', '4', 'a']
myprint(*list)
```

언패킹은 함수 사용시에 활용된다. <br>

myprint(*list)에서 list앞에 *가 붙어있다. 이것이 언패킹이다. <br>

`*`를 붙이면 iterable한 list가 모두 풀어져 인자에 들어가게된다. <br>

그러면 여기서 언패킹을 안했을땐 어떻게되는지 알아보자 <br>

**myprint(list)** 이러면 list가 문자열을 담은 리스트 하나로 함수안에 들어간다. <br>

`['1', '2', '3', '4', 'a']`로 출력될것이다. <br>


#### Packing & Unpacking의 정리

* packing - 함수 정의할 때 - 함수 실행시에 받은 모든 원소를 묶고 싶을때
* unpacking - 함수 사용할 때 - 입력하는 리스트, 딕셔너리 등을 풀어서 함수에 넣고 싶을때


