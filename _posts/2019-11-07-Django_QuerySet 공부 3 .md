---
layout: post
title:  "Django_queryset에 대한 이해 3"
comments: true
description: "Django, QuerySet"
author: SeungHyeon Tak
date:   2019-11-07 23:30:00 +0700
categories: [Django]
tags: [Django]
keywords: "Django, QuerySet"
---
#### Django - QuerySet 이해하기
<br>

ORM을 사용하면 SQL 데이터베이스를 손쉽게 다룰 수 있다.
하지만 직접 작성한 SQL에 비해 느리고 비 효율적이고 단점또한 있다.
<br>
ORM을 효과적으로 사용하는건 ORM Query를 어떻게 작성하는지 이해한다는 뜻이다.
<br>
<br>
#### Django는 지연연산이다.
Django의 QuerySet은 DB의 여러 레코드를 나타낸다.(필터링 또한 가능)
예로 한 반의 학생의 이름이 'Kim'인 모든 사람을 나타내는것을 Query로 나타내보자
<br>

```python

student_set = Student.objects.filter(first_name = 'Kim')

```

<br>
위의 코드는 어떤 쿼리에도 전달하지 않는다.
student_set에 어떠한 행동을 해도 DB에 아무런 메시지도 전달하지 않는다.
이로 인해 좋은점은 DB에 쿼리를 전달하는 일이 웹 애플리케이션을 느리게 하는 주범이기 때문이다.
<br>
DB의 레코드를 진짜로 가져오면 쿼리셋을 순회해야하기 때문이다.
예를 들어 순회하는 과정을 나타낼 수 있다.

<br>
```python
for student in student_set:
    print(student.last_name)
```

<br>
#### Django의 쿼리셋은 캐시된다.
이게 무슨말인가 하면,
쿼리셋을 순회하는 시점에 쿼리셋에 해당하는 DB의 레코드들을 실제로 가져오며, 
모두 Django 모델로 변환된다. 
이를 평가라고 하는데, 
평가된 모델들은 쿼리셋의 내장 캐시에 저장되며, 
덕분에 우리가 쿼리셋을 다시 순회하더라도 똑같은 쿼리를 DB에 다시 전달 하지 않는다.


```python
pet_set = Pet.objects.filter(species='Cat')

# 처음쓰는 쿼리에선 쿼리가 평가되고 결과는 캐시된다.
for pet in pet_set:
    print(pet.first_name)

# 다음에 똑같은 순회를 반복하게 될경우 이미 캐시된 쿼리셋이 사용된다.
for pet in pet_set:
    print(pet.last_name)
```

#### if문에서는 쿼리셋 평가가 발생난다.
쿼리셋의 캐시 기능은 특히, 
일단 쿼리셋에 레코드가 존재하는지를 검사한 후 하나라도 발견되었을 때만 
이를 순회하여 할 때 유용하다.


```python
restaurant_set = Restaurant.objects.filter(cuisine="Indian")

# if문은 쿼리셋을 평가한다.
if restaurant_set:
    for restaurant in restaurant_set:
	print(restaurant.name)
```


#### 결과 전체가 아닌 하나의 정보만 알고싶을때 쿼리셋 캐시가 문제됨
결과 전체를 순회하지 않고 단지 레코드에서 최소 하나의 정보가 존재하는지 알아보고싶을때가 있다.
이럴땐, if 문을 사용하면 if문 떄문에 쿼리셋이 평가 되고, 
이에 따라 쿼리셋 캐시에도 전체 레코드가 저장된다.


```python
city_set = City.objects.filter(name='Seoul')

# if 문은 쿼리셋을 평가한다.
if city_set:
    # 쿼리에 전체가 아닌 하나의 정보를 알고 싶은 상황인데도 불구하고
    # ORM은 전체 결과를 가져온다.
    print('Hello')
```


위 상황을 피하고 싶으면 어떤방법이 있는가?
`exists()`를 사용할 수 있다.
이 메서드는 최소한 하나의 레코드가 존재하는지 여부를 확인하여 알려주는 구문이다.


