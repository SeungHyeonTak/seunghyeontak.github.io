---
layout: post
title:  "Django_DRF 요청 / 응답"
comments: true
description: "Django, DRF"
author: SeungHyeon Tak
date:   2019-11-19 00:27:00 +0700
categories: [Django]
tags: [Django]
keywords: "Django, DRF"
---
#### Django - DRF 요청과 응답
<br>

REST 프레임워크의 두가지 핵심요소에 대해 알아보고자 한다. <br>

#### 요청(Request) 객체
REST 프레임워크의 Request 객체는 평범한 HttpRequest 객체를 확장하여 좀 더 유연하게 요청을 파싱한다. <br>
Request 객체의 핵심부는 request.data 속성이다. <br>
이 속성은 request.POST와 비슷하지만 웹 API에 좀 더 적합하다. <br>
<br>
> request.POST # 폼 데이터만 다루며, POST 메서드에서만 사용가능 <br>
> request.data # 아무 데이터나 다룰 수 있고 POST뿐만 아니라 PUT 과 PATCH 메서드에서도 사용가능 <br>
<br>

#### 응답(Response)객체
REST 프레임워크에는 Response 객체도 존재한다. <br>
이 객체는 TemplateResponse 타입이며, 렌더링 되지 않은 콘텐츠를 불러와 클라이언트에게 return할 콘텐츠 형태로 변환한다. <br>
<br>
> return Response(data) # 클라이언트가 요청한형태로 콘텐트를 렌더링함
<br>

#### 상태 코드
우리가 만든 뷰에서 숫자 형태의 HTTP 상태코드를 사용하는 경우, 읽기에도 어렵고, 코드에 오류가 있더라도 발견하기 쉽지 않다.<br>
REST 프레임워크에서는 오류에 대해 조금 더 명확한 Status코드를 제공한다. <br>
따라서 숫자로 된 식별자 보다는 문자 형태의 식별자를 사용하는 것이 효율적이다. <br>
<br>

#### API뷰 감싸기
REST 프레임워크는 API뷰를 작성하는데 사용할 수 있는 두가지 wrapper를 제공한다. <br>
첫번째, FBV(함수 기반 뷰)에서 사용할 수 있는 `@api_view` 데코레이터 <br>
두번째, CBV(클래스 기반 뷰)에서 사용할 수 있는 `APIView`클래스 <br>

* 이 wapper들은 Request에 기능을 더하거나, context를 추가해 컨텐츠가 잘 변환되도록 한다.
* 또한 405 Method Not Allowed를 반환하거나, request.data가 깨졌을 때, ParseError를 던지기도 한다.
<br>
(작성중...)

