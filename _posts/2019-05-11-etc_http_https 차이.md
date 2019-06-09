---
layout: post
title:  "(etc). Http와 Https의 차이"
comments: true
description: "Git"
author: SeungHyeon Tak
date:   2019-05-11 23:39:00 +0700
categories: [etc]
tags: [etc]
keywords: "Http, Https"
---
#### Http와 Https의 차이

* http - HyperText Transfer Protocol - 문서를 주고 받는 용도로 사용되었다.
   * http - http는 웹브라우저(client)와 서버(server)간의 웹페이지같은 자원을 주고 받을 때 쓰는 통신 규약이다.(http는 텍스트 교환)

* https - 인터넷 상에서 정보를 암호화하는 SSL(Secure Sockey Layer)프로토콜을 이용하여 웹브라우저(client)와 서버가 데이터를 주고 받는 통신 규약이다.
   * https는 http의 보안상의 문제를 해결해주는 프로토콜이다.
   * http 메시지(text)를 암호화 하는것
   * https의 암호화 원리를 간단히 알아보면 핵심은 공개키 암호화 방식이다.
<br>
* HTTP Status Code
  * 200 - 정상 요청
  * 302 - 임시 URL로 이동
  * 404 - 서버가 요청한 페이지를 찾을 수 없음
  * 500 - 서버 오류 발생

* 200
  - HttpResponse()
  - render()
  - JsonResponse()

* 302 - redirect (특정 페이지로 이동)
  - HttpResponseRedirect()
  - resolve_url()
  - redirect()

* status code 404
  - raise Http404 : 404 응답 처리
  - get_object_or_404
    * views.py에서 이 표현을 써서 404에러 처리
  - HttpResponseNotFound : 잘 안씀

* 500 - 갑작스러운 오류 - 서버오류 아님
