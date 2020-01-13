---
layout: post
title:  "JavaScript 클로저"
comments: true
description: "javascript 클로저"
author: SeungHyeon Tak
date:   2020-01-14 00:42:00 +0700
categories: [javascript]
tags: [javascript]
keywords: "javaScript 클로저"
---
## JavaScript 클로저

클로저란?

내부함수가 외부함수의 맥락에 접근할 수 있는것을 가르킬 수 있는것을 클로저라 한다.

그러면 내부함수와 외부함수가 뭔지 알아야 한다. 코드로 예시를 들며 알아 볼 것이다.

```javascript
function outter(){ // 외부 함수
    function inner(){ // 내부 함수
	var title = 'coding everybody';
	alert(title);
    }
    inner();
}
outter();
```

이렇게 사용하는 이유는 어떠한 함수안에서만 사용되는 함수가 있는데,

그 함수를 바깥에 선언하면 응집성이 떨어지기 때문이다.

그래서 함수안에 함수가 또 있으면 즉, 내부함수가 포함되어 있으면 외부 함수에서 선언하여 

해당 외부함수를 호출 하는것이 가독성도 높아지고 함수를 불러오기도 편해지기 때문이다.

또 한가지 이제 클로저의 특징중 하나를 코드로 예시 들어볼 수 있다.

```javascript
function outter(){
    var title = 'coding everybody'; // 외부함수에 선언된 지역변수
    function inner(){ // 내부 함수
	alert(title); // 외부 함수의 지역변수에 접근 가능!!
    }
    inner();
}
outter();
```

위 코드에서 의문점은 어떻게 외부 함수의 지역변수를 내부함수가 불러올 수 있냐이다.

클로저의 특징 하나가 내부함수에서 외부함수의 지역변수에 접근 할 수 있다는 점이다.

그리고 이번에는 좀 어려울 수 있는데,

일단 코드를 보면서 이해하자

```javascript
function outter(){
    var title = 'coding everybody';
    return function (){
	alert(title);
    }
}

inner = outter();
inner();
```

여기서 참고 해야 할 점 return을 한 함수는 한번 사용하면 죽어버린다.

그러므로 `inner = outter()`에서 `outter()`를 호출했을때 내부함수의 수명은 다했다고 봐야한다.

마지막에 `inner()`를 호출할때 title은 내부함수에 존재 하는 값이 아니고 외부함수에 존재하는 값이다라고만 기억해 놓자! 더 정확한 내용은 좀 더 공부해봐야 할 것같다.

즉, 클로저는 내부함수를 가지고 있는 외부함수에 접근할 수 있을뿐만아니라 외부함수가 종료된 이후에도 내부함수를 통해 접근 할 수 있다. 이것이 클로저의 특징이다.
