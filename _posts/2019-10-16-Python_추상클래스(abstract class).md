---
layout: post
title:  "python 추상클래스 (abstract class)"
comments: true
description: "Python, abstract class, ABC"
author: SeungHyeon Tak
date:   2019-10-16 23:06:23 +0700
categories: [python]
tags: [python]
keywords: "python"
---
#### 추상클래스(abstract class)
<br>
> 추상클래스란 미구현 추상메소드를 한개 이상 가지며, 자식 클래스에 해당 추상 메소드를 반드시 구현하도록 강제한다. <br>
> (자식클래스에서 반듯이 구현해야 되는 기능을 가진 부모클래스)<br>
>> 추상클래스는 메서드의 목록만 가진 클래스이며, 상속받는 클래스에서 메서드 구현을 강제하기 위해 사용함 <br>
<br>
> * 상속받은 클래스는 추상메소드를 구현하지 않아도, import할 때까지 에러는 발생하지 않으나 객체를 생성할 시 에러가 발생한다. <br>
> * 반드시 abc 모듈을 import 해야한다. <br>
> * 추상메소드는 생략하면 기본적인 클래스 기능은 동작한다. 추상메소드를 추가한 후에는 객체를 생성하면 에러가 발생 <br>
> * 추상클래스는 @abstractmethod 데코레이터를 활용한 1개 이상의 abstract method를 가져야 한다.<br>
> * 추상클래스를 정의 할 때는 from abc import * 모듈 적용이 필요하다.<br>

```python
from abc import *


class StudentBase(metaclass=ABCMeta):
    """
    * 추상 클래스는 인스턴스로 만들 때는 사용하지 않으며 오로지 상속에만 사용한다.
    * 파생 클래스에서 반드시 구현해야 할 메서드를 정해 줄 때 사용한다.
    """

    @abstractmethod
    def study(self):
        pass  # 추상 메서드는 호출할 일이 없으므로 빈 메서드로 만듦

    @abstractmethod
    def go_to_school(self):
        pass

    def fun(self):
        pass


class Student(StudentBase):  # 파생 클래스 구현
    """
    StudentBase에서 상속받을때 study만 가져와서 쓰면 Error
    추상메서드(AbstractBase Class)를 가져와 쓸때는 @abstractmethod가 붙은 함수는 모두 가져와서 써야함
    """

    def study(self):
        print("공부하기")

    def go_to_school(self):
        print("학교가기")


if __name__ == "__main__":
    """
    추상 클래스는 인스턴스로 만들수 없다.
    밑에 보면 추상클래스를 상속 받은 클래스를 인스턴스로 만든걸 볼 수 있는데,
    (예를 들어 james = StudentBase() 이렇게 만들 수 없다)
    """
    james = Student()
    james.study()
    james.go_to_school()

```
