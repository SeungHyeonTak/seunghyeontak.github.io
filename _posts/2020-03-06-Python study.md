---
layout: post
title:  "python study"
comments: true
description: "Python study"
author: SeungHyeon Tak
date:   2020-03-06 22:52:00 +0700
categories: [python]
tags: [python]
keywords: "python study"
---
## python study

파이썬을 공부하면서 궁금한 내용들 기록하기

### decorator는 언제 사용되나?

함수를 빠르게 변경할 때 사용 가능하다.

**참고)**

메인구문이 있고, 여기에 부가적인 구문을 추가하고 싶을때가 있는데, 

부가적인 구문을 반복해서 사용하고 싶은 경우도 있다.

이때 부가적인 작업을 decorator로 선언해서 자유롭게 사용 가능

### 리스트와 튜플의 주된 차이점은 무엇인가?

리스트는 `mutable`하고 튜플은 `immutable`하다.

**참고)**

- mutable은 값이 변한다는 뜻이다.
  - List
  - Dict
- immutable은 값이 변하지 않는다는 의미이다.
  - String
  - Int
  - Tuple

### 파이썬에서 메모리는 어떻게 관리되나요?

파이썬에서는 개별적인 힙을 사용해서 메모리를 유지한다.

따라서 힙은 모든 파이썬 객체와 자료구조를 가지고 있다.

이 영역은 파이썬 인터프리터만 접근 가능하며 프로그래머는 사용할 수 없다.
  
  
### Lambda와 Def의 주된 차이점은 무엇인가?

Def는 다중 표현을 갖는다. 반면에 lamdba는 단일 함수이다.

Def는 함수를 생성하고 이름을 지정하여 나중에 다시 호출한다.

Lambda는 함수 객체를 구성하고 반환한다.

Def는 선언을 반환할 수 있지만, Lambda는 할 수 없다.

### Docstring은 무엇인가?

Docstring은 파이썬의 모듈, 함수, 클래스, 메서드의 정의에 처음 부분에 선언되는 유니크한 텍스트 이다.

ex)

```python
class CustomClass:
"""
클래스의 문서화 내용을 입력한다.
"""
	def custom_function(param):
		'''
		함수의 문서화 내용을 입력한다.
		'''
		pass
```

