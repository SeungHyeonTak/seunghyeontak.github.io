---
layout: post
title:  "Django_기본.URL"
comments: true
description: "Django"
author: SeungHyeon Tak
date:   2019-06-02 20:29:00 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.기본 웹 개발

* URL / 정규 표현식 쓰는법
<br>
* 정규 표현식
  * 거의 모든 프로그래밍 언어에서 지원가능
  * 장고 URL Dispatcher에서는 정규표현식을 통한 URL 매칭
  * 문법
    * 1글자에 대한 패턴 + 연속된 출연 횟수 지정
    * 대괄호 내에 1글자에 대한 후보 글자들을 나열
<br>
* 정규 표현식 패턴 쓰는법
  * 1자리
    * "[0123456789]" or "[0-9]" or "[\d]"
  * 2자리
    * "[0123456789][0123456789]" or "[0-9][0-9]" or "\d\d"
  * 3자리
    * "\d\d\d" or "\d{n}" -> 2자리면 n에다가 2를 써주면 된다.
  * 알파벳 소문자 1글자
    * "abcd...z" or "[a-z]"
  * 알파벳 대문자 1글자
    * "ABCD...Z" or "[A-Z]"
<br>
* 반복 횟수 지정 문법
  * r"\d" : 별도 횟수 지정 없음  1회 반복
  * r"\d{1}" : 1회 반복
  * r"\d{2}" : 2회 반복
  * r"\d{2,4}" : 2회~4회 반복
  * r"\d?" : 0회 혹은 1회 반복
  * r"\d*" : 0회 이상 반복
  * r"\d+" : 1회 이상 반복  *(제일 많이 씀)*
*주의 할 사항 _ 정규표현식은 띄워쓰기 하나에도 민감해서 띄워쓰기를 하면 안됨*
<br>

* path()와 re_path()
  * path()
    * 장고 1.x 버전에서 쓰는 표현식과 다르게 정규표현식 기입이 간소화

```python
path('shop/<int:year>/', view.year_test),
```

  * re_path()
    * url()을 쓰는 형식과 같음

```python
url(r'^shop/(?P<year>[0-9]{4})/$', view.year_test),
re_path(r'^shop/(?P<year>[0-9]{4})/$', view.year_test),
```

이번에 공부 하면서 커스텀 Path Converter에 대해 알아봤는데

```python
# 앱이름/converters.py
class FourDigitYearConverter:
    regex = '\d{4}"
    def to_python(self, value): # url로부터 추출한 문자열을 뷰에 넘겨주기 전에 변환
        return int(value)
    def to_url(self, value): # url reverse 시에 호출
        return "%04d" % value

# 앱이름/urls.py
from django.urls import register_converter
register_converter(FourDigitYearConverter, 'yyyy')
urlpatterns = [
    path('articles/<yyyy:year>/', views.year_archive),
]

# views.py
from django.http import HttpResponse

def archives_year(request, year):
    return HttpResponse(f'{year}년도에 대한 내용')
```

articles/2019 이렇게 링크를 넣으면 이것만의 새로운 url이 보여진다.

#### get_absolute_url
* 간편하게 url을 불러올 수 있다. 꼭 구현하기
* 특정 모델에 대한 Detail뷰를 작성할 경우, Detail에 대한 URLConf설정을 하자마자
  * 꼭 get_absolute_url을 설정 해주자(매우 간결해짐)

* detail뷰를 만들게 되면 get_absolute_url() 멤버 함수를 무조건 선언

```python
# models.py에서 작성

class Post(models.Model):
    ...
    def get_absolute_url(self):
	return reverse('blog:post_detail', args=[self.id])
```

* resolve_url / redirect를 통한 활용

```python
resolve_url('blog:post_detail', post.id)
resolve_url(post) # get_absolute_url 있는지 체크해서 리턴
```

* url templates tag로 활용

```java
<li>
    <a href="{{ post.get_absolute_url }}">{{ post.title }}</a> 
</li>
```

마지막으로 새로운 장고 앱을 생성후 진행 작업 알아보기
1. 앱 생성

```
$ python manage.py startapp shop
```

2. 앱 이름/urls.py 파일 생성
3. 프로젝트/urls.py에 include 적용
4. 프로젝트/settings.py의 INSTALLED_APPS에 앱 이름 등록
