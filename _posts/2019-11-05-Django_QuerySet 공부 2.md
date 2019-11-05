---
layout: post
title:  "Django_queryset에 대한 이해 2"
comments: true
description: "Django, QuerySet"
author: SeungHyeon Tak
date:   2019-11-05 23:13:00 +0700
categories: [Django]
tags: [Django]
keywords: "Django, QuerySet"
---
#### Django - QuerySet 이해하기
<br>

#### related_name
<br>
Django에서 모델 테이블을 다 짜거나 파악한 뒤에 우리는 Query로 추출을 할 수 있다.
이 과정에서 바로 코드를 친다던지 검색을 한다던지 등등 이런 부분은 별로 좋지 않다.
그래서 제일 먼저 할 수 있는것이 바로 말로 하나하나 풀어서 정리하는것이다.
이 모델의 핵심적인 요소가 무었인지 필요한 부분은 무엇인지 정하고 문제를 하나하나 풀어가며 query를 추출 하는것이 가장 올바른 형식이다.
<br>

오늘 볼 내용은 모든걸 알기 보단 오늘 공부했던걸 정리하는 형식으로 정리를 해나아갈 것 이다.
<br>
> related_name <br>

예를 들어보면

```python
class User(models.Model):
    pass

class Comment(models.Model):
    User = models.ForeignKey(User, on_delete=models.CASCADE, related_name=들어갈 내용)
```

<br>
여기서 `related_name`에 들어가는 구문은 무었일까? 지금생각해보면 별거 아니지만 얼마전까지의 나였으면 그냥 단순하게 User를 ForeignKey로 받아왔다고 생각하여 `user`를 넣어줬을 것이다. 하지만 제발 그러지말자 만약 그렇게 되면 User의 Comment를 불러올때 `comment = user.user.all()` 이런 상황이 올것이다. 이렇게 되면 누가 봐도 헷갈릴것이다.
제대로 썻다면 `user.comment.all()` 이런 형식으로 됬을것이다.
User가 Comment에세 정참조라면
Comment는 User에게 역참조가 된다.

그래서 정리하자면
* ForeignKey로 연결시킬때 `related_name=comment`라고 지정해놓으면 user.comment가 Comment model에 생기는 것이 아니라 Comment가 참조하고 있는 User 모델에 생긴다는 뜻이다.
* `related_name`은 참조해준 객체 입장에서 적어줘야한다.
* `related_name`은 ORM 쿼리문 없이 소통하기 위한것임을 알아두길 바란다.

*****

#### annotate / aggregate
<br>

Django를 처음 배운 사람이라면 `Post.objects.all()` 이라는 구문은 잘 알것이다.
하지만 이 구문은 매우 좋지 않은 구문이다. 왜냐하면 qeury의 성능을 떨어뜨리기 때문이다.
성능을 높이기에 많은 구문들이 있지만 그 중 하나(?)인 `annotate / aggregate`를 살펴 볼것이다. 
바로 예를 들어 살표 보도록 한다.
책을 판매하는 사이트가 있다고 가정한다.
간단하게 책 이름 - 가격으로 모델을 구성해본다.

```python
class Product(models.Model):
    name = models.CharField('책이름', max_length=150, unique=True)
    price = models.IntegetField('가격')
```

이러한 제품들이 팔린 내역을 확인한다.

```
2019-10-01 | 언어의 온도
2019-10-01 | 언어의 온도
2019-10-01 | 모든순간이 너였다
2019-10-02 | 언어의 온도
2019-10-02 | 모든순간이 너였다
2019-10-05 | 언어의 온도
2019-10-06 | 나는 둔감하게 살기로 했다
2019-10-06 | 언어의 온도
2019-10-07 | 모든순간이 너였다
2019-10-10 | 언어의 온도
2019-10-10 | 고급 C 언어
```

이러한 판매 내역 모델을 구현하자

```python
class OrderLog(models.Model):
    product = model.Foreignkey(Product)
    created = model.DateTimeField('판매일', auto_now_add=True)
```

* 문제
> 총 판매액 구하기 <br>

<br>
아까 처음 했던말 기억하는지 모르겠지만, 이렇게 큰 요구사항이 던져졌다.
그러면 어떻게 쪼개야 할 지 생각해보자
<br>
판매 내역을 보면 물품 이름만 있고 가격이 안적혀 있다.
`orders = OrderLog.objects.values('created', 'product__name', 'product__price')`
이렇게 한줄 한줄 나열 할 수 있다.
하지만 `__` 언더바가 두개나 쓰여 보기 싫은 사람도 분명 있을것이다.
그 부분을 해결하기 위해 annotate()와 F객체를 쓴다.

