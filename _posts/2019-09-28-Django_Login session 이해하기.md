---
layout: post
title:  "Django_Login session 이해하기"
comments: true
description: "Django Login session"
author: SeungHyeon Tak
date:   2019-09-28 15:54:13 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django Login Session


![loginsession](https://user-images.githubusercontent.com/46446165/65812832-2501c700-e208-11e9-80a6-1e6dd8e6b8ed.png)

#### 설명
> client에서 server측으로 로그인 요청을 DB에 저장이 되고 client의 server쪽에서 Cookie값이 저장이 된다.
> 이후 Cookie를 지우지 않고 그대로 놔두면 다음번에 요청을 보냈을때 이미 저장되있는 client값이 확인된다.(다시 저장되지 않음)



