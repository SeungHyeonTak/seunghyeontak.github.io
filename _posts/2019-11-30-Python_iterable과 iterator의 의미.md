---
layout: post
title:  "python_iterable과 iterator의 의미"
comments: true
description: "Python"
author: SeungHyeon Tak
date:   2019-11-30 23:58:00 +0700
categories: [python]
tags: [python]
keywords: "python, iterable, iterator"
---
## python iterable & iterator

### Iterable

* iterable의 의미는 member를 하나씩 차례대로 반환 가능한 말한다 object를.
* iterable 객체 - 반복 가능한 객체
* iterable한 타입
  * list
  * dict
  * set
  * bytes
  * tuple
  * range
* 반복문에서 iterable한 타입을 확인하는 방법이 있다.

python을 접해봤으면 for문의 range()를 사용해본 경험이 있을것이다. 이때 생성된 list가 iterable하기 때문에 순차적으로 member들을 불러서 사용이 가능했던 것이다. <br>

비순차적 타입인 dict나 file도 iterable하다고 말할 수 있다. <br>

dict 또한 for문을 이용해 순차적으로 접근이 가능하다. <br>

### Iterator

* iterator 객체 - `값을 차례대`로 꺼낼 수 있는 객체이다.
* iterator는 iterable한 객체를 내장함수 또는 iterable객체의 메소드로 객체를 생성할 수 있다.
* 파이썬 내장함수 `iter()`를 사용해 iterator 객체를 만들어본다.

```python
a = [1,2,3] # 리스트형으로 
a_iter = iter(a)
type(a_iter) # 타입 확인

b = {1, 2, 3}
dir(b)
b_iter = b.__iter__()
type(b_iter)
# <class 'list_interator'>
```

* next라는 파이썬 내장함수를 사용할때마다 첫번째, 두번째, 세번째 값이 출력된다.
* 더이상 출력 될 값이 없으면 `stopinteration`이라는 예외가 발생한다.

```python
next(a_iter)
# 1

next(a_iter)
# 2

next(a_iter)
# 3

b_iter.__next__()
# 1

b_iter.__next__()
# 2

b_iter.__next__()
# 3
```
