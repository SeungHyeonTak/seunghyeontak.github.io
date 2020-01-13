---
layout: post
title:  "JavaScript private"
comments: true
description: "javascript private"
author: SeungHyeon Tak
date:   2020-01-14 01:41:00 +0700
categories: [javascript]
tags: [javascript]
keywords: "javaScript private"
---
## JavaScript private

private가 무엇인가?

영어의 뜻은 `은밀한` 이라는 뜻을 가지고 있다.

프로그래밍에서 private는 소프트웨어가 커짐에 있어서 아무나 수정할 수 있는걸 방지 할 수 있게 해주는 녀석이다.

```javascript
function factory_movie(title){ // 여기서 매개변수 title은 지역변수의 의미를 가지고 있다.
    return { // 메소드를 2개 가진 객체
	get_title: function(){ // 메소드 (내부함수)
	    return title;
	},
	set_title: function(_title){ // 메소드 (내부함수)
	    title = _title
	}
    }
}

// ghost와 matrix는 똑같이 리턴되는 객체를 담고 있지만, 그 객체가 내부적으로 가지고 있는 메서드들의 title의 값은 다르다.
ghost = factory_movie('Ghost in the shell');
matrix = factory_movie('Matrix');

alert(ghost.get_title());
alert(matrix.get_title());

ghost.set_title('공각기동대');

alert(ghost.get_title()); // 위에서 ghost를 set title로 수정해줬기때문에 변경된 값이 나온다.
alert(matrix.get_title()); // matrix는 아무것도 건드리지 않았기때문에 그대로 나온다.
```

여기서 알 수 있는건

set_title을 통해서만 수정 할 수 있고,

get_title을 통해서만 가져올 수 있는걸 알 수 있었다.

다시한번 클로저에 대해서 알 수 있는것이 있다면

클로저라는것은 자바스크립트가 private한 변수를 사영할 수 있는 좋은 매커니즘이라는 것이다.
