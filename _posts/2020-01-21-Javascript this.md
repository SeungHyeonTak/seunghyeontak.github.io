---
layout: post
title:  "JavaScript this"
comments: true
description: "javascript this"
author: SeungHyeon Tak
date:   2020-01-21 00:58:00 +0700
categories: [javascript]
tags: [javascript]
keywords: "javaScript this"
---
## JavaScript this

this는 함수내에서 함수호출의 맥락을 의미한다고 이전부터 계속 들어왔다.

this라는것은 javascript함수안에서사용할 수 있는 변수 이다.

함수를 어떻게 호출하느냐에 따라 this의 쓰임이 달라진다고 볼 수 있다.

코드로 살펴 보면

```javascript
function func(){
    if(window == this) {
	document.write("window === this");
    }
}
func();
```

위의 코드를 봤을때 `func()`를 호출하면서 func 함수가 실행 되는데, 그 안에 조건문으로 window와 this가 같다는 조건이 걸려있다.

그냥 무심코 func()를 실행시켜보면 결과는 window===this가 나올것이다.

이로써 알수 있는 정보는 this와 window가 같다는 정보를 알 수 있다. (물론 함수에서 아무 파라미터를 받지 않는다는 가정하에)

#### 메소드의 호출

객체의 소속인 메소드(속성인데 함수로 표현된것)의 this는 그 객체를 가르킨다.

```javascript
var o = {
    func: function() {
	if (o === this) {
	    document.write("o===this");
	}
    }
}
o.func(); // func은 o(객체)의 소속이다.
// 객체안에서 메소드로 표현되었으며 여기서 this는 o를 가리키고 있다.
```

#### 생성자와 this

```javascript
var funcThis = null;

function Func() {
    funcThis = this;
}

var o1 = Func();
if (funcThis === window){
    document.write('window');
}

var o2 = new Func();
if (funcThis === o2) {
    document.write('o2')
}
```

위의 코드를 하나씩 보자면 `함수`로 호출된것과 `생성자`로 호출된것이 있다.

여기서 각자의 조건은 다 타겠지만 어떤식으로 진행 되는지는 자세히 알 필요가 있다.

o1같은 경우는 위에서 계속 봐왔던 예제인 반면에 생성자로 진행된 예제는 여기서 처음 볼것이다.

생성자로 진행된 코드는 `new Func()`에 대한 `호출이 모두 끝난 다음에 o2라는 변수에 생성한 객체가 할당된다는 점`이다.

예를 들어

`if (o2 === this){}`라는 코드를 Func()함수안에 집어 넣게 되면 어떻게 될까?

결과는 해당 조건을 타지 않는다. 위에서 말했듯이 생성자로 진행된것은 호출이 모두 끝나야 해당 변수에 할당되기 때문이다.

저 상태에서 o2가 가진 값은 `undefind`이다.

따라서 this는 함수에 따라서 유연하게 사용될 수 있다.
