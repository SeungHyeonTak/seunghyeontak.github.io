---
layout: post
title:  "python_클래스 작성하는 법"
comments: true
description: "Python"
author: SeungHyeon Tak
date:   2019-11-23 21:08:00 +0700
categories: [python]
tags: [python]
keywords: "python"
---
#### python class 설계하기
##### 5가지 클래스 설계의 원칙 (S.O.L.I.D)
* S - SRP 단일 책임 원칙
* O - OCP개방 (폐쇄원칙)
* L - LSP 리스코프 치환 법칙
* I - ISP 인터페이스 분리 원칙
* D - DIP 의존성 역전 법칙
<br>
##### 클래스 설계 팁
직접 많이 작성하며 이해하는 부분이지만, 처음에는 들어봤다가 중요하다 <br>
원칙을 이해하면서, 각각의 `클래스 설계 예를 들여다보고`, 이해를 한다는 느낌으로 접근 한다. <br>

#### 실제 현업에서의 코드 작성시 주의하는 부분
> 중복코드 제거는 매우 좋음 <br>

* 단 두번이라도 사용된다면, 코드 수정시 각 코드를 모두 수정해야 하기 때문
* 여러 동일한 코드를 수정하다보면 시간도 많이 소요되고, 복잡도 높아지고, 버그 가능성이 많음
* `중복 코드는 함수로 만들던지`, `클래스 메서드/인터페이스로 만들던지` 반드시 하나의 코드로 만드는것이 필요함
<br>
> 최적화는 제일 나중에 할것: 굳이 잘돌아가면, bottleneck이 존재할때만 <br>

괜히 중간중간에 최적화 한다고 계속 바꾸게 되면, 시간만 많이 들 수 있다. <br>
중간중간 수정해서 최고의 코드로 만든다해도 `결국 바뀌는 경우가 있으므로` 최적화는 나중에!! <br>

*****

#### S - SRP 단일 책임 원칙
* 클래스는 단 한개의 책임을 가져야한다. (`클래스를 수정할 이유가 오직 하나`이어야 한다.)
* 예를 들어 계산기 기능 구현시 계한을 하는 책임과 GUI를 나타낸다는 책임을 서로 분리하여, 각각 클래스로 설계해야한다.
* 실제로는 애매한 부분이 많이 존재한다. 가급적 설계시 이런 부분을 염두에 두자가 합리적일 수 있다.
<br>

```python
# 나쁜 예
# 학생성적과 수강하는 코스를 한개의 class에서 다루는 예
# 한 클래스에서 두개의 책임을 갖기 때문에, 수정이 용이하지 않다.
class StudentScoreAndCourseManager(object):
    def __init__(self):
        scores = {}
        courses = {}
        
    def get_score(self, student_name, course):
        pass
    
    def get_courses(self, student_name):
        pass
    
# 변경 예
# 각각의 책임을 한개로 줄여서, 각각 수정이 다른 것에 영향을 미치지 않도록 함
class ScoreManager(object):
    def __init__(self):
        scores = {}
        
    def get_score(self, student_name, course):
        pass
    
    
class CourseManager(object):
    def __init__(self):
        courses = {}
    
    def get_courses(self, student_name):
        pass
```

<br>

#### O - OCP개방 - 폐쇄 원칙
* 확장에는 열려있어야 하고, 변경에는 닫혀있어야 한다.
* 예를 들어 캐릭터 클래스를 만들 때, 캐릭터마다 행동이 다르다면, 행동 구현은 캐릭터 클래스의 자식 클래스에서 재정의(Method Override)한다.
  * 이 경우 캐릭터 클래스는 수정할 필요없다(변경에 닫혀 있음)
  * 자식 클래스에서 재정의하면 된다(확장에 대해 개방됨)

```python
# 나쁜 예
class Rectangle(object):
    def __init__(self, width, height):
        self.width = width
        self.height = height
        
class Circle:
    def __init__(self, radius):
        self.radius = radius

class AreaCalculator(object):
    def __init__(self, shapes):
        self.shapes = shapes

    def total_area(self):
        total = 0
        for shape in self.shapes:
            total += shape.width * shape.height
        return total

shapes = [Rectangle(2, 3), Rectangle(1, 6)]
calculator = AreaCalculator(shapes)
print("The total area is: ", calculator.total_area())

###################

# 좋은 예
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height
    
class Circle:
    def __init__(self, radius):
        self.radius = radius
        
    def area(self):
        return 3.14 * self.radius ** 2
    
    
'''
다른 도형에 대해 확장하기 위해서,
AreaCalculator는 수정이 필요 없음 (변경에 닫혀 있음)
단지, Shape을 상속받은 다른 class를 정의하기만 하면 됨 (확장에 대해 개방됨)
'''
class AreaCalculator(object):
    def __init__(self, shapes):
        self.shapes = shapes

    def total_area(self):
        total = 0
        for shape in self.shapes:
            total += shape.area()
        return total


shapes = [Rectangle(1, 6), Rectangle(2, 3), Circle(5), Circle(7)]
calculator = AreaCalculator(shapes)

print("The total area is: ", calculator.total_area())

```

