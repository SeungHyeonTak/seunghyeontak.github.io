---
layout: post
title:  "JavaScript 객체지향프로그래밍"
comments: true
description: "javascript"
author: SeungHyeon Tak
date:   2020-01-05 23:30:00 +0700
categories: [javascript]
tags: [javascript]
keywords: "javaScript 객체지향"
---
#### JavaScript 객체지향프로그래밍

간단하게 객체지향기법에 대해 알고 가자

일단 간단하게 코드를 보며 내용을 짚고 넘어가자

```javascript
const grades = {
    'list': {'a': 1, 'b':2, 'c':3},
    'show': function(){
	for(name in this.list){
	    Console.log(name + ':' + this.name + '<br />');
	}
    }
};

grades['list']['a'];  // 1
grades['show'] // show에서 적용된 function 호출
```

grades라는 하나의 객체안에 list와 show라는 데이터가 있다.

위에 보이는 코드처럼 서로 관련있는 데이터들끼리 묶어놓은걸 객체지향프로그래밍이라고 한다.
