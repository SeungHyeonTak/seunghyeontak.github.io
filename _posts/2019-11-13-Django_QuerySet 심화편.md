---
layout: post
title:  "Django_queryset 심화편"
comments: true
description: "Django, QuerySet"
author: SeungHyeon Tak
date:   2019-11-13 23:30:00 +0700
categories: [Django]
tags: [Django]
keywords: "Django, QuerySet"
---
#### Django - QuerySet 반드시 알아야 할 5가지 ORM쿼리
<br>

기존에 정리했던 쿼리와는 달리 비지니스 로직이 복잡한 상황에서 어떻게 ORM쿼리를 해야하는지
알아보는 시간을 가지려 한다.<br>
구글링으로 찾기 어려운 ORM쿼리 꿀팁(?) 5가지를 보려고 한다. <br>
<br>

#### Group by의 컬럼을 명시적으로 지정하고 싶을 때

일단 sql쿼리를 볼껀데, select절과 group by절에 명시적으로 지정해 주면 group by할 칼럼을 정해준다. <br>
<br>

```text
# sql query

select item_no, sum(qty) as total_qty 
from item 
where delete=False 
group by item_no

```

<br>

ORM으로 처리하기위해서는 `values()`메서드와 `order_by()`메서드에 같이 지정해주면 동일한 쿼리를 할 수 있다.

```python
Item.objects.filter(deleted=False).values('item_no').annotate(total_qty=Coalesce(Sum('qty'), Value(0),).order_by('item_no'))

# Item 모델에서 item_no 컬럼을 기준으로 group by를 하는 쿼리이다.

```

*****

#### Group by 후 특정 기준으로 그룹별로 하나의 row만 쿼리하고 싶을때


**ex)**

```text
------
table
------
group1 - 1
group1 - 2
group2 - 1
group2 - 2
group3 - 1
group3 - 2
group3 - 3
------

// 여기서 이름이 중복인 테이블에서 group 별로 첫번째 row만 쿼리하려는 상황
```

예를 들어 Django model을 만들고 QuerySet을 해보자 <br>

```python
# model

class Student(Model):
    student_no = IntegerField()
    username = CharField(max_length=200, unique=True)

class Score(Model):
    score_no = IntegerField()
    student_no = ForeignKey(Student)
    date = DateTimeField()
    score = IntegerField()

```

학생 점수 모델이 존재하고 점수는 학생을 FK로 참조할때 학생별로 가장 최신 치른 시험의 점수를 쿼리하고 싶은 상황이라면, <br>
점수데이터를 학생명과 날짜별로 정렬하고, 학생명별로 `distinct()`메서드로 설정하여 하나의 row만 쿼리할 수 있다. <br>


```python
# ORM QuerySet

Score.objects.order_by('student_no__username', '-date').distinct('student_no__username')

# order_by에서 눈여겨 볼점은 동일한 점수가 존재할 수 있기 때문에 date라는 컬럼을 정렬조건으로 추가한 것이다.

# 순서가 중요하다!!!!!!
# order_by('-date', 'student_no__username')은 불가능!!

```

*****

#### 하나의 모델에서 독립적인 여러조건으로 Aggregation하고 싶을때

상품의 리뷰데이터 모델을 가정한다.<br>
리뷰는 `review_type`이라는 칼럼으로 타입을 구분하고 있다.

```python
# model

from django.db import models

class ItemReview(Model):
    pk = BigAutoField)
    item_no = ForeignKey(Item)
    point = IntegerField()
    review_type = IntegerField() # 0: tetx, 1: photo

# 여기서 한번의 쿼리로 모든 리뷰의 개수와, `review_type=1`인 리뷰의 개수는 어떻게 구별할수 있는가?
```

<br>

```python
# QuerySet

from django.db.models import Sum, Count, Case, When, Avg, IntegerField, Value

ItemReview.objects.filter(delete=False)
	.annotate(
		product_review_type=Case(When(review_type=1,then=1), 
		default=0, output_field=IntegerField()
	)
).aggregate(
	product_review_count=Coalesce(
		Sum('product_review_type'), Value(0)	
	),
	all_review_count=Coalesce(
		Count('item_review_no')
	),
	average_point=Coalesce(
		Avg('point'), Value(0)
	),
)
```

