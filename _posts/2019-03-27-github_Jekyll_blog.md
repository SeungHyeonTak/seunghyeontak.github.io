---
layout: post
title:  "Github-Jekyll 블로그 시작하기"
comments: true
description: "Github"
author: SeungHyeon Tak
date:   2019-03-27 13:29:03 +0700
categories: [Github]
tags: [Github]
keywords: "Jekyll"
---
#### Jekyll_Blog 만들기

*ubuntu 터미널 기준에 의해 작성된 글입니다.*

```
# 준비 단계
$ gem / $ jekyll 로 다운 되어있는지 확인
# 안되어있다면 -1번 부터 되어있다면 -2번 부터

# -1번
$ gem install bundler jekyll
# Ruby를 다운 받았으면 다 받아져있다고 함

# -2번
$ jekyll new "사용자가 저장하고자 하는 폴더"
# 터미널에서 직접 mkdir로 만들어 줘도 가능
# ex> jekyll new new_blog

# 지정폴더로 경로 이동한 다음 그 폴더 내에서 작업
$ cd new_blog

# github로 가서 새로운 Repositories를 만들고 설정해준다.
# (기본적인 설정 방법은 생략함)

# Jekyll Theme 다운 사이트에서 마음에 드는 사이트를 다운받는다.
<http://jekyllthemes.org/>

# 다운 받은 후 폴더 내에 _config.yml이라는 설정 파일에서 
# url과 baseurl을 다음과 같이 설정 해준다.
# -url : 내 깃헙 블로그 주소
# -baseurl : "" <<- 빈문자열로 둬야함(기본적으로 빈 문자열로 되어있지만 간혹 채워져있는 테마들이 있음)
# 그리고 새로 추가 해야하는 부분
# remote_theme : "테마주인의 id"/"테마 이름"
# ex) remote_theme : SeungHyeon/theme
# 그리고 이 폴더를 자신의 github에 push 작업을 진행 해주면 된다. 

```
