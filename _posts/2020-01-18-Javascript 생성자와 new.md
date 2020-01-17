---
layout: post
title:  "JavaScript 생성자와 new"
comments: true
description: "javascript 생성자와 new"
author: SeungHyeon Tak
date:   2020-01-18 00:46:00 +0700
categories: [javascript]
tags: [javascript]
keywords: "javaScript 생성자와 new"
---
## JavaScript 생성자와 new

앞서 본 내용을 알기전에 알아야 할것들을 다시 한번 정리하고 가자.

객체란 무엇인가?

객체란 서로 연관된 변수와 함수를 그룹이라고 한다.

객체 내의 변수를 프로퍼티(property)와 메서드(method)라고 부른다.

```javascript
var person = {} // 비어있는 객체

person.name = 'oop'; // 객체 내의 변수이니 프로퍼티
person.introduce = function(){ // 객체내의 함수이니 메소드
    return 'My name is' + this.name; // 여기서 this는 현재 함수가 속해있는 객체를 가르키는것이다. 그러므로 person 객체를 this라고 할 수 있다.
}
document.write(person.introduce());
```

여기서 보면 객체와 변수들이 떨어져 있는걸 볼 수 있는데, 만약 이렇게 쓰이고 객체와 변수 사이에 무수히 많은 코드들이 들어간다면 코드를 읽는 사람의 집중도는 현저하게 떨어질 수 있다.

그러면 위의 예제 말고 다르게 바꿔 보자

```javascript
var person = {
    'name': 'oop',
    'introduce': function(){
	return 'My name is' + this.name;
    }
}
```

분명 이렇게 쓰면 가독성이 좋아진걸 느낄 수 있다.

하지만 위의 예제들중 뭐가 틀리고 뭐가 맞다고 할수는 없다. 각자 상황에 따라 쓰이는것이기 때문에 잘 생각해서 사용하면 나을것이다.

#### new

생성자는 객체를 만드는 역할을 하는 함수라고 한다.

```javascript
function Person(){
    var p = new Person(); // 객체의 생성자
    p.name = 'oop'l
    p.introduce = function(){
	return 'My name is' + this.name;
    }
}
document.write(p.introduce());
```

여기서 `var p = new Person()`에 대해서 알아 볼껀데, 크롬 개발자도구에서 console에 `function Person(){}`을 입력하고 `var p = new Person()`다음에 `p`에 담겨있는 내용을 확인해보면
`Person {}`이 나올꺼다. 

우리는 여기서 한가지 알 수 있다. 자바스크립트에서 `{}`가 들어가면 객체인걸 알 수 있으므로

함수앞에 `new`를 붙이게 되면 함수를 객체로 만들어주는걸 알 수 있다.

#### 생성자

우리가 앞서 new에 대해서 알아봤다. 그럼 이제 생성자에 대해서 알아봐야 하는데, 

일단, 생성자가 하는일은 객체에 대한 내용을 초기화 한다고 생각하면 된다.

```javascript
function Person(name){
    this.name = name;
    this.introduce = function(){
	return 'My name' + this.name;
    }
}

var p1 = new Person('oop');
document.write(p1.introduce());

var p2 = new Person('poo');
document.write(p2.introduce());
```

일단 간단하게 코드를 짜봤는데,

이렇게 봐서는 이해가 안될 수도 있다.

일단 `Person`이라는 함수를 생성했고, 밑에 new를 통해서 Person이라는 객체를 만들어 준다.

그리고 생성된 객체를 통해 함수 호출이 되는데, 함수안에 `this`라는 단어가 보일 것이다.

여기서 this를 붙임으로 인해서 new로 인해 새롭게 태어난 여러개의 Person 객체들이 호출 될때마다 this로 인해 값이 초기화 된다.

즉, p1과 p2를 보면 둘다 다른 name을 파라미터로 줬는데, 서로 가진 파라미터로 호출되는걸 알 수 있다.

그래서 `생성자가 하는일이 객체에 대한 내용을 초기화 한다고 할 수 있다.`

영어로 쉽게 `init / initialize`라고 부른다.
