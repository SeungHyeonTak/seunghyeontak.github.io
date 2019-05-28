---
layout: post
title:  "Git flow 협업하기"
comments: true
description: "Git"
author: SeungHyeon Tak
date:   2019-05-28 19:46:11 +0700
categories: [Github]
tags: [Github]
keywords: "Git"
---
<br>
##### git flow 협업하기

* git-flow란?
  - git-flow는 전략적으로 관리하는 Git을 쉽고 편하게 도와주는 도구이다.
<br>
* 구조와 흐름
  - 가장중심이 되는 브런치는 master / develop 브런치이다.(무조건 있어야 한다.)
  - 왠만해선 이름을 변경하지 않고 진행 해야함
  - merge된 feature, release, hotfix 브런치(사용한 브런치)는 삭제하도록한다.
<br>
* Feature 브런치
  - 브런치 나오는곳 : develop
  - 브런치가 들어가는 곳 : develop
  - 이름 지정 : master, develop, release, hotfix를 제외한 어떤것이든 가능!
<br>
* 사용법

```
$ git flow init / (git flow -d init)
$ git flow feature start branch-name
# 개발 완료후
$ git flow feature finish branch-name
# 원격 저장소 반영
$ git push origin develop
```
<br>

이후 작업이 완료되면
github 페이지에서 pull request를 하며 master(즉, PM)이 코드를 보고 허락을 해주면 git 저장소에 업로드가 완료된것을 알 수 있습니다.
<br>

업로드 되어있는 파일들을 내려받으려면

```
$ git pull "git-site"
```

를 실행하면 최신화된 파일을 받을 수 있다.
