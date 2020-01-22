---
layout: post
title:  "Django request"
comments: true
description: "Django request"
author: SeungHyeon Tak
date:   2020-01-22 23:23:00 +0700
categories: [Django]
tags: [Django]
keywords: "Django request"
---
## Django request에 대해

## Request and response objects에 대해

Reqeust - 요청

response - 응답

Django는 요청 및 응답 객체를 사용하여 시스템을 통해 전달한다.

페이지가 요청되면 Django는 요청에 대한 메타데이터가 포함 된 HttpRequest 객체를 만든다.

그런 다음 Django는 적절한 보기를 로드하고 HttpRequest를 첫번째 인수로 view함수에 전달한다.

각 view는 HttpResponse 객체를 반환한다.

#### HttpRequest.scheme

요청 체계를 나타내는 문자열이다. (http or https).


#### HttpReqeust.body

Http요청 본문은 바이트 문자열이다.

이진 이미지, XML페이로드 등 기존 HTML 양식과 다른 방식으로 데이터를 처리하는데 유용하다.

기존 양식 데이터를 처리하려면 HttpRequest.POST를 사용하기

파일과 같은 인터페이스를 사용하여 HttpRequest에서 읽을 수도 있다. 

#### HttpRequest.path

체계 또는 도메인을 포함하지 않고 요청 된 페이지의 전체 경로를 나타내는 문자열이다.

ex)  "/music/bands/the_beatles/"

#### HttpRequest.path_info

일부 웹 서버 구성에서 호스트 이름 뒤의 URL 부분은 스크립트 접두사 부분과 경로 정보 부분으로 나뉜다.

path_info속성은 사용중인 웹 서버에 관계없이 항상 경로의 경로 정보 부분을 포함한다.

경로 대신이 옵션을 사용하면 테스트 서버와 배포 서버간의 코드를 쉽게 이동할 수 있다.

ex) 

어플리케이션의 WSGIScriptAlias가 '/minfo'로 설정된 경우 경로는 '/minfo/music/bands/the_beatles/'이고 

**path_info**는 '/music/bands/the_beatles/' 일 수 있다.

#### HttpRequest.method

요청에 사용된 Http메소드를 나타내는 문자열이다. (대문자로 보장됨)

```python
if request.method == 'GET':
    do_something()
elif request.method == 'POST':
    do_something_else()
```

#### HttpRequest.encoding

현재 어떤 속성의 인코딩을 쓰고 있는지 알 수 있다.

ex) request.encoding // UTF-8

#### HttpRequest.content_type

Content_type 헤더에서 구문 분석 된 요청의 mine유형을 나타내는 문자열이다.

#### HttpRequest.content_params

CONTENT_TYPE 헤더에 포함된 키 / 값 매개 변수 사전이다.

#### HttpRequest.GET

모든 HTTP GET 매개 변수를 포함하는 사전과 유사한 객체이다.

#### HttpRequest.POST

모든 HTTP POST 매개 변수를 포함하는 사전과 유사한 객체로, 요청에 양식 데이터가 포함되어 있다.

비형식 데이터에 액세스 해야하는 경우 대신 HttpRequest.body 속성을 통해 액세스 한다.

POST가 비어있는 POST dict을 사용하여 POST를 통해 요청을 수신 할 수 있다.

양식이 POST HTTP 메소드를 통해 요청 되었지만 양식 데이터는 포함되지 않았다.

따라서 POST 메소드 사용을 확인하기 위해 `if request.POST`를 사용하지 말고

`if request.method == "POST"`인 경우 사용 가능하다.

#### HttpRequest.COOKIES

모든 쿠키를 포함하는 dict. key와 values는 문자열이다.

#### HttpRequest.FILES 

업로드 된 모든 파일을 포함하는 dict와 유사한 object이다.

FILES의 각 키는 `<input type="file" name="">`의 이름이다.

요청 방법이 POST이고 요청에 게시된 <form>에 `enctype="multipart/form-data"`가 있는 경우 파일에는 데이터만 포함된다. 

그렇지 않으면 FILES는 빈 dict와 유사한 object가 된다.

#### HttpReqeust.META

사용 가능한 모든 HTTP 헤더가 포함된 dict이다.

사용 가능한 헤더는 클라이언트와 서버에 따라 다르지만 다음은 몇가지 예를 든 데이터이다.

* CONTENT_LENGTH -- 요청 본문의 길이 이다.(문자열)
* CONTENT_TYPE -- 요청 본문의 MIME 유형
* HTTP_ACCEPT -- 응답에 허용되는 content type
* HTTP_ACCEPT_ENCODING -- 응답에 허용되는 인코딩
* HTTP_ACCEPT_LANGUAGE -- 응답 가능한 언어
* HTTP_HOST -- 클라이언트가 보낸 HTTP host header
* HTTP_REFERER -- 참조 페이지가 있는 경우

등등 많은 내용은 django document를 참고 하면 된다.

자세한 내용은 [django document](https://docs.djangoproject.com/ko/3.0/ref/request-response/#django.http.HttpRequest)에서 찾아보면 된다.
