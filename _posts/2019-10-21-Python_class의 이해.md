---
layout: post
title:  "python class"
comments: true
description: "Python, class, namespace, instance"
author: SeungHyeon Tak
date:   2019-10-21 22:06:00 +0700
categories: [python]
tags: [python]
keywords: "python"
---
#### class에 대한 이해

> 선언 하기

```python
# class 선언하기
class UserInfo: # <- 클래스명의 첫글자는 대문자 그리고 단어와단어가 붙을시에도 첫글자에 대문자로 표기해준다.
    # class안에 구현해줘야할 메서드 def 함수
    def __init__(self): # 생성자 : 현재 class를 초기화해준다.
	pass
```

> 예제로 보기

```python
class UserInfo:
    def __init__(self, name):
	self.name = name
    def user_info_p(self): # method라 부름
	print("Name : ", self.name)

user1 = UserInfo("철수") # 인스턴스를 만들어 줌
# 인스턴스명 = 클래스()
user1.user_info_p() # 객체.메서드 형식으로 호출
# 객체.메서드
user2 = UserInfo("영희")
user2.user_info_p()

# user1과 user2는 각자 독립적이다.
# id / __dict__ 를 확인해 볼 수 있다.
print(id(user1))
print(user1.__dict__)
```

*****

#### namespace

> namespace의 종류 <br>
>> * 지역 네임스페이스(nonlocal) : 함수 및 메서드별로 존재하며, 함수 내의 지역변수들의 이름들이 소속됨 <br>
>> * 전역 네임스페이스(global) : 모듈별로 존재하며, 모듈 전체에서 통용될 수 있는 이름들이 소속됨 <br>
>> * 빌트인 네임스페이스(built-in) : 기본 네장 함수 및 기본 예외 들의 이름들이 소속된다. 파이썬으로 작성된 모든 코드 범위가 포함된다. <br>

*****

#### 변수의 스코프
> 변수의 스코프란 변수의 이름으로 그 변수가 가리키는 엔티티를 찾을 수 있는 영역의 범위를 말한다. 이는 현재 위치에서 액세스할 수 있는 네임스페이스들과 그 순서에 의해 결정 된다.

*****

#### self의 의미
> self는 객체의 인스턴스 그 자체를 의미함, 대부분 객체지향 언어는 메서드에 안보이게 전달 하지만 파이썬 클래스의 메서드를 정의할 때는 self를 꼭 명시해야하고 그 메서드를 불러올 때 self는 자동으로 전달된다.


* `__init__`

> __init__은 파이썬에서 쓰이는 생성자이다. <br>

```python
class A:
    def __init__(self):
	self.x = 'Hello'
    
    def method_a(self, foo):
	print(self.x + ' ' + foo)

a = A()
```

> 위의 코드에서 A()는 생성자 __init__에 어떤 파라미터도 넘기지 않고, A 객체를 생성하여 이를 반환 받음 <br>
> 만약 A(24, 'Hello')와 같이 쓰면 파라미터 2개를 받는 생성자가 필요한데 현재 __init__은 그 어떤 파라미터도 받지 않으니 excetion이 발생한다.

