---
layout: post
title:  "JavaScript 전역변수와 지역변수"
comments: true
description: "javascript"
author: SeungHyeon Tak
date:   2020-01-08 23:46:00 +0700
categories: [javascript]
tags: [javascript]
keywords: "javaScript 전역변수와 지역변수"
---
## JavaScript 전역변수 / 지역변수

전역 변수 : 함수 바깥에 선언 되는 함수로 어디서든 쓰일 수 있다.
지역 변수 : 함수 안에 선언 되는 함수로 그 함수가 호출 될때만 사용된다.

```javascript
var vscope = 'global'; // 전역변수
function fscope(){
    var vscope = 'local'; // 지역변수
    alert(vscope);
}

fscope(); // 이 상태에서 호출하게 되면 지역변수가 가진 `local`이 호출된다.
// 하지만 함수안에 var이 선언되어있지 않으면 `golbal`이 호출된다.
```

※ 참고

전역변수를 사용할 명확한 이유가 없으면 지역변수를 사용하는걸 권장한다.

이유는 전역변수로 선언했다가 전역변수로 선언된 값이 다른 함수들 안에서 어떻게 변경되어 함수가 호출될지 모르니깐 안전하게 지역변수로 다루는게 더 효율적이다.
