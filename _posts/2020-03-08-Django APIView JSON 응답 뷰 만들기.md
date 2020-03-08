---
layout: post
title:  "Django APIView, JSON 응답 뷰 만들기"
comments: true
description: "Django APIView"
author: SeungHyeon Tak
date:   2020-03-08 23:19:00 +0700
categories: [Django]
tags: [Django]
keywords: "Django APIView"
---
## APIView, JSON 응답 뷰 만들기

### DRF의 기본 CBV인 APIView

APIView 클래스 혹은 `@api_view` 장식자

view에 여러 기본 속성을 부여합니다.

1. renderer_classes : 직렬화 class 다수
 - rest_framework.renderers.JSONRender : JSON 직렬화
 - rest_framework.renderers.TemplateHTMLRender : HTML 페이지 직렬화
2. parser_classes : 비직렬화 class 다수
 - rest_framework.parsers.JSONParser : JSON 포맷 처리
 - rest_framework.parsers.FormParser
 - rest_framework.parsers.MultiPartParser
3. authentication_classes : 인증 class 다수
 - rest_framework.authentication.SessionAuthentication : 세션에 기반한 인증
 - rest_framework.authentication.BasicAuthentication : HTTP Basic 인증
4. throttle_classes : 사용량 제한 class 다수
 - 빈 튜플
5. permission_classes : 권한 class 다수
 - rest_framework.permissions.AllowAny : 누구라도 접근 허용
6. content_negotiation_class : 요청에 따라 적절한 직렬화/비직렬화 class를 선택하는 class
 - rest_framework.negotiation.DefaultContentNegotiation
 - 같은 URL로의 요청이지만, JSON응답을 요구하는 것이냐 / HTML응답을 요구하는 것인지 판단
7. methodata_class : 메타 정보를 처리하는 class
 - rest_framework.metadata.SimpleMetadata
8. versioning_class : 요청에서 API버전 정보를 탐지하는 class
 - None : API 버전 정보를 탐지하지 않겠다.
 - 요청 URL에서, GET인자에서, HRADER에서 버전정보를 탐지하여, 해당 버전의 API뷰가 호출되도록 한다.

### DRF의 2가지 기본 뷰

1. APIView : 클래스 기반 뷰
2. @api_view : 함수 기반 뷰를 위한 장식자


### APIView

APIView -> Generic -> ViewSet

하나의 CBV이므로 하나의 URL만 처리 가능

각 method(get, post, put, delete)에 맞게 멤버 함수를 구현하면, 해당 method요청이 들어올 때 호출

1. 직렬화/비직렬화 처리 (JSON 등)
2. 인증 체크
3. 사용량 제한 체크 : 호출 허용량 범위인지 체크
4. 권한 클래스 지정 : 비인증/인증 유저에 대해 해당 API호출을 허용할 것인지를 결정
5. 요청된 API 버전 문자열을 탐지하여, request.version에 저장

위의 셋팅을 한 다음에 멤버함수에 맞게 호출 한다.

