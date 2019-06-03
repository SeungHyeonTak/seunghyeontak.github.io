---
layout: post
title:  "Django_기본.QuerySet"
comments: true
description: "Django"
author: SeungHyeon Tak
date:   2019-06-02 19:16:00 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.기본 웹 개발

####QuerySet

* Model Manager
  * 데이터베이스 질의 인터페이스를 제공
  * 디폴트 Manager로서 ModelCls.objects가 제공

```python
# 생성되는 SQL 윤곽 -> SELECT * FROM app_model;
ModelCls.objects.all()

# 생성되는 SQL 윤곽 -> SELECT * FROM app_model ORDER BY id DESC LIMIT 10;
ModelCls.objects.all().order_by('-id')[:10]

# 생성되는 SQL 윤곽 -> INSERT INTO app_model (title) VALUES ("New Title");
ModelCls.objects.create(title="New Title")

# Django에서 이렇게 써준다.
```

#### QuerySet
* SQL을 생성해주는 인터페이스
* 순회가능한 객체
* iterable한 객체

* Chaining을 지원
  * 이런식으로 쓸 수 있음
    * Post.objects.all().filter().exclude()... -> QuerySet
  * QuerySet은 Lazy한 특성(게으른 특성을 가지고 있음)
    * QuerySet을 만드는 동안에는 DB접근을 안함
    * 실제로 데이터가 필요한 시점에 접근을 한다.
<br>
* 조회요청 방법
  * queryset.filter() -> 조건을 넣을 수 있음(and 조건)
  * querset.exclude() -> Not 조건을 출력
* 특정 모델객체 1개 획득을 시도
  * queryset[숫자인덱스] - 10개중 [0]이렇게 하나만 쓸 수 있음
  * queryset.get() - 1개를 가져온다 / 0개 및 2개 이상을 가져올시 예외발생
  (0개이면 - DoesNotExist / 2개이상 - MultipelobjectsReturned)
  * queryset.first() - 첫번째 
  * queryset.last() - 마지막
  (위의 2개는 해당 레코드가 없으면 None이 출력)
<br>

* filter <-> exclude (SELECT 쿼리에 WHERE 조건 추가)
  * 인자로 "필드명 = 조건값" 지정
  * 1개 이상의 인자 지정 -> 모두 AND 조건으로 묶임

* OR 조건 추가
  * django.db.models.Q 활용하여 씀
  * Q로 인해 OR조건으로 묶는다.
  
  ```python
  Item.objects.filter(Q(name="New Item")&Q(price=3000))
  Item.objects.filter(Q(name="New Item")|Q(price=3000))
  # 이런식으로 사용 가능
  ```
  
filter / exclude의 필드 타입 다양한 조건 매칭
* 숫자 / 날짜 / 시간 필드
  * 필드명__lt = 조건값 è 필드명 < 조건값
  * 필드명__lte = 조건값 è 필드명 <= 조건값
  * 필드명__gt = 조건값 è 필드명 > 조건값
  * 필드명__gte = 조건값 è 필드명 >= 조건값
* 문자열 필드
  * 필드명__startswith = 조건값 è 필드명 LIKE “조건값%”
  * 필드명__endswith = 조건값 è 필드명 LIKE “%조건값”
  * 필드명__contains = 조건값 è 필드명 LIKE “%조건값%”
  * 필드명__istartswith = 조건값 è 필드명 ILIKE “조건값%”
  * 필드명__iendswith = 조건값 è 필드명 ILIKE “%조건값”
  * 필드명__icontains = 조건값 è 필드명 ILIKE “%조건값%”

* 정렬 조건 추가
(SELECT 쿼리에 ORDER BY 추가)
  * 정렬 조건을 추가 하지 않으면 일관된 순서를 보장 받을 수 없으므로 항상 정렬을 시키는것을 습관화 하는것이 좋다.
  * 정렬 조건의 2가지 방법
    * 모델 클래스의 Meta 속성으로 ordering 설정 : list로 지정 [] (요즘 쓰는 방식)
    * 모든 queryset에 order_by()에 지정해준다.(지정할때마다 이렇게 해야함)

* 슬라이싱을 통한 범위 조건 추가하기
(OFFSET/LIMIT 추가)
  * str/list/tuple 에서의 슬라이싱과 거의 유사(※역순 슬라이싱은 지원 불가)
  * 객체 [start:stop:step]
    * OFFSET -> start
    * LIMIT -> stop - start
  * 혹시나 역순 슬라이싱을 해야한다면
    * 사용불가 예)
    
    ```python
    qs = Item.objects.all().order_by('id')
    qs[-10:]
    Error
    AssertionError: Negative indexing is not supported
    #라는 에러가 뜬다. 이럴때를 대비하여 reverse를 쓸 수 있는데 사용방법은 다음과 같다.
    reversed(qs.reverse()[:10])
    # 이런식으로 쓸 수 있다.
    ```