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

(작성중 ..)
print(Man);
pring(Woman);
```
