---
layout: post
title:  "JavaScript 정규표현식"
comments: true
description: "javascript"
author: SeungHyeon Tak
date:   2020-01-07 23:12:00 +0700
categories: [javascript]
tags: [javascript]
keywords: "javaScript 정규표현식"
---
## JavaScript 정규표현식

#### 정규표현식이란?

문자열에서 특정한 문자를 찾아내는 도구이다.

정규표현식을 사용하는 수십 줄에 해당하는 코드를 간략하게 몇 줄로 처리할 수 있다.

#### 컴파일 만들기

간단하게 패턴을 만들어서 찾는 방법이다.

```javascript
// 기본으로 문자열을 담기
var pattern = "a";

// 정규표현식 리터럴
var pattern = /a/;

// 정규표현식 객체 생성자
var pattern = new RegExp("a");
```

#### 정규표현식 메소드 실행

위에서 정규표현식을 컴파일해서 객체를 만들었다면 이제 문자열에서 원하는 문자를 찾아내야 한다.

> RegExp.exec();
> RegExp --> 패턴을 하나 정의 했다고 보면됨

```javascript
var pattern = /a/;

pattern.exec('abcd'); // 결과 값 = ["a"]
```

**.exec()**는 필요한 정보를 추출하는것이다.

```javascript
// 예를 몇개 더 들어보면

var pattern = /a./;

pattern.exec("abcde"); // 결과 값 = ["ab"]

pattern.exec("jasnkdjfn"); // 결과 값 = null
```

여기서 ab가 나온 이유는 pattern에서 a뒤에 `.`이 의미하는것이 a문자 뒤에 어떠힌 문자이던 하나의 문자를 출력 한다는 의미를 가지고 있다.

그리고 두번째는 찾는 값이 없어서 null을 반환하는것이다.

```javascript
var pattern = /a/;

pattern.test('abcd'); // True
pattern.test('jasjnfas'); // False
```

**.test()**
위의 예시는 우리가 찾고자 하는 정보의 존재 유무가 있는지 없는지 체크해준다.

