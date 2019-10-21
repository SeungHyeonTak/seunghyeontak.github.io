---
layout: post
title:  "JavaScript 객체"
comments: true
description: "JavaScript"
author: SeungHyeon Tak
date:   2019-10-15 23:30:00 +0700
categories: [JavaScript]
tags: [JavaScript]
keywords: "JavaScript"
---
#### JavaScript 객체

#### 객체
> 객체는 우리가 변수 혹은 상수를 사용하게 될 때 하나의 이름에 여러 종류의 값을 넣을 수 있게 해줌 <br>

```javascript
const dog = {
	name : '멍멍이',
	age : 2
};

console.log(dog.name);
console.log(dog.age);
```

> 객체를 선언 할 떄에는 `{}` 문자 안에 원하는 값들을 넣어주면 된다. <br>
> `키 : 원하는 값` 파이썬에서 dict 형태와 같다. <br>.
> 키 부분에는 공백이 없어야 한다. 혹시 공백이 필요한 상황이라면 따옴표로 처리 할 수 있다. <br>

* `함수에서 객체를 파라미터로 받기`

```javascript
const Man = {
	name : '철수',
	age : '10'
};

const Woman = {
	name : '영희',
	age : '10'
};

function print(human) {
	const text = '이름은 ${human.name} 이고, 나이는 ${human.age} 살 입니다.';
	console.log(text);
}

print(Man);
pring(Woman);
```

* `객체 비구조화 할당`

> 객체구조 분해 라고 불리기도 함

```javascript
const ironMan = {
  name: '토니 스타크',
  actor: '로버트 다우니 주니어',
  alias: '아이언맨'
};

const captainAmerica = {
  name: '스티븐 로저스',
  actor: '크리스 에반스',
  alias: '캡틴 아메리카'
};

function print(hero) {
  const { alias, name, actor } = hero;
  const text = `${alias}(${name}) 역할을 맡은 배우는 ${actor} 입니다.`;
  console.log(text);
}

print(ironMan);
print(captainAmerica);
```

> 파라미터 단계에서 객체 비구조화 할당을 할 수 있음

```javascript
const ironMan = {
  name: '토니 스타크',
  actor: '로버트 다우니 주니어',
  alias: '아이언맨'
};

const captainAmerica = {
  name: '스티븐 로저스',
  actor: '크리스 에반스',
  alias: '캡틴 아메리카'
};

function print({ alias, name, actor }) {
  const text = `${alias}(${name}) 역할을 맡은 배우는 ${actor} 입니다.`;
  console.log(text);
}

print(ironMan);
print(captainAmerica);
```

* 객체안에 함수 넣기
> 이런식으로 객체안에 function을 넣을 수 있다.

```javascript
const dog = {
  name: '멍멍이',
  sound: '멍멍!',
  say: function say() {
    console.log(this.sound);
    // 함수가 객체안에 들어가가 되면 this는 자신이 속해있는 객체를 가르키게 된다.
  }
};

dog.say(); // 함수를 선언 할 때 이름이 없어도 된다.
```

> 객체 안에 함수를 넣을 때, `화살표 함수`로 선언한다면 제대로 작동하지 않는다.

```javascript
// 안좋은 예

const dog = {
    name : '멍멍이',
    sound : '멍멍!!',
    say: () => { // 이런식으로 하면 안됨
	console.log(this.sound);
    }
};
```

> 이유 -> function으로 선언한 함수는 this가 제대로 자신이 속한 객체를 가르키게 되는데, 화살표 함수는 그렇지 않기 때문
<br>
* Getter 함수와 Setter 함수

> 객체 안에 Getter 함수와 Setter 함수를 설정하는 방법 알아보기 <br>
> 객체를 만들고 나면, 다음과 같이 객체안의 값을 수정 할 수 있음

```javascript
const numbers = {
    a: 1,
    b: 2
};

numbers.a = 5;
console.log(numbers);
```

<br>

```javascript
const numbers = {
    a: 1,
    b: 2,
    get sum() {
	console.log("sum 함수가 실행된다.");
	return this.a + this.b;
    }
};

console.log(number.sum);
numbers.b = 5;
console.log(number.sum);
```

