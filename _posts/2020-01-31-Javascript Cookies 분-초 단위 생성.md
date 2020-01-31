---
layout: post
title:  "JavaScript Cookies"
comments: true
description: "javascript Cookies"
author: SeungHyeon Tak
date:   2020-01-31 12:11:00 +0700
categories: [javascript]
tags: [javascript]
keywords: "javaScript Cookies"
---
## JavaScript Cookies 초 / 분 단위 설정


사용하게된 이유

쿠키로 인해 새로바뀐 intro 페이지로 다시 돌아갈 수 없었음.

로직을 참고하니, 기본적인 로직은 

```javscript
Cookies.set('cookies_name', 'cookies_value', {expires: 3})
```
이렇게 되어있으며 쿠키가 3일동안 저장되어있는 코드로 작성되어 있다.

만약, expires에 '1'이런식으로 입력하게 되면 해당 쿠키가 하루동안 유지된다고 한다.

**참고**

> 쿠키는 기본적으로 만료일을 일(하루)단위로 처리하지만 초, 분 단위로도 쿠키 만료일을 설정 할 수 있습니다

하지만 현재 우리가 사용하고 있는 서비스는 intro 이미지를 보여줌으로써 고객들에게 소개될 예정으로 main 사이트로 들어갈때 생성된 Cookies값은 1분으로 설정 하도록 설정 하였다.

쿠키를 초 / 분 단위로 생성하게 하려면

```javascript
var date = new Date();
date.setTime(date.getTime() + 1*1000); // 1초
date.setTime(date.getTime() + 1*60*1000); // 1분
Cookies.set('cookies_name', 'cookies_value', {expires:date})
```

위의 코드 처럼 초, 분 단위로 얼마든지 변경 가능하다.

관련사이트 >> [[jQuery] 쿠키 초, 분 단위 생성](https://m.blog.naver.com/PostView.nhn?blogId=vucket&logNo=220892207607&proxyReferer=https%3A%2F%2Fwww.google.com%2F)


