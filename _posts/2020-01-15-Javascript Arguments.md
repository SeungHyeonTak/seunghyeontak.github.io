---
layout: post
title:  "JavaScript Arguments"
comments: true
description: "javascript Arguments"
author: SeungHyeon Tak
date:   2020-01-15 00:46:00 +0700
categories: [javascript]
tags: [javascript]
keywords: "javaScript Arguments"
---
## JavaScript Arguments


### Arguments!!

Arguments는 무엇인가?

프로그래밍을 조금이라도 접해봤다면 알 수 있는 단어이다.

기본적으로 함수의 매개변수가 있는데, 무수히 많은 매개변수를 일일히 다 넣어줄 순 없는 노릇이다.

그럴때! 우리가 편하게 쓰기위해 사용하는것이 Arguments이다.

일단 말로라도 조금 이해가 되었다면 예제 코드로 접근해보자

```javascript
function sum(){
    var _sum = 0;
    for (var i=0;i<arguments.length;i++){
	document.write(i + ' : ' + arguments[i] + '<br />');
	_sum += arguments[i];
    }
    return _sum;
}
document.write('result :' + sum(1,2,3,4));
```

코드를 보며 이해 하자면 맨 밑의 `document.write('result :' + sum(1,2,3,4));` 부분에서 sum부분에서는 `1,2,3,4`라는 매개변수를 가지고 있다.

하지만 함수로 선언된 sum부분에서는 아무 매개변수값을 받아오지 않게 보여진다.

하지만 `arguments`라는 단어가 for문에서 보일것이다. 물론 오직 for문에서만 도는게 아니고 해당 매개변수를 받아온 함수 안에서 사용가능한데,

arguments는 javascript와 약속되어 있는 특수한 변수여서 그렇게 불러와지는것이다.

직접 테스트 해보고 이해하는것이 더 빨리이해될 수 도 있다.

### 매개변수의 수

매개변수와 관련된 두가지 수가 있다.

1. 함수.length
2. arguments.length

이렇게 두가지 있는데, 다른점이 뭘까?

다른 점을 말하자면 첫번째 함수로 전달된 방법은 `실제 인자의 수`를 의미하고 있고,

두번째 arguments로 전달된 방법은 함수에 정의된 `인자의 수를 의미` 한다.

잘 이해가 되지 않는다면 예제 코드로 이해 할 수 있다.

```javascript
function one(arg1){
    console.log(
	'one.length', one.length, 
	// 이 부분은 함수.length 이다. 밑에 1,2,3이라고 매개변수를 받아왔지만 함수는 매개변수 하나만 있다고 판단하여 아무리 많이 써도 1이라고 표시 될것이다.
	'arguments.length', arguments.length 
	// arguments는 위에서도 말했듯이 아무리 많이 받아온 매개변수라도 이 하나의 변수로 다 포함 할 수 있다.
    );
}

one(1,2,3);
```

근데 매개변수의 수가 뭐가 중요하지?

이렇게 매개변수를 사용함에 있어서 함수를 다르게 쓰게될때 문제가 발생할 수 있다.

예를 들어 `난 함수안에서 매개변수의 수가 5개가 들어와야 일을 할꺼야` 라고 로직이 구성되있는데, 함수.length를 쓴다면 백날해봤자 로직을 타지 않을것이다.

이런 문제로 인해 더욱더 좋은 코드를 짤 수 있는 유용한 방법들이 있다.
