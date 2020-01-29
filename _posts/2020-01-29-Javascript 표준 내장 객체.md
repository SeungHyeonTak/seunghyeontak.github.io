---
layout: post
title:  "JavaScript prototype"
comments: true
description: "javascript prototype"
author: SeungHyeon Tak
date:   2020-01-23 00:55:00 +0700
categories: [javascript]
tags: [javascript]
keywords: "javaScript prototype"
---
## JavaScript prototype

### 상속이란?

상속은 객체의 로직을 그대로 물려받는 또 다른 객체를 만들 수 있는 기능을 의미한다.

javascript에서 상속은 중요한 개념중에 하나이므로 꼭 알고 있어야 한다.

일반적으로 변수와 메소드가 모아져있는 객체가 있고 새로운 객체가 하나 있는데 여기서 새로운 객체가 변수와 메소드가 모아져있는 객체의 내용을 쓸 수 있는것이 상속이다.

하지만, 여기서 의문점이 드는점은 '그러면 원래 있던 객체만 쓰면 되지 왜 상속을 하냐'라는 생각을 가질 수 있다.

의문점에 대한 해답을 말하자면,

상속받은 객체가 부모객체의 어느 기능을 제외하고 또 어떠한 기능을 추가해서 자식객체의 맥락에 맞게 부모 객체의 로직을 재활용해서 사용가능하게 할 수 있는것이 상속의 방식이다.

### prototype

prototype은 javascript에서 사용하는 상속의 방법이다.

일단 사용하는법을 예제로 써보겠다.

```javascript
function Person(name){
    this.name = name;
}
Person.prototype.name = null;
Person.prototype.introduce = function(){
    return 'My name is ' + this.name;
}

function Programmer(name){
    this.name = name;
}
Programmer.prototype = new Person();

var p1 = new Programmer('tsh');
document.write(p1.introduce + '<br />');
```

해당 코드를 보면 `Programmer`라는 객체에서는 `introduce`라는 메소드가 없는걸 알 수 있다.

하지만 Programmer.prototype을 보면 Person을 new로 인해 만들어진 생성자함수가 있다.

이 뜻은 Programmer가 Person을 상속하겠다! 라는 뜻이라고만 알고 있으면 된다.

이렇게 해서 부모에서 쓰이는 값들을 자식이 상속해서 쓸 수 있는 예제를 보았다.

좀 더 응용해서 다른 예제를 볼 수 있다.

```javascript
// 부모
function Person(name){
	this.name = name;
}

Person.prototype.name = null;
Person.prototype.introduce = function(){ // 부모가 가진 값
	return 'My name is ' + this.name;
}

// 자식
function Programmer(name){
	this.name = name;
}
Programmer.prototype = new Person();
Programmer.prototype.coding = function(){ // 부모에겐 없고 자식만 가진 값
	return "hello world";
}

// 자식
function Designer(name){
	this.name = name;
}

Designer.prototype = new Person();
Designer.prototype.design = function(){  // 부모에겐 없고 자식만 가진 값
	return 'beautiful';
}

var p1 = new Programmer('tsh');
document.write(p1.introduce() + '<br />'); //Person에 정의 되어있는 introduce에서 사용된다.
document.write(p1.coding() + '<br />');

var p2 = new Designer('hoj');
document.write(p2.introduce() + '<br />');
document.write(p2.design() + '<br />');
```

위 예제도 물론 상속 관련 코드인데 이번 코드는 부모에게 받은 값만이 아닌 자식이 가진 값을 쓰는 코드도 같이 작성해 보았다.

두개의 자식이 아무리 하나의 부모객체를 상속 받는다 하더라도 각각의 객체에서도 차별화된 값을 만들어 사용할 수 있다.

이렇게 객체지향적인 언어들은 상속 부분이 큰 비중을 차지 하고 있는걸 알 수 있다. 여러므로 유용하게 쓰여서 코드를 깔끔하게 해주기 때문이다.

위의 예제처럼 바로 위의 객체가 최상위 객체이면 일반적으로 `prototype`이라고 할 수 있고

상속을 몇차례씩 받아서 내려오게 되면 `prototype chain`이라고 불리운다.

최상위 부모 객체 <- 자식 객체 <- 자식 객체 .... 이런식으로 말이다. (계속 연결된 prototype)
