---
layout: post
title:  "JavaScript 모듈"
comments: true
description: "javascript"
author: SeungHyeon Tak
date:   2020-01-06 22:38:00 +0700
categories: [javascript]
tags: [javascript]
keywords: "javaScript 모듈"
---
## JavaScript 모듈

#### 모듈이란?

프로그램은 작고 단순한것에서 크고 복잡한것으로 진화한다.

재활용성을 높이고 유지보수를 쉽게 할 수 있는 다양한 기법이다.

크고 복잡한 로직이 있더라도 이것을 잘 분류하여 묶어 놓고 분리해서 필요할때 가져다 쓰는것을 간단히 말해 모듈이라 한다.

코드를 동작하는 방법에 따라 여러개로 분리한다.

- 자주 사용되는 코드를 별도의 파일로 만들어서 필요할때마다 재활용
- 코드 수정시에 필요한 로직을 빠르게 찾을 수 있다.
- 필요한 로직만을 로드해서 메모리의 낭비를 줄일 수 있다.

하지만!

순수 자바스크립트에서는 모듈이라는 개념이 분명하게 존재하지 않는다.

자바스크립트가 구동되는 환경을 `호스트`환경이라 한다.

#### 모듈화 직접 해보기

말로만 해서는 무슨 말인지 이해하기 힘들다.

그래서 간단한 내용으로 모듈화를 어떻게 쓰는지 알아보자.

```javascript
// demo1.html
<!DOCTYPE html>
<html>
<head>
	<title></title>
	<script type="text/javascript" src="greeting.js"></script>
	// 밑의 js파일에서 실행될 소스코드를 가져온다는 뜻이다.
</head>
<body>
	<script type="text/javascript">
		alert(welcome()) // js파일에서 가져온 welcome 함수를 실행시킨다.
	</script>
</body>
</html>
```

```javascript
// greeting.js
function welcome(){
	return 'Hello world';
}
```


