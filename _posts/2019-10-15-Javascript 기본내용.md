---
layout: post
title:  "JavaScript 기본내용"
comments: true
description: "javascript"
author: SeungHyeon Tak
date:   2019-10-15 23:19:00 +0700
categories: [javascript]
tags: [javascript]
keywords: "javascript"
---
## JavaScript

* 에디터 - Sublime Text
  * Sublime Text에서 side bar 줄 넣는 방법 (view / sidebar / Hide side bar)

#### 숫자와 문자

> 생활코딩 javascript 참고

경고창(alert)으로 확인해보기

```
<!DOCTYPE html>
<html>
<head>
	<title></title>
</head>
<body>
<script type="text/javascript">
	alert(1+1);
	// 이런식으로 정수, 실수 연산된 값 경고창으로 띄울 수 있음
</script>
</body>
</html>
```

> 수의 연산

```
Math.pow(3,2); // 3의2승으로 제곱을 나타낸다.
Math.round(10.6); // 반올림을 나타낸다.
Math.ceil(11.4); // 해당 수의 바로 위의 수를 나타낸다. 결과 : 12 
Math.floor(10.2); // ceil과 반대(가까운 수 중 그 아래 수) 결과 : 10
Math.sqrt(9); // 제곱근을 나타낸다.
Math.random(); // 랜덤한 수 출력
```

> 문자의 표시

문자는 "abcd", 'abcd' 이렇게 큰따옴표와 작은따옴표를 구분지어 쓴다.

apple's 라는 문자열을 쓰고 싶을때 "apple\'s" 이렇게 표현 시킬 수 있다.

이때 \(역슬래시)를 escape라고 부른다.

typeof - 해당 타입이 문자열인지 숫자인지 표시해준다.

/n - 줄바꿈을 나타내준다.

"abcd".length - 문자열의 갯수를 나타낸다.

"abcd".indexof('c') - 해당 문자열이 몇번째 있는지 찾아준다.

웹에서 페이지 검사(F12)를 눌러 Console 창에서 직접 간단하게 실행 해볼 수도 있다.

