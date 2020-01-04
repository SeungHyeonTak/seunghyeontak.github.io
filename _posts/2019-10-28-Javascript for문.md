---
layout: post
title:  "JavaScript 반복문"
comments: true
description: "javascript"
author: SeungHyeon Tak
date:   2019-10-28 21:03:00 +0700
categories: [javascript]
tags: [javascript]
keywords: "javascript"
---
#### JavaScript 반복문
<br>

> for문의 기본적인 코드 <br>

```javascript
for (let i=0;i<10;i++){
    console.log(i);
}

// for (조기구문; 조건구문; 변화구문) 으로 사용한다.
```

*****

#### 배열과 for

> 배열과 for문을 함께 활용하기 <br>

```javascript
// 이런식으로 사용할 수 있다.
const names = ['객체1', '객체2', '객체3'];

for (let i=0;i<names.lenght;i++) {
    console.log(names[i]);
}
```

*****

#### while문

> while문은 특정 조건이 참이라면 계속해서 반복하는 반복문이다. <br>
> for문은 특정 숫자를 가지고 숫자의 값을 비교하고, 증감해주면서 반복한다면, while문은 조건을 확인만 하면서 반복한다. (조건문 내부에서 변화를 직접 주어야 한다.) <br>

```javascript
let i = 0;
while (i < 10){
    console.log(i);
    i++;
}

// while문을 사용할 땐 조건문이 언젠가 false가 되도록 신경써야한다. 언젠간 false로 전환이 되지 않는다면 반복문이 끝나지 않고 영원히 반복되기 때문이다.
```

<br>

*****

#### for...of

> `for..of`문은 배열에 관한 반복문을 돌리기 위해서 만들어진 반복문이다. <br>
> 보통 배열의 내장함수를 많이 사용하기 때문에 사용할 일이 거의 없다고 보면 된다. <br>

*****

#### for...in

> 객체를 위한 반복문을 알아보기 전에 객체의 정보를 배열 형태로 받아올 수 있는 함수 몇가지를 볼 수 있다. <br>

```javascript
const doggy = {
    name: '멍멍이',
    sound: '멍멍',
    age: 2
};

console.log(Object.entries(doggy));
console.log(Object.keys(doggy));
console.log(Object.values(doggy));
```
<br>

* `for...in` 구문을 사용

```javascript
const objects = {
    name: '객체1',
    age: '객체2',
    phone: '객체3'
};

for (let key in objects) {
    console.log('${key}: ${objects[key]}');
}
```

<br>

*****

#### break 와 continue

> 반복문 안에서는 `break`와 `continue`를 통해 반복문에서 벗어나거나, 그 다음 루프를 돌게끔 할 수 있다. <br>

```javascript
for (let i = 0; i<10; i++) {
    if (i == 2) continue; // 다음 루프 실행
    console.log(i);
    if (i == 5) break; // 반복문 끝내기
}
```
<br>

*****

#### 퀴즈

> `숫자로 이루어진 배열이 주어졌을때, 해당 숫자 배열안에 들어있는 숫자 중 3보다 큰 숫자로만 이루어진 배열을 새로 만들어서 반환해보세요.`

```javascript
function biggerThanThree(numbers) {
    // 내용 구현하기
}

const numbers = [1,2,3,4,5,6,7];
console.log(biggerThanThree(numbers));
```

<br>
