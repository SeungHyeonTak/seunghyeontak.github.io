---
layout: post
title:  "(etc). Ubuntu MySQL 삭제"
comments: true
description: "MySQL"
author: SeungHyeon Tak
date:   2019-10-09 21:59:20 +0700
categories: [etc]
tags: [etc]
keywords: "MySQL"
---
#### etc. MySQL 삭제하기

> Mysql 설치 시에 패키지 에러가 나거나 중간에 잘못건든부분에 의해 정상적인 구동이 되지 ㅇ낳을 경우,
> 해결책을 모르겠다면, Mysql을 삭제하고 재설치를 하고싶을때가 있다.
> Ubuntu Linux 환경에서 Mysql 삭제에 대한 내용

> 삭제

```text
$ sudo /etc/init.d/mysql stop

$ sudo apt-get remove --purge mysql-server mysql-client

$ sudo netstat -tap | grep mysql

$ sudo apt-get remove --purge mysql-server*

$ sudo apt-get remove --purge mysql-client*
```
