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

(작성중...)