![console](https://user-images.githubusercontent.com/46446165/71642192-f3a70480-2cea-11ea-85cc-cecb527246c3.png)


#### 변수와 상수

> 변수 : 바뀔수 있는 값을 말함(한번 값을 선언하고 나서 바꿀 수 있음) <br>
> 상수 : 한번 선언하면 값을 바꿀 수 없다. 고정적이다.<br>

```javascript
// 변수
let value = 1; // 변수를 선언할때는 let이라는 키워드를 쓴다.
// 한번 선언했으면 똑같은 이름으로 선언하지 못한다.

// 상수
const a = 1; // 상수를 선언할때는 const라는 키워드를 쓴다.
// a로 선언한 값은 변경 불가

// 참/거짓(Boolean)
let good = true; // 참
let loading = false; // 거짓

// null / underfined
// null은 말 그대로 없다라는 의미를 가짐
const friend = null;

// underfined는 값이 아직 설정되지 않았다는 의미를 가짐
let criminal;
console.log(criminal);
```

#### 연산자

> 연산자는 프로그래밍 언어에서 특정 연산을 하도록 하는 문자이다. <br>

* `산술 연산자`
  * '+' : 덧셈
  * '-' : 뺄셈
  * '*' : 곱셈
  * '/'  : 나눗셈

> 기본적으로 알고있는 산술연산자 4가지가 있다. <br>
> 그리고 파이썬에서는 적용이 안되던 증감연산자를 사용 할 수 있다.

```javascript
let a = 1;
a++;
++a;
console.log(a);
```

#### 대입 연산자
> 특정값에 연산 한 값을 바로 설정할때 사용하는 연산자이다.

```javascript
let a = 1;
a = a + 3; // 이렇게 쓴 코드를
a += 3; // 이렇게 쓸 수 있다.
```

#### 논리 연산자
> 논리 연산자는 Boolean 타입을 위한 연산자이므로 true / false를 위한 연산자 이다.

* ! : NOT (NOT 연산자는 true -> false // false -> true)
* && : AND (양쪽의 값이 둘 다 true 일떄만 결과물이 true입니다.)
* || : OR (양쪽의 값 중 하나라도 true라면 결과물이 true입니다. 혹은 둘다 false일때만 false !!)

> 연산의 순서<br>
> 사칙 연산을 할 때 곱셈, 나눗셈이 먼저이고 이후 덧셈, 뺄셈인것 처럼<br>
> 논리 연산자도 순서가 있다. NOT(!) -> AND(&&) -> OR(||)<br>

```javascript
const value = !((true && false) || (true && false) !false);
```

#### 비교 연산자
> 말 그대로 비교 할 때 사용한다.

```javascript
const a = 1;
const b = 1;
const equals_true = a === b; // 일치할때
const equals_false = a !== b; // 일치하지않을때
```

> `=`를 3개 사용하여 비교한다. (2개도 가능) <br>
> underfined와 null도 같은 값으로 간주 <br>

**※ 참고**

==(equal operator)는 동등연산자이고 ===(strict operator)는 일치연산자라고 나타낸다.

==는 데이터 타입이 다르더라도 내용은 일치하므로 True를 주는 반면에

===는 데이터 타입조차 비교하게 되므로 정확한 판별을 할 수 있다.

```
alert(1 == "1") // True
alert(1 === "1") // False
```

되도록이면 **===**를 쓰자

#### 조건문

> if 문 <br>

```javascript
const a = 1;
if (a + 1 === 2){
	console.log('a + 1 이 2입니다.');
} else if (a + 1 === 3) {
	console.log('a + 1 이 3입니다.');
} else {
	console.log('원하는 숫자가 아닙니다.');
}
```

> switch/case문 <br>

```javascript
const device = 'iphone';

switch (device) {
  case 'iphone':
    console.log('아이폰!');
    break;
  case 'ipad':
    console.log('아이패드!');
    break;
  case 'galaxy note':
    console.log('갤럭시 노트!');
    break;
  default:
    console.log('모르겠네요..');
}
```

#### 함수

**함수를 쓰는 이유**

* 재사용성
* 유지보수의 용이
* 가독성

위 3가지가 큰 역할을 하기에 함수를 쓰는것이 좋다.

> 특정 코드를 하나의 명령으로 실행 할 수 있게 해주는 기능 <br>
> 특정 값의 합을 구하는 식을 함수로 만들기 <br>

```javascript
function add(a,b) {
	return a + b;
	// console.log('호출 되지 않음!');
}
// 함수를 만들때 function 키워드를 사용
// 이후 함수에서 어떤 값을 받아올지 정해주는데 이를 파라미터라 한다.
// 함수 내부에서 return 키워드를 사용하여 함수의 결과물을 지정 할 수 있다.
// 참고 return 이후의 코드는 실행되지 않음!
const sum = add(1, 2);
console.log(sum);

// 함수는 여러가지로 표현할 수 있다.
// 함수를 add라는 변수에 대입하여 쓸 수 있다.
add = function(a,b){
	return a + b;
}

//익명함수
//이름이 필요없고 바로 실행되는 함수

(function(){
	return '바로출력';
})();

```

#### 화살표 함수
> 함수를 선언하는 방식 중 또 다른 방법은 화살표 함수 문법을 사용하는 것 <br>
> function 키워드 대신에 `=>` 문자를 사용해서 함수를 구현할 수 있다. <br>
> return 없이 줄여 쓸 수 있다.

```javascript
const add = (a,b) => {
	return a + b;
};
console.log(add(1,2));

// or 

const add = (a,b) => a+b;
console.log(add(1,2));

// 두가지 방법으로 구현 가능하다.
```


#### 알아두면 쓸모 있는 명령어

```javascript
document.write('안녕'); // 웹페이지로 출력되게 해주는 명령어
```