```python
from django.db.models import F, Sum, Count, Case, When# F()를 쓰려면 이렇게 메서드를 불러야한다. 나머지도 곧 쓸테니 미리 불러두자

orders = OrderLog.objects.values('created', 'product__name', 'product__price')
위의 작성한 쿼리를 변형하여 이렇게 작성 할 수 있다.
orders = OrderLog.objects.annotate(name=F('product__name'), price=F('product__price')).values('create', 'name', 'price')
이렇게 나타낼 수 있다.
# 필자는 위에 방식을 더 선호한다. 왜? 아직 위에가 더 편해서 ^^ 
``` 

이렇게 구매날짜 책이름 가격을 나열 하는데 성공 했다.

<br>
> annotate에 대해 <br>
방금 까지 사용한 annotate는 필드 하나를 만들고 거기에 어떤 내용을 채운다 라고 생각할 수 있다.(필드를 추가한다고 생각하자!)

<br>
> 제품가격을 모두 더해 총 판매액 구하기 <br>

지금까지 팔린 책들의 내역을 조회해봤다면 이제 이 책들의 판매액을 합산 시켜준다.

Django에서는 QuerySet의 특정 필드를 모두 더할 때는 aggregate 메서드를 사용한다.
앞의 코드를 이어서 실행해보면

```python
orders.aggregate(total_price=Sum('price')) #로 나타낼 수 있으며
# 실행 결과는 {'total_price': 234523} 이런식으로 나올것이다.
```

> **aggregate**의 사전적 의미는 합계 / 종합이다. <br>
> 필드 전체의 합이나 평균, 개수 등을 계산할 때 사용하면 된다.

<br>

> 그러면 총합을 구해봤으니 일별 판매액을 구해볼 수 있다. <br>

<br>
우리는 앞서 일별로 판매액을 구하고 싶다고 했다.
이때 뭔가를 기준으로 값들을 묶고 싶다면 `values`와 `annotate`를 사용하면 된다.
여기서 일별(created)로 묶고 싶은 값(price)로 해서 실행하면 되는데, 말로만으론 잘 감이 안올수도 있다.
<br>

```python
daily_list = orders.values('created').annotate(daily_total=Sum('product__price'))

# 이제 daily_list를 for문 돌려서 확인해보자
# 확인해보면 날짜별로 daily_total된 값들이 잘 나열되있는걸 볼 수 있다.
```

※ 여기서 잠깐!!!!!
F()를 쓴 부분에서는 `values`메서드 이전에 `annotate`로 추가했던 필드는 `values` 메서드 이후에 나오는 `annotate`메서드에 참조할 수 없다.
즉, values가 annotate보다 뒤에 있어야 F()를 사용할 수 있다.

<br>

> 조금 더 들어가보면 이제 `날짜별`, `제품별` `판매개수`를 구해볼 수 있다. <br>
<br>
> values에 넣을 기준 필드 `created` / `name` 그리고 `annotate`에서는 레코드 개수를 세주기위한 `Count`메서드를 사용할것이다.
<br>
```python

daily_count = orders.values('created', 'name').annotate(count=Count('name'))

```

<br>

**자 여기서 잘 정리 안되는 분들은 잠깐 잘 정리하고 밑 부분 시작하기**

<br>
*****

<br>
> 특정 제품의 날짜별 판매 개수 구하기 <br>
<br>
> 전체 제품이 아닌 관심 있는 제품(특정제품)의 날짜별 판매 개수를 알고 싶을때
<br>
* `filter`를 사용한다.

말 그대로 간단하다. 내가 언어의 온도라는 책에 대한 리스를 알고 싶으면 filter에 특정 책 이름만 filter걸어주면 된다.
<br>

```python
book_daily_count = orders.filter(name='언어의 온도').values('created','name').annotate(count=Count('product'))
```

<br>
> 우리가 크게 목표로 잡았던 **annotate와 aggregate**이 유용한 사례를 살펴 볼 것이다. <br>
> 바로 결제 취소 발생이 되었을때 이다. <br>
> 물론 판매가 있으면 취소도 있는법! - 결제가 취소되었을 때는 취소 로그를 남겨야 한다. <br>
> 지금은 간단하게 결제 취소(is_cancel)필드를 추가하고, 결제 취소 내역인 경우에는 해당 필드를 True로 설정하겠다. <br>

* model을 수정하자

