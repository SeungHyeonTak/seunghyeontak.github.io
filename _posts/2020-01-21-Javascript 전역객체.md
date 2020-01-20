---
layout: post
title:  "JavaScript 전역객체"
comments: true
description: "javascript 전역객체"
author: SeungHyeon Tak
date:   2020-01-21 00:07:00 +0700
categories: [javascript]
tags: [javascript]
keywords: "javaScript 전역객체"
---
## JavaScript 전역객체

전역객체(Global object)는 특수한 객체이다. 모든 객체는 이 전역 객체의 프로퍼티라고 한다.

코드로 이해하자면

```javascript
function func(){
    alert('Hi');
}

func(); // 일반적으로 우리가 쓰는 방법
window.func(); // 전역객체를 사용하여 쓰는 방법
```

이렇게 두가지로 쓸 수 있는데, 우리는 함수앞에 '.'이 있고 그 앞에 무언가가 있다면 그것에 대해 의미 하는것을 `객체`라고한다고 알고 있다.

하지만 `func()`만 써도 돌아가는걸 우리는 지금까지 잘 써왔다.

그럼 `window`는 왜 쓰는걸까? 

여기서 `window`는 전역객체라고 불린다. 즉, 암시적으로 생략가능하다고 알고 있으면 된다.(생략 했을때 기본적으로 사용되기 때문이다.)

그냥 이러한 것들이 있다고만 알아두면 된다.

**참고**

같은 javascript라고 전역객체 전부 window를 쓰는것은 아니다.

node.js는 `global`을 쓰기때문에 다른걸 쓸땐 그나마 알아두는것이 좋다. 
