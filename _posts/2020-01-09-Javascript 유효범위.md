---
layout: post
title:  "JavaScript 유효범위"
comments: true
description: "javascript"
author: SeungHyeon Tak
date:   2020-01-09 01:17:00 +0700
categories: [javascript]
tags: [javascript]
keywords: "javaScript 유효범위"
---
## JavaScript 유효범위

### javascript 유효범위의 대상

자바스크립트는 함수에 대한 유효범위만을 제공한다.

예를 들어서 Java, C 등등 이러한 언어들은 for문안에서 선언한 변수에 대해서는 따로 호출할 수 없다.

만약 호출한다고 하면 존재하지 않는 변수를 호출 했다고 하며 에러를 띄울 것이다.

예를 들면

```java
for (int i=0;i<5;i++){
    String str = 'aa';
}

System.out.println(str); // 이러한 경우에는 for문안에서 선언된 지역변수를 가져올 수 없다.
```

하지만 자바스크립트에서는 함수의 {}(중괄호)안에서 선언된것들만이 지역변수의 특성을 가지고

for, if 등 이러한 곳에서 쓰이는 {}(중괄호)에서는 지역변수로서의 의미를 갖지 않는다.

```javascript
for (var i=0;i<5;i++){
    var str = 'aaaa';
}

alert(str);
```

### javascript의 정적 유효범위

일단 코드를 보며 이해하는게 더 낫다고 생각하여 코드로 설명 하겠다.


```javascript
var i = 5;

function a(){ 
    var i=10; // 2. 호출된 상태에서 제일 처음 i라는 변수에 10이라는 값을 담았다.
    b(); // 3. b함수를 호출
}

function b(){
    document.write(i); // 4. 호출 받고 i를 출력한다. (여기서 i는 뭐가 호출 될까???)
}

a(); // 1. 일단 a를 호출한다.
```

위의 예제에 대한 b에서 호출되는 i의 값은 `5`이다.

왜 그런지 잘 모르겠다면 일단 이 말을 머리에 새겨두고 이해해보자

* 사용될때 호출되는것이 아니다. `정의될때 호출된다`.

a에서는 b를 호출하기만 했다. 그러므로 a에서 선언된 i랑은 아무 상관이 없다. 그래서 b에서 사용될 i 변수는 전역변수 i=5가 호출 된다.

위의 예제에서 봤듯이 이렇게 사용되는게 `정적 유효범위`이다.
