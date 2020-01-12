---
layout: post
title:  "JavaScript 비동기"
comments: true
description: "javascript"
author: SeungHyeon Tak
date:   2020-01-11 22:38:00 +0700
categories: [javascript]
tags: [javascript]
keywords: "javaScript 비동기"
---
## JavaScript 비동기

자바스크립트 비동기처리에는 Ajax가 있다. 그리고 콜백을써서 유용하게 사용될 수 도 있다.

일단, Ajax의 뜻은 Asynchronous Javascript and XML의 풀네임으로 Asynchronous가 비동기라는 뜻을 가지고 있다.

우리는 웹페이지에서 비동기 처리가 진행되는 과정을 알 수 있는데, 그 방법은 개발자도구를 켜서 Network 부분에서 비동기 처리되는 부분을 클릭하면 Ajax관련하여 내용이 뜨는걸 확인 할 수 있다.

그러면 여기서 동기적, 비동기적의 뜻은 뭔지 궁금할 수 있다.

동기적 : 서버에서 웹페이지를 전송해줄때까지 그대로 기다리고 있어야하는것을 동기적이라 한다.(정보가 올때까지 아무것도 못한다.)

비동기적 : 서버와 웹페이지가 내부적으로 통신을 진행하고 있으므로 사용자는 다른 일들을 할 수 있는 부분을 비동기적이라 한다. (실제로 다른페이지로 넘어가는건 안되고 해당 페이지에서 발생하는 간단한 이벤트들에 대해서만 가능하다.)

그리고 우리는 자바스크립트를 사용하면서 jQuery라는 특수한 객체를 사용 할 수 있는데 jQuery를 사용할땐 `$`기호를 항상 붙이고 사용해야 한다.

(작성중..)
