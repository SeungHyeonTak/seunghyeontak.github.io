---
layout: post
title:  "Pycharm 다운"
comments: true
description: "Python pycharm 다운하기"
author: SeungHyeon Tak
date:   2019-03-05 21:06:23 +0700
categories: [python]
tags: [python]
keywords: "pycharm 다운"
---
## Pycharm 다운

### Window
1. 공식 홈페이지 접속
<https://www.jetbrains.com/pycharm/download/>
접속하면 2개의 선택지가 나옵니다.
   * 유료인 Professional
   * 무료인 Community
당연히 무료인 Community를 받습니다.
2. 다운받는 파일을 실행하면 계속 Next를 누릅니다.
   * 설치 경로파일을 수정하고 싶으면 수정합니다.
3. 세번째 설치화면이 나오면
   * 32-bit launcher 체크 / Create associathions 부분의 .py를 체크한 후 Next
4. 그리고 마지막에 Run pycharm Community Edition을 체크 하고 Finish를 누른 후 빠져나옵니다.
5. Complete Installation이라는 화면이 나오면 Do not import settings를 체크 후 ok합니다.
6. 다음은 에디터에 배경색과 글자색을 설정 할 수 있는 부분이다.(원하는걸 설정하고 Ok)
7. Pycharm은 프로젝트 단위로 소스파일을 관리합니다.
   * Create New Project를 누릅니다.
   * 그리고 원하는 프로젝트 이름을 적고 Create 해준다.
8. window pycharm 설치 끝

*****

### Linux
1. 일단 시작은 window와 똑같이 설치 홈페이지에서 Community를 설치 해준다.
2. 압축을 풀어준다.
3. 터미널에서 Pycharm의 bin 폴더로 이동하여
<br>
```
$ sh pycharm.sh
```
<br>
4. 이후 설치화면이 뜨면 모든 설정을 default값으로 두고 설치를 진행
   * 혹시 JDK관련 오류가 발생하면 jdk와 jre를 설치 해준다.
   * $ sudo apt-get install default-jdk
   * $ sudo apt-get install default-jre

5. 이후 터미널에서 pycharm의 bin경로에서 `$ ./pycharm.sh`로 실행 가능하다.

이상 Linux의 설치도 알아보았다.
