---
layout: post
title:  "Django server 구동 테스트"
comments: true
description: "Django runserver"
author: SeungHyeon Tak
date:   2019-04-22 11:52:12 +0700
categories: [Django]
tags: [Django]
keywords: "Django runserver 해보기"
---
#### Django Server 구동하기
*Django server 구동 테스트*

Django까지 설치 완료 후 작업을 시행해 보겠습니다.
먼저 만든 프로젝트로 갑니다.(저는 config라는 프로젝트를 생성했습니다.)

```
# 프로젝트 폴더 안에 있는`settings.py`로 가서 제일 밑 부분으로 가면
LANGUAGE_CODE = 'ko-kr'
TIME_ZONE = 'Asia/Seoul'
```

이렇게 변경 시켜준다음
pycharm 터미널에서

```
$ python3 manage.py runserver
```

를 실행해주면 <http://127.0.01:8000/> 이라는 주소가 나올껀데
이 주소를 실행 해보면
![server](https://user-images.githubusercontent.com/46446165/57067357-b62c4a80-6d09-11e9-8ee5-851ecedeb35e.png)
이러한 화면이 나오면 구동 성공 한것입니다.