<br>
#### L - LSP 리스코프 치환 법칙
> 자식 클래스는 언제나 자신의 부모클래스와 교체할 수 있다는 원칙 <br>

<br>
* iphone is a kind of smartphone
  * 스마트폰은 다른 사람과 전화와 메시지가 가능하다.
  * 스마트폰은 데이터 또는 와이파이를 이용해 인터넷을 사용할 수 있다.
  * 스마트폰은 앱 스토어를 통해 앱을 다운 받을 수 있다.

* 위 설명을 아이폰으로 대체하면 이렇게 된다.
  * 아이폰은 다른사람과 전화와 메시지가 가능하다.
  * 아이폰은 데이터 또는 와이파이를 이용해 인터넷을 사용할 수 있다.
  * 아이폰은 앱 스토어를 통해 앱을 다운 받을 수 있다.

#### I - ISP 인터페이스 분리 원칙
* 클래스에서 사용하지 않는(상관없는) 메서드는 분리해야함

```python
# 추상클래스
from abc import *

class Character(metaclass=ABCMeta):
    @abstractmethod
    def attack(self):
        pass
    
    @abstractmethod
    def move(self):
        pass

    @abstractmethod
    def eat(self):
        pass
```

`metaclass`란 무엇일까? <br>
* 클래스를 만들기위해 파이썬에서는 기본 metaclass가 사용됨
  * 쉽게말해 클래스를 만들기 위해서 metaclass라는것이 필요함
  * class 생성시, ()아무것도 넣지 않으면, 기본 파이썬에서 클래스를 만들기 위한 메타클래스가 쓰인다고 보면 된다.
  * 추상  클래스 만들시에는 기본 메타클래스로는 생성이 어려우므로, 다음과 같이 생성
    * `class Charactor(metaclass=ABCMeat)`
  * 싱글톤을 위해 기본 metaclass를 바꾸는것이다. (싱글톤에 대해서는 디자인패턴 부분에서 설명)
    * class PrintObject(mateclass=Singleton)


```python
# 나쁜 예제로 알아보는 추상클래스 상속하기

from abc import *

class Character(metaclass=ABCMeta):
    @abstractmethod
    def attack(self):
        pass
    
    @abstractmethod
    def move(self):
        pass


class Elf(Character):
    def attack(self):
        print ("practice the black art")
    
    def move(self):
        print ("fly")

        
class Human(Character):
    def attack(self):
        print ("plunge a knife")
    
    def move(self):
        print ("run")

    def eat(self):  # <--- 메서드 확장
        print ("eat foods")

if __name__ == '__main__':
    elf1 = Elf()
    human1 = Human()

    elf1.attack()
    elf1.move()
    human1.attack()
    human1.move()
    human1.eat()
```

#### D - DIP 의존성 역전 법칙
* 부모 클래스는 자식 클래스의 구현에 의존해서는 안됨
  * 자식 클래스 코드 변경 또는 자식 클래스 변경시, 부모 클래스 코드를 변경해야 하는 상황을 만들면 안됨
* 자식 클래스에서 부모 클래스 수준에서 정의한 추상 타입에 의존할 필요가 있음

```python
# 나쁜예
class BubbleSort:
    def bubble_sort(self):
        # sorting algorithms
        pass

class SortManager:
    def __init__(self):
        self.sort_method = BubbleSort() # <--- SortManager 는 BubbleSort에 의존적
        
    def begin_sort(self):
        self.sort_method.bubble_sort() # <--- BubbleSort의 bubble_sort 메서드에 의존적

# BubbleSort의 bubble_sort 메서드 변경
class BubbleSort:
    def sort(self):
        print('bubble sort')
        pass

# => 에러 출력
```

```python
# 좋은 예
class SortManager:
    def __init__(self, sort_method):    # <--- 의존성을 주입시킨다고 이야기함
        self.set_sort_method(sort_method)
        
    def set_sort_method(self, sort_method):
        self.sort_method = sort_method
        
    def begin_sort(self):
        self.sort_method.sort()         # <--- 하부 클래스가 바뀌더라도, 동일한 코드 활용 가능토록 인터페이스화

class BubbleSort:
    def sort(self):
        print('bubble sort')
        pass

class QuickSort:
    def sort(self):
        print('quick sort')
        pass

bubble_sort1 = BubbleSort()
quick_sort1 = QuickSort()

sorting1 = SortManager(bubble_sort1)
sorting1.begin_sort()

sorting2 = SortManager(quick_sort1)
sorting2.begin_sort()
```

```python
# 좋은 예
class SortManager:
    def __init__(self, sort_method):    # <--- 의존성을 주입시킨다고 이야기함
        self.set_sort_method(sort_method)
        
    def set_sort_method(self, sort_method):
        self.sort_method = sort_method
        
    def begin_sort(self):
        self.sort_method.sort()         # <--- 하부 클래스가 바뀌더라도, 동일한 코드 활용 가능토록 인터페이스화

class SelectionSort:
    def sort(self):
        print('selection sort')
        pass

selection_sort = SelectionSort()
sorting3 = SortManager(selection_sort)
sorting3.begin_sort()
```
