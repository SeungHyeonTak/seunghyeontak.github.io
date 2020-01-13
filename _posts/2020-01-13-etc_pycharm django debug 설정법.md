---
layout: post
title:  "(etc). Pycharm django debug 설정방법"
comments: true
description: "pycharm django debug"
author: SeungHyeon Tak
date:   2020-01-13 14:22:00 +0700
categories: [etc]
tags: [etc]
keywords: "pycharm django debug"
---
## etc. Pycharm에서 django debug 설정하기


![스크린샷 2020-01-13 오후 2 25 02](https://user-images.githubusercontent.com/46446165/72234511-bb6bc400-3610-11ea-9a2c-1424187e28a6.png)

Django 우측 상단을 보면 `Add configuration..`이라는 것이 보이는데 이 부분을 누르면

![스크린샷 2020-01-13 오후 2 29 17](https://user-images.githubusercontent.com/46446165/72234635-4cdb3600-3611-11ea-935e-067b0f4810b9.png)

위와 같은 화면이 나온다. 여기서 우측 상단에 `+` 버튼을 누르고 `django server`를 누르고

![스크린샷 2020-01-13 오후 2 33 34](https://user-images.githubusercontent.com/46446165/72234741-d12db900-3611-11ea-8154-7ac526387021.png)

그 다음 name부분에 이름을 각자 작성해주고 `Environment variables`부분에서 수정해줘야 하는데, 

만약 settings.py를 분리 안하였다면 그냥 그대로 두고 분리해서 작업을 했다면 위 화면처럼 각자가 정한 

settings name을 작성해주면 된다.

![스크린샷 2020-01-13 오후 2 46 17](https://user-images.githubusercontent.com/46446165/72235083-8614a580-3613-11ea-9d01-15f190601cc5.png)

debug 설정이 완료 되었다면 처음에 `Add configuration..`라고 되어있던 부분이 dubug 설정할때 name에 적었던 이름이 나타내질 것이다.

그런 이후 중단점을 선택하고 옆에 run버튼이나 그 옆의 debug 버튼을 눌러 주면 디버깅 할 수 있다.
