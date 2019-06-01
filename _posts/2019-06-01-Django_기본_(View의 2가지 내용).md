---
layout: post
title:  "Django_기본.(Django에서 View란?)"
comments: true
description: "Django"
author: SeungHyeon Tak
date:   2019-06-01 14:13:54 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.기본 웹개발
<br>
* Django에서 View는 2가지로 나뉘어진다.
<br>
* class형 view (클래스형 뷰)
  -Forms : 입력폼 생성, 입력값 유효성 검사 및 DB로의 저장
  -테스팅
  -국제화&지역화
  -캐싱
  -Geographic : DB의 Geo 기능 활용 (PostgreSQL 중심)
  -Sending Emails
  -Sysdication Feeds (RSS/Atom)
  -Sitemaps - 검색 엔지 최소화

* def형 view (함수형 뷰)
  - HTTP 요청 처리
  -Models : 데이터베이스와의 인터페이스
  -Templates : 복잡한 문자열 조합을 보다 용이하게 주로 HTML문자열 조합 목적으로 사용하지만, 푸쉬 메세지나 이메일 내용을 만들 때에도 쓰면 편리
  -Admin 기초 : 심플한 데이터베이스 레코드 관리 UI
  -Logging : 다양한 경로로 메세지 로깅
  -Static files : 개발 목적으로 정정인 파일 관리
  -Messages framework : 유저에게 1회성 메세지 노출 목적

