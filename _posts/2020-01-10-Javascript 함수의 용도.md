---
layout: post
title:  "JavaScript 함수의 용도"
comments: true
description: "javascript"
author: SeungHyeon Tak
date:   2020-01-10 00:04:00 +0700
categories: [javascript]
tags: [javascript]
keywords: "javaScript 함수의 용도"
---
## JavaScript 함수의 용도

javascript에서는 함수도 객체이다. 즉, 일종의 값이라고 생각하면 된다.

여기서 값이라고 하면 변수처럼 `var a = 'value'`처럼 변수에 담을 수 있는 값을 생각하면 된다.

하지만, 거의 모든 언어가 함수를 가지고 있다.

javascript의 함수가 다른 언어의 함수와 다른점은 `함수가 값이 될 수 있다는 점이다.`

말로는 이해 하기 힘들수 있으므로 코드로 설명 하겠다.

```javascript
function a() {}  ==> var a = function() {} // 이렇게 표현 될 수 있다
```

위에서 본 예제는 javascript에서 함수를 선언할 때 볼 수 있는 형식이다. 전혀 새로운 코드가 아니다.

또한! 함수는 객체의 값으로 포함 될 수 있다.

이렇게 객체의 속성으로 담겨진 함수를 메소드(method)라고 부른다.

```javascript
a = {
    b: function() {
	
    }
}
```

위에서 b는 `key`값이라 부를 수 있다.(값을 저장할 수 있다.)

그리고 b가 담고 있는 function은 `value`를 나타낸다.

이것을 하나로 a가 묶고 있는데 이렇게 표현한것을 `객체`라 부른다.

이걸 통해 우리가 알아야 할 부분이 두가지 있다.

1. 객체 안에서 변수의 역할을 하는것을 속성(property)
2. 객체 안에 함수가 담겨 있다면 메소드(method)

이렇게 두가지를 숙지 하고 있으면 된다.

그리고 자바스크립트에서 함수는 값이기 때문에 다른 함수의 인자로 전달 받을 수 있다.

예제로 살펴 본다.

```javascript
function cal(func, num){
	return func(num);
}
function increase(num){
	return num+1;
}
function decrease(num){
	return num-1;
}

alert(cal(increase,1));
alert(cal(decrease,1));
```

밑의 alert를 보면 함수가 다른 함수를 인자로 받아들인걸 볼 수 있다.
