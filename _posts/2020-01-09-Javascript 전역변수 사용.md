---
layout: post
title:  "JavaScript 전역변수 사용하기"
comments: true
description: "javascript"
author: SeungHyeon Tak
date:   2020-01-09 00:55:00 +0700
categories: [javascript]
tags: [javascript]
keywords: "javaScript 전역변수 사용하기"
---
## JavaScript 전역변수 사용

전역변수를 꼭 사용하기 원한다면 하나의 객체를 `전역변수`로 만들고, 객체의 속성으로 변수를 관리하는 방법을 사용한다.

예를 들어서 코드를 써볼 수 있다.

```javscript
var MYAPP = {} // 하나의 객체를 전역변수로 만들어준다.

MYAPP.calculator = {
    'left': null,
    'right': null
}
MYAPP.coordinate = {
    'left': null,
    'right': null
}

MYAPP.calculator.left = 10; // 이러한 속성들을 변수로 관리하여 사용 할 수 있다.
MYAPP.calculator.right = 20;

function sum(){
    return MYAPP.calculator.left + MYAPP.calculator.right;
}
document.write(sum());
```
