---
layout: post
title:  "Django MessageFramework"
comments: true
description: "Django MessageFramework"
author: SeungHyeon Tak
date:   2019-06-15 23:11:27 +0700
categories: [Django]
tags: [Django]
keywords: "Django MessageFramework"
---
## Django MessageFramework

#### Message Framework

* 1회성 메세지를 담는 용도
* HttpRequest 인스턴스를 통해 메세지를 남길 수 있음
* 메세지는 1회 노출이 되고, 사라짐 (새로고침하면 보여지지 않음)
ex) 저장되었습니다. / 로그인 되었습니다.

#### Message Levels를 통한 메세지 분류
* DEBUG : 디폴트 설정 상으로 메세지를 남겨도 무시 
* INFO
* SUCCESS
* WARNING
* ERROR (danger)

```python
# views.py

if form.is_valid():
	# 두가지 방법으로 쓰인다(2번째 방법 추천)
	message.add_message(request, message.INFO, '새 글이 등록되었습니다.') # 비추
	message.info(reqeust, '새 글이 등록되었습니다.') # 추천
```

<br>
* html파일 제일 상위단에 적용
<br>
![asdasd1231](https://user-images.githubusercontent.com/46446165/59553327-75914380-8fce-11e9-8b11-eb631f75b9ff.png)
