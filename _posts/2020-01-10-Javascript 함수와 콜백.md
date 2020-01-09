---
layout: post
title:  "JavaScript 함수의 콜백"
comments: true
description: "javascript"
author: SeungHyeon Tak
date:   2020-01-10 00:34:00 +0700
categories: [javascript]
tags: [javascript]
keywords: "javaScript 함수의 콜백"
---
## JavaScript 함수의 콜백

javascript 함수의 용도 에서 봤듯이 함수는 객체의 값이나 매개변수로 인해 함수의 인자로 쓸 수 있는걸 공부 해봤다.

이렇게 사용된 함수가 또 어떤식으로 사용 할 수 있을까?

js의 함수는 해당 함수의 `리턴`값으로도 사용할 수 있다.

```javascript
function cal(mode) {
    var funcs = {
	'plus': function(left, right){return left + right;},
	'minus': function(left, right){return left - right;},
    }
    return funcs[mode];
}

// 값 테스트 하기
alert(cal('plus')(2,1));
alert(cal('minus')(2,1));
```

함수를 다시 리턴해서 사용 할 수 있다.

그리고 `배열`의 값으로도 사용할 수 있는데, 예제를 보며 이해하자

```javascript
function process = [
    function(input) {return input + 10;},
    function(input) {return input * input;},
    function(input) {return input / 2;},
];

var input = 1;
for (var i=0;i<process.length;i++){
    input = process[i](input); // for문을 돌면서 하나하나 input의 값을 계산하여 최종 값을 alert에서 출력 시킨다.
}

alert(input);
```

이렇게 함수가 다양하게 사용되는걸 알 수 있다.

지금 까지 함수가 쓰이는곳을 정리 해보면

* 변수
* 매개변수
* 리턴값
* 배열

이렇게 쓰일 수 있는 데이터를 `first-class-citizen`이라고 부른다.

#### 콜백

콜백이란 어떠한 함수가 수신하는 인자가 함수인 경우를 `콜백 함수`라 한다.

```javascript
var numbers = [20,10,9,8,7,6,5,4,3,2,1];
numbers.sort(); // 정렬 해주는 내장 함수
/*
위의 코드를 다시 되짚어 보면 sort()라는 함수앞에 어떠한 변수거나 무언가가 있다면 이것은 객체라고 부른다.
그리고 sort()는 객체에 속해 있기 때문에 메소드라고 불린다.
*/

// 여기서 numbers.sort()를 콘솔로 찍어보면 [1,10,2,20,...] 이런식으로 나올것이다. 이유는 sort()를 하면서 js가 배열안의 값들을 문자로 인식하여 정렬 하기 때문이다.

// 그래서 해결 방법은 콜백 함수를 써서 문제를 해결 하는 것이다.

var sortfunc = function(a,b){
    console.log(a,b);
    // 여기서 첫번째 방법
    if (a>b){
	return 1;
    } else if (a<b){
	return -1;
    } else{
	return 0;
    }

    // 두번째 방법
    return a-b;
}

alert(numbers.sort(sortfunc));
// 여기서 sort()라는 함수가 sortfunc라는 함수를 인자로 뒀기때문에 콜백이 된다.
```

첫번째 방법에서 a를 기준으로 두 수의 크기를 비교해서 a가 크면 양수를 반환 하고 a가 더 작으면 음수를 반환하게 되는 로직이다.

예를 들어 a,b에 2,3이 왔을 경우 음수가 반환되면서 a가 더 작다는 걸 알 수 있다.

그래서 [a,b,...] 순으로 정렬 하게 된다.

결론적으로 콜백함수는 값으로서 함수를 사용 할 수 있다.(자바스크립트의 함수가 값이기 때문에 콜백이 가능하다!!!)





