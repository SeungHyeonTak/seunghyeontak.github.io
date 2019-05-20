---
layout: post
title:  "Django_24.(설문조사 만들기) admin_customize"
comments: true
description: "Django_part24"
author: SeungHyeon Tak
date:   2019-05-09 11:21:12 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.24 웹 개발

##### admin customize

* Django admin은 기본으로 제공 되있는 앱이다.

`settings.py`

```python
INSTALLED_APPS = [
	'django.contrib.admin', # 이 내용이 확인되면 제공되있는거다.
]
```

지금 admin 페이지에 접속해보면 POLLS에 Choice 와 Questions가 있다.
이것을 customize을 하기 위해 코드를 변경 해볼것이다.

`admin.py` 로가서 수정해보기

`fields 써보기`

```python
class Question(admin.ModelAdmin):
	fields = ['pub_date', 'question_text'] # 순서가 바뀌게 나올것이다.
# 이렇게 입력하고 등록을 해줘야 정상적으로 보여진다.
admin.site.register(Question, QuestionAdmin) #이렇게 추가를 해줘야한다.
# 그리고 runserver로 확인해보자
```

`fieldsets 써보기`

```python
class Question(admin.ModelAdmin):
	fieldsets = [
		(None, {'fields':['question_text']}), # 맨앞에 None은 제목이 들어갈 자리
		('Date Information', {'fields': ['pub_date']}), #중간의 소제목
	]
```

Choice 클래스를 한꺼번에 입력받고 싶을때 `Inline`이라는 기능을 이용할 수 있다.

```python
class ChoiceInline(admin.StackedInline):
    model = Choice # 어떤 모델을 쓸건지
    extra = 2 # 한번에 몇개씩 추가할건지
# 위의 내용을 Question에 붙인다.

class Question(admin.ModelAdmin):
	...
	inlines = [ChoiceInline] # 위에서 만들었던 클래스를 이렇게 붙인다.
```

* StackedInline 보다 더 조밀하게 보일 수 있는 TabularInline을 쓸 수 있다.

```python
class ChoiceInline(admin.TabularInline):
    model = Choice
    extra = 1
```

* 다음은 Data Information을 숨기거나 보여질 수 있게 펼치고 접기를 구현 해볼거다.

```python
# collapse 라는 기능을 사용해본다.
class Question(admin.ModelAdmin):
	fieldsets = [
		(None, {'fields':['question_text']}),
		('Date Information', {'fields': ['pub_date'],'classes':['collapse']}),
	]
# 이렇게 collapse를 추가 하면 숨김 펼치기를 할 수 있다.
```

* Question의 목록 부분에 가면 지금은 제목만 보이는데 이를 추가적으로 보여주고 싶을때

```python
# list_display를 쓴다
class Question(admin.ModelAdmin):
	fieldsets = [
		(None, {'fields':['question_text']}),
		('Date Information', {'fields': ['pub_date'],'classes':['collapse']}),
	]
	inlines = [ChoiceInline]
	list_display=('question_text', 'pub_date', 'was_published_rescently')
	# display할 리스트를 선택한다. / 튜플로 진행하며 컬럼을 써준다. / 컬럼이 아니여도 메서드도 쓸 수 있다.(리스트 마지막 부분이 메서드를 써준것이다.)
```

* 정렬 기능 추가 구현

`models.py`에서 구현
```python
	class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')

    def __str__(self):
        return self.question_text

    def was_published_recently(self):
        now = timezone.now()
        return now - datetime.timedelta(days = 1) <= self.pub_date <= now 
    # 밑에 3줄 부분 추가 하여 정렬 기능 구현
    was_published_recently.admin_order_field = 'pub_date'
    # 제공 되는 메서드는 기본적으로 정렬 기능이 없는데,
    # admin.order_field를 쓰면 정렬기능이 구현된다.
    was_published_recently.boolean = True
    was_published_recently.short_description = 'published recently?'
```

* 리스트의 필터기능과 검색기능 구현하기

다시 `admin.py`로 와서

```python
class QuestionAdmin(admin.ModelAdmin):
	...
	list_filter = ['pub_date']
	# 날짜 타입이라서 자동으로 관련 필터를 생성함
	search_fields = ['question_text']
	# 검색창이 구현됨
```
