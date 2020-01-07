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


#### 정규표현식 옵션 (i,g)

> i를 붙이면 대/소문자를 구분하지 않는다.

```javascript
var oi = /a/i;
var str = "Abcd";

str.match(oi); // oi는 소문자 'a'를 찾고 있지만 i를 통해서 str의 A를 찾는걸 볼 수 있다.
```

> g를 붙이면 검색된 모든 결과를 리턴한다.

```javascript
var og = /a/g;
var str = "abcdea";

str.match(og); // 결과 값으로 ["a","a"]를 리턴한다. 이유는 str에 담긴 문자열중에 a가 두번 있기 때문이다.
```

i와 g 두개다 한번에 사용하면 검색된 모든 대/소문자를 리턴하게 된다.

#### 캡처

캡처란 그룹을 지정하고 그 그룹을 가져와서 사용할 수 있는 기능을 캡처라고 하는데 이렇게 말로만 들어서는 매우 이해하기 힘들다.

정규표현식에서 사용되는 `(\w+)\s(\w+)`라는 표시가 있다.

하나씩 의미를 살표 보면
* `()` -> 해당 괄호는 정규표현식에서 그룹으로 표시된다.
* `\w` -> 문자를 의미한다. (A~Z, a~z, 0~9)까지를 해당하는 문자이다.
* `+` -> 수량자라고 불리며 해당 표시는 앞에있는 문자가 `하나이상`일때 유효하다.
  * `#`처럼 하나만 쓰여지면 사용할 수 없다.
* `\s` -> 공백이라는 의미이다. (white_space)
  * A A 사이에 띄워진 공백을 의미한다.

좀 더 알아보기 위해 예시를 보며 이해할 수 있다.

```javascript
var pattern = /(\w+)\s(\w+)/;
var str = coding everybody;

var result = str.replace(pattern, "$2, $1") // 결과 값 = everybody, coding 이렇게 자리가 뒤바껴 리턴 된다.
```

여기서 `$1`과 `$2`가 의미하는 것은 그룹을 의미한다. 

$1은 첫번째 그룹 $2는 두번째 그룹을 의미하게 된다.

이렇게 원래 있던 위치에서 그 그룹을 가져와 자리를 바꿔 넣을 수 있는 개념을 캡처 한다라고 이해 하면 될것이다.

#### 치환

치환은 해당 값을 가져와 넣어준다 `캡처`의 예시를 봤을때 `replace`안에 넣은 값들을 치환했다라고 표현한다. 이정도로 이해 하면 될것 같다.

치환도 간단하게 예시를 들어볼 수 있다.

```javascript
var urlpattern = /\b(?:http?):\/\/[a-z0-9-+&@#\/%?=~|!:,.;]*/gim;
var content = "네이버 : http://naver.com 입니다.";

var result = content.replace(urlpattern, function(url){
	return '<a href="'+url+'">' + url + '</a>';
})
```

위 예제는 content에 있는 url만을 링크로 표시되게 해주는 정규표현식이다.

정규표현식은 여러므로 쓰일 수 있으므로 잘 여러가지 방법들을 많이 찾아보면 된다.
