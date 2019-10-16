---
layout: post
title:  "python property 사용하기"
comments: true
description: "Python, property"
author: SeungHyeon Tak
date:   2019-10-16 23:30:23 +0700
categories: [python]
tags: [python]
keywords: "python, getter, setter, property"
---
#### property 사용하기
<br>
> 다음과 같이 클래스에서 메서드를 통하여 속성의 값을 가져오거나 저장하는 경우가 있음 <br>
> 이때 값을 가져오는 메서드를 getter, 값을 저장하는 메서드를 setter라고 부름 <br>

```python
class Person:
    def __init__(self):
	self.__age = 0

    def get_age(self):    # getter
	return self.__age

    def set_age(self, value):    # setter
	self.__age = value

if __name__ == "__main__":
    james = Person()
    james.set_age(20)
    print(james.get_age())
```

> 파이썬에서는 @property를 사용하여 getter / setter를 간단하게 구현할 수 있음 <br>
>> * @property <br>
>> * @method_name.setter <br>
> * _name : 내부적으로 사용하는 변수
> * __name : class 외부적으로 접근 안됨

```python
class Person:
    """
    @property를 사용하여 getter setter 구현
    """

    def __init__(self):
        self.__age = 0

    @property  # 값을 가져오는 메서드 (getter)
    def age(self):
        return self.__age

    @age.setter  # 값을 저장하는 메서드 (setter)
    def age(self, value):
        self.__age = value


if __name__ == "__main__":
    james = Person()
    james.age = 20  # 인스턴스.속성 형식으로 접근하여 값 저장
    print(james.age)  # 인스턴스.속성 형식으로 값을 가져옴

```
