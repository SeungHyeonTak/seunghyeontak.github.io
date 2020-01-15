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

(작성중...)
