---
layout: post
title:  "python replace"
comments: true
description: "Python replace"
author: SeungHyeon Tak
date:   2020-02-12 17:25:00 +0700
categories: [python]
tags: [python]
keywords: "python replace"
---
## python replace

### 문자열에서 특정 문자 제거 하고 싶을때 사용

전부 다 다룰수 없기때문에

**" "(큰따옴표)를 예를 들어 다뤄 보겠다.**

String 객체의 따옴표 문자는 replace 메소드를 호출하여 제거된다. 

이 메소드는 문자를 입력 및 문자에서 제거하여 대체합니다.

다룰 예제는 기존 str형을 쓸때 ' '안에다 쓰지만 그 안에다 " "또 쓸 수 있는건 모두 알것이다. " "을 제거 하기 위해 예제를 작성해본다.

```python
date = '"안녕하세요"'
date.replace('"', "") 
# 간단하다 replace앞의 parameta에는 제거 하고 싶은 문자, 
# 두번째 parameta에는 제거한 문자 대신 넣고 싶은거 없다면 그냥 예제처럼 두면된다.

# 이렇게 되면 출력은 큰따옴표 없는 '안녕하세요' 가 출력 될것이다.
```