<br>
`annotate()`는 주석이라는 뜻으로, 기존 컬럼값을 manipulate하여 새로운 컬럼의 값으로 생성하는 역할을 한다. `annotate`를 활용하여 `review_type`이 특정값인 리뷰를 1, 아닌 경우 0인 값이 되는 `product_review_type` 컬럼을 하나 추가한다. <br>
<br>
`aggregate`함수는, 모든 컬럼을 합쳐주는 기능을 한다. `annotate()`에서 생성한 `product_review_type`컬럼의 값을 모두 더하면 특정 review_type인 총 리뷰의 개수를 구할 수 있다. <br>
<br>
`all_review_count`는 모든 리뷰의 수, `average_point`는 모든 리뷰의 평점을 계산하는 예시이다.<br>
<br>

*****

#### Exists, Not Exists

쿼리에서는 Exists 함수를 쉽게 사용한다. <br>

```python
# SQL

select * form a where exists(select 1 form b where b.example_no=a.example_no)

```

Django에서는 `OuterRef`함수를 사용하여 서브쿼리를 작성해야 한다. <br>
서브 쿼리로 들어갈 쿼리셋을 미리 작성할 때, 메인 쿼리와 조인할 컬럼을 `OuterRef`메소드를 지정해준다. <br>

```python
subquery = B.objects.filter(example_no=OuterRef('example_no'))
```

B에 존재하는 A의 row를 식별하기 위하여 `annotate`로 새로운 컬럼을 생성해주고 filter에서 True로 존재하는 row만 제한하여 쿼리해준다.<br>
SQL에서 NotExists는 ~Exists()로 쿼리할 수 있다. <br>

```python
qs = A.objects.filter.annotate(joined_example_no=Exists(subquery)
				.filter(joined_example_no=True))

```

*****

#### 일대다(1:Many)모델에서 many의 값을 aggregate하여 1모델에 업데이트 하고 싶을때

> 다시 학생과 점수모델을 불러온다.
<br>

```python
class Student(Model):
    username = CharField(max_length=200,unique = True)
    total_eng_score = IntegerField()
    total_math_score = IntegerField()
class Score(Model):
    student = ForeignKey(Student)
    date = DateTimeField()
    score = IntegerField()
    type = CharField() # type: eng, math
```

학생의 수학, 영어 총 시험점수를 합하여 학생의 모델에 갱신해주고 싶을때

```python
# 업데이트할 점수를 aggregate하는 서브쿼리
score_aggr = Score.objects.filter(
    student=OuterRef('student')
).values(
    'student'
).annotate(
    sum_eng_score=Coalesce(
        Sum(
            Case(
                When(
                    type='eng',
                    then=F('score'),
                ),
                default=0,
                output_field=IntegerField()
            )
        ),
        Value(0)
    ),
    sum_math_score=Coalesce(
        Sum(
            Case(
                When(
                    type='math',
                    then=F('score'),
                ),
                default=0,
                output_field=IntegerField()
            )
        ),
        Value(0)
    ),
)
```

위의 쿼리를 보면 앞 전 주제에서 보았던 values()메서드로 명시적으로 group by 할 컬럼을 지정해주고, OuterRef()메서드에 메인메서드로 조인할 컬럼을 지정해 주었다. <br>

```python
# 위의 서브쿼리를 Student모델에 업데이트하는 메인쿼리셋
Student.object.annotate(
    aggr_total_eng_score=Subquery(
        score_aggr.values('sum_eng_score')[:1],
        output_field=IntegerField()
    ),
    aggr_total_math_score=Subquery(
        score_aggr.values('sum_math_score')[:1],
        output_field=IntegerField()
    ),
).update(
    total_eng_score=F('aggr_total_eng_score'),
    total_math_score=F('aggr_total_math_score')
)
```

더 간결한 방법은 밑의 코드

```python
Student.object.update(
    total_eng_score=Subquery(
        score_aggr.values('total_eng_score')[:1]
    ),
    total_maht_score=Subquery(
        score_aggr.values('total_math_score')[:1]
    )
)
```

추가적으로 `aggregate`한 값들에 조건을 주고 싶은 경우에는 위의 방식으로 활용해야한다. <br>
총합한 점수가 0보다 작은 경우에는 0으로 바꿔서 업데이트를 하는 경우이다.<br>


```python
Student.object.annotate(
    aggr_total_eng_score=Subquery(
        score_aggr.values('total_eng_score')[:1],
        output_field=IntegerField()
    )
).update(
    total_eng_score=Case(
        When(
            aggr_total_eng_score__lt=0,
            then=0
        ),
        default=F('aggr_total_eng_score'),
        output_field=IntegerField()
    )
)

```

*****