```python
class OrderLog(models.Model):
    product = models.ForeignKey('Product')
    created = models.DateTimeField('판매일', auto_now_add=True)
    is_cancel = models.BooleanField('결제 취소', default=False)  # 추가된 필드
```

그리고 결제 취소 내역을 쿼리로 추가한다.

```python
book_cancel = Product.objects.get(name='언어의 온도')
day = datetime(2019, 10, 1)
OrderLog.objects.create(created=day, product=book_cancel, is_cancel=True)
```

> 결제 취소 총 판매액 수정 <br>

지금 시점에 총 판매액을 다시 계산해보면 맨 마지막에 추가한 결제 취소 내역 때문에 총 판매액이 잘못 계산된다.
<br>
이 부분은 좀 까다롭다. 결제 취소라는 조건이 생겨나버렸기때문이다.
이를 위해 조건적 애너테이션을 사용해서 현재의 문제를 해결할 수 있다.
<br>

```python
order_list2 = orders.annotate(
	sales_price=Case(
	    When(
		is_cancel=False, # 결제 취소가 아닌 경우 
		than=F('price')
	    ),
	    default=0
	),
	cancel_price=Case(
	    When(
		is_cancel=True, # 결제 취소인 경우
		then=F('price')
	    ),
	    default=0
	)
)
```

조금 어렵게 보이지만 설명하자면
이렇게 구성하게 되면 판매내역엔 sales_price 필드에 price값이 들어가고, 결제 취소 내역인 경우엔 cancel_price 필드에 price값이 들어간다. default=0이므로 해당하지 않는 필드에는 0이 들어간다.

> 판매 금액의 합 - 취소 금액의 합 = 총 판매액 <br>

```python

result = order.aggregate(total_price=Sum('sales_price')-Sum('cancel_price'))

```

<br>
위 쿼리를 보면 알겠지만 aggregate를 사용하여 특정 필드의 연산을 처리했고, 판매금액과 취소금액을 연산하여 total_price필드에 넣었다.

이렇게 다양한 요구사항들이 언제 본인들을 당활시킬줄 모르니 하나의 요구사항이 오더라도 잘 쪼개서 생각하고 쿼리를 추출 하자

[참고 사이트](http://raccoonyy.github.io/django-annotate-and-aggregate-like-as-excel/)

****

#### select_related / prefetch_related

<br>
기본적으로 Django ORM을 돌릴때 불필요한 쿼리를 많이 요청하여 성능이 떨어질때가 많다.
실제로 djangp_debug_toolbar로 확인해보면 눈으로 볼 수 있다.
<br>

기본적으로 ORM에서는 하나의 모델을 바탕으로 ForeignKey가 묶인 다른 자료들도 불러서 사용가능함
여기서 그렇다하고 끝낼거였음 이거 하고 있지도 않았을 것이다.

#### *그렇담 뭐가 문제냐?*
하나의 모델을 불러오고 또 그 관련된 ForeignKey가 연관된 자료들을 불러오려고 하면 DB에 접근 을 여러번 해야한다.

그래서! 하나의 모델을 불러올 때 미리 ForeignKey로 연결된 자료까지 불러 오면 해당 DB에 접근을 한번만 해도 된다.
이렇게 성능을 올릴 수 있다.

<br>
그렇다면 **select_related / prefetch_related**는 무엇인가?
위 두개의 구문은 하나의 Query를 가져올때 미리 related objects(관련된 객체?)들을 다 불러 와주는 함수이다.

비록 query를 복잡하게 만들지만, 불러온 데이터는 모두 cache에 남아있게 되므로 DB에 다시 접근해야 하는 이슈가 줄어들 수 있다.
**(쉽게 한번 힘들게 적고 쉽게 불러올래 / 쉽게 적고 여기저기 돌아올래)**

위 두가지 다 DB에 접근하는 수를 줄여주면서 퍼포먼스를 줄여주지만 두개도 차이가 있다.
<br>

#### 차이

> select_related <br>

select_related는 ForeignKey or OneToOneField에서 사용가능하다.

<br>

> prefetch_related <br>

prefetch_related는 ForeignKey or ManyToMany등등 가리지 않고 사용가능하다.

<br>

둘다 필요한 데이터들을 한번에 가지고 올 수 있다. 하지만!
실제로 해당 내용을 한번에 모두 가지고 오기 위해 호출해야 하는 횟수가 다르다.
prefetch_related가 더 많은 쿼리문을 실행해야하고
select_related는 한번의 쿼리문으로 모든것을 가져올 수 있다.
<br>
아직 많이 사용해보진 않았지만 잘 사용하면 정말 유용하게 쓸 수 있을것 같다.
<br>

