---
layout: post
title:  "JavaScript 프로토타입과 클래스"
comments: true
description: "JavaScript"
author: SeungHyeon Tak
date:   2019-10-28 22:03:00 +0700
categories: [JavaScript]
tags: [JavaScript]
keywords: "JavaScript"
---
#### JavaScript 프로토타입과 클래스
<br>

#### 객체 생성자

> 객체 생성자라는것은 함수를 통해서 새로운 객체를 만들고 그 안에 넣고 싶은 값 혹은 함수들을 구현 할 수 있게 해준다. <br>

```javascript
function Animal(type, name, sound) {
    this.type = type;
    this.name = name;
    this.sound = sound;
    this.say = function() {
	console.log(this.sound);
    };
}

const dog = new Animal('개', '멍멍이', '멍멍');
const cat = new Animal('고양이', '야옹이', '야옹');

dog.say();
cat.say();
```

> 객체 생성자를 사용할 때는 보통 함수의 이름을 `대문자`로 시작하고, 새로운 객체를 만들때에는 `new`키워드를 앞에 붙여주어야 한다. <br>

> 같은 객체 생성자 함수를 사용하는 경우, 특정 함수 또는 값을 재사용 할 수 있는데, 그것이 프로토 타입이다. <br>

*****

#### 프로토타입

* 프로토타입은 무엇인가?
> 자바스크립트는 클래스라는 개념이 없다. 그래서 기존의 객체를 복사하여 새로운 객체를 생성하는 프로토타입 기반의 언어이다.(class 대신 prototype) <br>
> 프로토타입 기반 언어는 객체 원형인 프로토타입을 이용하여 새로운 객체를 만들어낸다.<br>
> 프로토타입은 객체를 확장하고 객체 지향적인 프로그래밍을 할 수 있게 해준다. <br>


<br>
> 프로토타입은 다음과 같이 객체 생성자 함수 아래에 `.prototype.[원하는키] = ` 이후 코드를 입력하여 설정 가능하다. <br>

```javascript
function Animal(type, name, sound) {
    this.type = type;
    this.name = name;
    this.sound = sound;
}

Animal.prototype.say = function() {
    console.log(this.sound);
};

Animal.prototype.sharedValue = 1;

const dog = new Animal('개', '멍멍이', '멍멍');
const cat = new Animal('고양이', '야옹이', '야옹');

dog.say();
cat.say();

console.log(dog.sharedValue);
console.log(cat.sharedValue);
```

<br>

#### 객체 생성자 상속받기

> `objects1`과`objects2`라는 새로운 객체 생성자를 만든다고 가정할때, 객체 생성자들에 `Result`의 기능을 재사용한다고 가정하면 이렇게 보일 수 있다. <br>

<br>
```javascript
function Result(type, name, number) {
    this.type = type;
    this.name = name;
    this.number = number;
}

Result.prototype.say = function() {
    console.log(this.number);
};

Result.prototype.say.value = 1;

function objects1(name, number) {
    Result.call(this, '객체1', name, number);
}

objects1.prototype = Result.prototype;

function objects2(name, number) {
    Result.call(this, '객체2', name, number);
}

objects2.prototype = Result.prototype;

const ob1 = new objects1('객체11', '11');
const ob2 = new objects2('객체22', '22');

ob1.say();
ob2.say();
```

#### 클래스

> 클래스라는 기능은 C++, Java, C#, PHP 등의 다른 프로그래밍 언어에는 있는 기능인데 자바스크립트에는 없었기 떄문에 예전 js에서는 위와 같이 객체생성자 함수를 사용하여 작업을 했다고 한다.<br>

> 지금 버전에서는 `class`라는 문법이 추가되었는데, 우리가 객체 생성자로 구현했던 코드를 조금 더 명확하고, 깔끔하게 구현 할 수 있게 해준다. 상속도 훨씬 쉽게 해줄 수 있다. <br>

<br>
```javascript
class Animal {
    constructor(type, name, sound) {
	this.type = type;
	this.name = name;
	this.sound = sound;
    }
    say() {
	console.log(this.sound);
    }
}

const dog = new Animal('개', '멍멍이', '멍멍');
const cat = new Animal('고양이', '야옹이', '야옹');

dog.say();
cat.say();

// 여기서 say라는 함수를 클래스 내부에 선언하였는데, 클래스 내부의 함수를 `메서드`라고 부른다. 이렇게 메서드를 만들면 자동으로 `prototype`으로 등록이 된다.
```
<br>

* `class`를 사용했을때, 다른 클래스를 쉽게 상속 가능