```python
tree_set = Tree.objects.filter(type='deciduous')

# exists()는 쿼리셋 캐시를 만들지 않으면서 레코드가 존재하는지 검사한다.
if tree_set.exists():
    # DB에서 가져온 레코드가 하나도 없다면 트래픽과 메모리를 절약할 수 있다
    print('Hello')
```

#### 우리가 사용하고자하는 쿼리가 엄청크다면? -> 쿼리셋 캐시가 문제가됨!
우리가 사용할 DB의 레코드 수 가 얼마나 많을지 모른다.
그러한 데이터를 한번에 가져와 메모리에 올리는 행위는 매우 비효율적이다.
더욱더 최악인 사실은 그 거대한 Query가 서버 프로세스를 잠궈 버려 
웹 어플리케이션이 죽을 수 있다는 사실이다.

쿼리셋 캐시를 방지하면서도 전체 레코드를 순회해야 한다면, iterator()를 사용하자,
이는 데이터를 작은 덩어리로 쪼개어 가져오고 이미 사용한 레코드는 메모리에서 지운다.


```python
star_set = Star.objects.all()

# iterator() 메서드는 전체 레코드의 일부씩만 DB에서 가져오므로
# 메모리를 절약할 수 있다.
for star in star_set.iterator():
    print(stat.name)
```


퀴리셋 캐시를 방지하고자 iterator() 메서드를 사용한 경우 똑같은 쿼리셋을 한 번 더 순회할 때 쿼리를 다시 평가한다.
따라서 iterator() 메서드를 사용하기 전, 
**우리의 코드에서 똑같은 거대 쿼리셋을 재사용하려고 하지 않는지 주의해야한다.**


#### 쿼리셋이 엄청 큰 경우 if도 문제가 될 수 있다.
쿼리셋 캐시와 if, for문을 조합하면 상황에 따라 쿼리셋을 순회하거나 하지 않을 수 있다.
하지만 엄청 큰 쿼리를 사용하려할땐 쿼리셋 캐시는 고려 사항이 될 수 없다.

* 간단한 해결책으로 `exists()`, `iterator()`를 함께 사용하면 두개의 쿼리를 실행하고 그 결과를 쿼리셋 캐시로 생성하는 일을 방지할 수 있다.


```python
molecule_set = Molecule.objects.all()

# 첫번째 쿼리로, 쿼리셋에 레코드가 존재하는지 확인한다.
if molecule_set.exists():
    # 또 다른 쿼리로 레코드를 조금씩 가져온다.
    for molecule in molecule_set.iterator():
	print(molecule.velocity)
```


<br>
#### Query 다시한번 되짚어보기
* all()
테이블 데이터를 전부 가져오기 위해서 사용된다.
query의 성능을 위해서라면 잘 안쓰는것이 좋겠다.


```python
Post.objects.all()
# Post에 저장된 모든 데이터를 가져온다.
```

* get()
하나의 Row만을 가져오기 위해서 get() 메서드를 활용 할 수 있다.


```python
Post.objects.get(pk=1)
# id 컬럼이 1인 row를 가져온다.
```


* filter()
Query에 주어진 매개 변수와 일치하는 객체를 반환하는것(특정 조건에 맞는 Row들만 가져온다)
본인이 필요로 하는 매개변수들만 따로 걸러서 확인 할 수 있다.


```python
Post.objects.filter(title='python')
# python에 관한 포스팅 제목만 출력된다.
```

* exclude()
특정 조건을 제외한 나머지 Row들을 가져오기 위해서는 exclude() 메서드를 사용함.
예를 들어 title 필드에서 여행이 아닌 데이터만 가져오고 싶을때


```python
Post.objects.exclude(title='여행', name='뜬금이')
# 여행 이라는 title과 뜬금이라는 name을 가진 필드는 빼고 다 가져온다.
```


* count()
데이터의 갯수를 세기 위해 count()메서드를 사용한다.


```python
Post.objects.count()
```

* order_by()
데이터를 키에 따라 정렬하기 위해 order_by() 메서드를 사용한다. order_by()안에는 정렬 키를 나열할 수 있는데, 앞에 `-`가 붙으면 내림 차순이다. 물론 `-`가 없으면 오름차순이다.


