---
layout: post
title:  "Django_queryset 성능 개선"
comments: true
description: "Django, QuerySet"
author: SeungHyeon Tak
date:   2019-10-30 20:46:13 +0700
categories: [Django]
tags: [Django]
keywords: "Django, QuerySet"
---
#### Django - 쿼리 성능 향상
<br>

#### select_related() / prefetch_related()
<br>

> 우리가 DB를 작성함에 있어서 DB의 성능 및 처리속도 최적화는 꼭 필요하다. <br>
> 일단 확인하기 전에 `django-debug-toolbar`를 설치하자<br>

<br>
디버그툴바 설치 : [django-debug-toolbar](https://seunghyeontak.github.io/2019-06-03/Django_%EA%B8%B0%EB%B3%B8_toolbar/)
<br>

> 여기서 설치하고 SQL부분에서 우리가 쿼리를 돌리면서 얼마나 반복하고 최소화 하는지 확인할 수 있다. 물론 그 외에도 많은것을 확인할 수 있다. <br>

보통 우리가 기본적으로 알고 잘 모르는 초급 개발자가 많이 쓰는 문구는 `Movie.objects.all()`을 많이 쓸것이다. 하지만 이 구문을 쓰면 적은 데이터를 조회하고 화면까지 출력하는데 몇십번의 쿼리가 반복하고 중복될것이다.

그 문제를 해결 하기 위해 활용할 수 있는 방법이 있다.

* select_related()
  * ForeignKey, OneToOneField 관계에서 활용

* prefetch_related()
  * ManyToManyField, ForeignKey의 reverse relation에서 활용

이러한 구문들을 쓰게 되면 눈에 확띄게 쿼리문이 반복하는걸 줄일 수 있을 것이다.
<br>
> 사용 방법은 다음과 같다. <br>
> Order.objects.select_related('user').filter(user__level='admin').values('id') <br>
<br>

이렇게 사용할 수 있으며 맨뒤에 `values`라는 구문이 있는데 이 구문은 sql의 select에 해당한다. `values`를 사용하지 않으면 sql의 `select *`와 같이 전체 컬럼을 출력한다.
<br>

* queryset에서 SQL문을 보고 싶다면
> `str(order.query())`이런식으로 사용한다면 SQL문을 직접 볼 수 있다. <br>
<br>

#### 지연연산에 대해
<br>
> Django의 ORM은 무슨 연산인가? <br>
> -> 지연연산이다 <br>
* 지연 연산이란?
  * 데이터가 정말로 필요하기 전  까지는 Django가 SQL을 호출하지 않는 특징
  * 따라서 ORM 메소드와 함수를 얼마든지 연결해서 코드를 쓸 수 있다.
  * 한 줄에 길게 쓰는 대신에 여러줄에 나눠 쓰면 가독성을 향상시키며, 유지보수를 쉽게 해준다.

<br>

```python
from django.models import Q

from promos.models import Promo

def fun_function(**kwargs):
  """유효한 아이스크림 프로모션 찾기"""
  results = Promo.objects.active()
  results = results.filter(
              Q(name__startswith=name)|
              Q(description__icontains=name)
              )             
  results = results.exclude(status='melted')
  results = results.select_related('flavors')
  return results
```

<br>