위의 예시 처럼 첫번째줄 바로 아랫줄에 큰따옴표(") 3개나 작은따옴표(') 3개로 작성하면 된다.

### Function call 혹은 A callable Object는 무엇인가?

파이썬에서 함수는 호출가능한 객체로 취급된다.

### Call-by-value는 무엇인가?

표현식 또는 값이 함수의 각 변수에 바인딩되는지 여부에 대한 인수이다.

파이썬은 그 변수를 함수 수준 범위에서 로컬로 취급한다.

해당변수에 대한 모든 변경사항은 로컬로 유지되며 함수 외부에 반영되지 않는다.

### 파이썬에서 call-by-reference는 무엇인가?

call by reference와 pass by reference는 서로 바꿔 사용할 수 있다.

참고로 인수를 전달하면 간단한 복사가 아닌 함수에 대한 암시적 참조로 사용할 수 있다.

이 경우 인수에 대한 모든 수정 사항도 호출자에게 표시된다.

### 파이썬은 call-by-value인가, call-by-reference인가?

결론은 둘다 아니다.

어떤 값을 전달하느냐에 따라 달라지는 것이다.

파이썬의 자료형엔 크게 immutable(불변)과 mutable(가변)이 있다.

int, str같은 타입이 immutable이고 list, dict같은 타입이 mutable이다.

불변 타입의 객체를 넘기면 call by value가 되고 가변 타입의 객체를 넘기면 call by reference가 된다.

즉, 할당 되는것에 따라 전달 방식이 달라지는 것이다.

### *Args는 무엇을 하는 기능인가?

N개의 매개변수를 넘기겠다는 표시

### *Kwargs는 무엇을 하는 기능인가?

N개의 변수의 파라미터를 넘기겠다는 것인데 이때 이름 혹은 키워드를 가지고 있습니다.

### 파이썬에서 Main() 메소드가 있나요?

파이썬은 인터프리터 언어이기 떄문에 일반적으로는 code by code로 실행된다.

현재 모듈 이름을 확인하는 방식으로 main함수를 사용할 수 있다.

### GIL이란 무엇인가?

글로벌 인터프리터 락으로 파이썬 객체가 다중 스레드가 동시에 실행되지 않도록 하는 것이다.

### 파이썬에서 쓰레드의 안정성은?

파이썬에서 쓰레드에 대해 안정성있는 접근을 보장한다.

이를 위해서 GIL(global interpreter lock)을 사용한다.

만약 쓰레드가 GIL lock을 잃었을 경우에 쓰레드로부터 코드를 안정성 있게 만들어야 한다.

### 파이썬에서는 어떻게 메모리 관리를 하나?

파이썬에서는 내부적으로 힙 매니저가 구현되어 있다.

힙 매니저는 모든 오브젝트와 자료구조를 가지고 있다.

힙 매니저는 오브젝트에 대한 힙 스페이스의 할당과 할당 해제를 한다.

### 파이썬에서 튜플은 무엇인가?

튜플은 collection데이터 타입의 하나로 immutable한 자료형이다.

리스트와 비슷한 구조를 갖지만 수정이 가능하냐 안하냐의 차이를 가지고 있다. (변경 불가)

### 파이썬에서 딕셔너리는 무엇인가?

딕셔너리는 collection데이터 타입의 하나로서 key, value의 구조로 이뤄진 데이터 타입이다.

해쉬, 맵 혹은 해쉬맵이라고도 불린다.

### 파이썬에서 Set은 무엇인가?

Set은 순서가 없는 (unordered)한 collection 데이터 객체의 하나로 유니크한 원소들만 갖고 있다.

### 파이썬 딕셔너리는 어떻게 사용되나?

딕셔너리 타입은 하나의 그룹(key)과 다른 그룹(value)을 매핑하기 위해서 사용될 수 있다.

### 파이썬의 리스트는 연결리스트(linked list)인가?

파이썬의 리스트는 가변길이 배열로 C스타일의 연결리스트와 다르다.

내부적으로 다른 객체를 참조하기 위한 연속적인 배열을 가지며, 

배열 변수에 대한 포인터와 그 길이를 목록 헤드 구조에 저장한다.

### 파이썬에서 클래스는 무엇인가?

파이썬은 객체지향 프로그래밍을 지원하며 거의 모든 OOP특징을 사용할 수 있도록 제공한다.

파이썬의 클래스는 객체를 만들기 위한것이다. 

멤버 변수를 정의하고 그 변수들 사이의 행동을 가진다.

### 파이썬의 클래스에서 Attributes와 Methods는 무엇인가?

기능을 정의하지 않은 클래스는 쓸모가 없다. 속성을 추가하므로 이 문제를 해결 할 수 있는데, 

속성은 데이터와 기능을 위한 컨테이너 역할을 한다.

또한, Attribute를 직접 class 내부에 추가 할 수도 있다.

```python
class Human:
	profession = 'programmer' # Attribute
	def set_profession(self, new_profession): # 이 기능을 Method라 부른다.
		self.profession = new_profession
man = Human()
print(man.profession)
```

### 런타임에 클래스 속성에 값을 할당하는 방법은 무엇인가?

클래스에 init메소드를 추가하고 최초에 변수를 입력해준다.

```python
class Human:
	def __init__(self, profession):
		self.profession = profession
	def set_profession(self, new_profession):
		self.profession = new_profession
		
man = Human('Manager')
print(man.profession)
```

### 파이썬에서 상속은 무엇인가?

상속이란 OOP에서 객체가 부모 클래스의 특징에 접근할 수 있는것을 말한다.

### 파이썬에서 Composition은 무엇인가?

composition은 파이썬에서 상속의 한 종류이다.

기본 클래스에서 상속을 하지만 파생 클래스의 멤버 역할을 하는 기본 클래스의 인스턴스 변수를 사용한다.

쉽게 말해서 다른 클래스의 일부 기능을 그대로 이용하고 싶으나, 전체 기능 상속은 피하고 싶을때 사용

composition or aggrefation이라고도 한다.

상속 관계가 복잡할 경우 코드 이해가 어려운 경우가 많다.

컴포지셩은 명시적 선언, 상속은 암시적 선언

### 파이썬에서 Erros와 Exceptions는 무엇인가?

Erros는 코딩 이슈로서 프로그램이 비정상적으로 종료되게 되는것이다.

Exceptions는 외부적인 이벤트로 인해 발생하는 것으로 인터럽트 같은 경우가 있다.

### 파이썬의 Iterators는 무엇인가?

Iteraters는 배열과 같은 객체로서 다음 구성 성분으로 이동하는 것이 가능하다.

next() 메서드로 데이터를 순차적으로 호출 가능하다.

### iterators와 itrable의 차이는 무엇인가?

리스트, 튜플, 딕셔너리 및 세ㅌㅌ와 같은 컬렉션 유형은 모두 반복 가능한 객체 인 반면 

순회하는 동안 반복자를 반환하는 반복 가능한 컨테이너이기도 한다.

iterable은 하나씩 차례로 반환 가능한 객체이다. __iter__, __getitem__ 메서드로 정의된 class들이 iterable하다고 할 수 있다.

iterable을 iterator로 변환하고 싶다면, iter()라는 빌트인 함수를 사용하면 변환 가능하다.

### iterable과 iterator의 각각의 의미

- iterable 객체 - `반복 가능한 객체`
- 대표적으로 iterable한 타입 - list, dict, set, str, bytes, tuple, range

- iterator 객체 - `값을 차례대로 꺼낼 수 있는 객체이다.`
- iterator는 iterable한 객체를 내장함수 또는 iterable객체의 메소드로 객체를 생성할 수 있습니다.

### 파이썬 generator는 무엇인가?

generators는 iterator를 생성하는 함수이다. yield 키워드를 사용하여 반환 한다.

yield는 return과 달리 반환 후에 종료되지 않고 그 상태를 유지한다.

generator를 사용할 경우의 이점으로는 memory를 효율적으로 사용할 수 있다는 것이다.


### 파이썬의 Enumerate는 무엇인가?

iterators와 함께 사용 가능한 것으로 iterations의 횟수를 셀 수 있다.