```python
Post.objects.order_by('-id')
# id값을 내림차순으로 나열
```

* distinct()
중복된 값은 하나로만 표시하기 위해 distinct()메서드를 사용한다.
예를 들어 name 필드가 중복되는 경우 한번만 표시하게 된다.


```python
Post.objects.distinct('name')
# 같은 이름이 여러개일경우 한번만 표시하게 해준다.
```


* first()
데이터들 중 처음에 있는 row만을 리턴한다.


```python
Post.objects.order_by('name').first()
# 가장 처음에 있는 name필드만 리턴한다.
```


* last()
데이터들중 마지막에 있는 row만 리턴한다.


```python
Post.objects.order_by('name').last()
# 가장 마지막에 있는 name필드만 리턴한다.
```

* annotae()
객체와 관련된 객체에 대해 계산된 집계 표현식(평군, 합계)이다.
예를들면

```python
from django.db.models import Count

Post.objects.annotate(Count('entry'))
# Post에서 작성된 항목수를 판별 할 수 있게 도와준다. 
```

자세히 말하자면
특정 창고에서 받은 발송장과 보낸 발송장을 참고하여 물건의 남은 양을 파악하고 있다.
이것을 가독성과 유지보수성을 위해 Query로 짜보겠다.
아 참고로 `annotate()`는 `Case`,`Value`,`When`을 사용할 수 있다.
`Case`는 if-elif-else와 비슷하다

```python
from django.db.models import Case, When # 사용하기 위해 선언 해줘야한다.

Case(
    When(
        invoice__dest=inventory, # 발송장의 수신 = 특정 창고
	then='stock_entry__quantity' # 물품목록에 포함된 개수(발송 개수)
    ),
    default=0 # 해당되는 쿼리셋이 없다면 0을 반환
)
```

이제 Case문을 실제 모델에서 작성해보자


```python
stock_list = Stock.objects.annotate(
    received_stocks=Sum(
	Case(
	    When(
		stock_entry__invoice__dest=inventory,
		then='stock_entry__quantity'
	    ),
	    default=Value(0)
	)
    )
)
```


* reverse()
`reverse()`메소드를 사용하여 쿼리 세트의 요소가 리턴되는 순서를 반대로 리턴한다.

```python
my_querset.reverse()[:5]
# 끝에서 슬라이싱은 지원하지 않는다.
```

참고로 말하자면 주어진 순서가 정의되어 있지 않으면 효과가 없다.
그래서 order_by()나 순서로 정렬되어있을때만 호출해야 한다.


* values()
모델 객체의 속성 이름에 해당하는 키와 객체를 나타낸다.
원하는 컬럼만 가져오기위해 사용할 수 있음
사용하면 해당 컬럼의 key,value의 쌍의 리스트를 얻을 수 있다.

```python
Post.objects.filter(name='lee').values()
# queryset으로 lee라는 이름을 가진 필드에 대한 키와 값이 포함되어서 나타남

Post.objects.values('id','name')
# 이렇게 되면 id와 name의 값만 보여진다.
```


* values_list()
values()는 사전을 반환하는 대신 values_list()는 튜플(tuples)의 형태 리스트로 가져온다.


```python
Post.objects.values_list('id','title')
# [(1,'python book')]
```


여기서 `flat`을 사용하면 tuples 형태가 아닌 값의 리스트의 형태로 가져올 수 있다.


```python
Post.objects.values_list('id').order_by('id')
# [(1,),(2,),(3,),...]

Post.objects.values_list('id', flat=True).order_by('id')
# [1,2,3,...]
```

(작성중...)

**update**
우리가 데이터를 생성할줄 알면 당연히 수정도 할줄 알아야한다.
수정하기 위해서는 먼저 수정할 Row 객체를 얻은 후 변경할 필드들을 수정한다.
마지막에 save()메서드를 호출하면 테이블에 데이터가 갱신된다.


```python
post = Post.objects.get(pk=1)
post.name = '파이썬'
post.save()
```

**delete**
데이터를 삭제하기 위해서는 먼저 삭제할 Row 객체를 얻은 후 delete() 메서드를 호출하면 된다.


```python
post = Post.objects.get(pk=2)
post.delete()
```
