---
layout: post
title:  "JavaScript apply"
comments: true
description: "javascript apply"
author: SeungHyeon Tak
date:   2020-01-16 00:25:00 +0700
categories: [javascript]
tags: [javascript]
keywords: "javaScript apply"
---
## JavaScript apply

우리가 javascript에서 함수를 호출 하는 방법은

```javascript
function func(){

}
func();
```

이렇게 호출 할 수 있다.

그리고 javascript를 학습하면서 객체는 속성을 가지고 있다고 배웠다.

속성에 값이 저장되어있으면 그대로 `속성`이라 부르고,

함수가 들어있으면 `메소드`라 부른다.

위의 예제 코드를 봤을때, func()함수를 정의 하고 메서드를 접근 시키려 한다면

```
func.apply();
func.call();
```

여기서 apply와 call이라는 메소드가 없는데 어디서 가져왔냐면 자바스크립트가 지원해주는 메소드라고 생각하면 된다.

저 두개말고도 여러가지가 있는데,

오늘은 `apply`에 대해서만 알아볼 예정이다.

그냥 apply를 사용할땐 `func.apply(null, [1,2,3])`이런식으로 사용된다.

쉽게 말하면 func(1,2)와 func.apply(null, [1,2,3])랑 같다는 소리다.

그럼 이걸 왜쓰는가? 라고 생각할 수 있지만 `apply`에 대한 진정한 의미를 알아보자

```javascript
o1 = {var1:1, var2:2, var3:3}
o2 = {v1:10, v2:50, v3:100, v4:25}

function sum(){
    var _sum = 0;
    for(name in this){
	_sum += this[name];
    }
    return _sum;
}
sum.apply(o1);
sum.apply(o2);
```

여기서 sum이라는 함수안에 for문에 this라는것이 보인다.

지금은 this에 대해 심각하게 생각할필요는 없고 추후에 또 보면 되니깐 

지금 여기서는 apply에 들어간 객체를 가져올때 쓰이는 구문으로 보면 된다.

쉽게 말하면 **암시적**으로 `var this = o1`으로 선언된다고 생각하면된다.

apply에서만 this가 쓰이는것이 아닌 현재 이 맥락에서 쓰일때(호출될때) 정해진다고 보면 된다.
