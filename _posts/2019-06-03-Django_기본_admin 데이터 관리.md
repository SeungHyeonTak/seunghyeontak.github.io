---
layout: post
title:  "Django_기본.admin을 통한 데이터 관리"
comments: true
description: "Django"
author: SeungHyeon Tak
date:   2019-06-03 23:15:11 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.기본 웹 개발

* django admin
  * django.contrib.admin 앱을 통해 제공
  * 모델 클래스 등록을 통해 (조회/추가/수정/삭제) 웹 UI를 제공해줌
  * 내부적으로 Django Form을 적극적으로 사용가능

* admin.py에 등록하는법 3가지

```python
# 방법 1
admin.site.register(Item)

# 방법 2
class ItemAdmin(admin.ModelAdmin):
	pass
admin.site.register(Item, ItemAdmin)

# 방법 3
@admin.register(Item)
class ItemAdmin(admin.ModelAdmin):
	pass

# 요즘에는 방법3을 가장 많이 쓴다고 함
```
<br>

* admin페이지로 들어가면 우리가 지정한 값이 그대로 보이지 않을것이다.
* 그것을 변경해주기 위해 str을 쓴다.

```python
# models.py
class Item(models.Modle):
	...
	def __str__(self):
		return self.name
		
# 이런식으로 써주면 관리자 페이지에서 가독성 좋게 확인할 수 있다.
```

* list_display 속성 정의
  * 모델 리스트에 출력할 컬럼 지정

```python
# admin.py
@admin.register(Item)
class ItemAdmin(admin.ModelAdmin):
	list_display = ['pk', 'name', 'short_desc', 'proce', 'is_publish']
	
	# 밑의 함수 부분은 models.py에서 따로 지정 해주지 않았지만 admin에서 직접 설정 해줄 수 도 있다.
	def short_desc(self, item):
		return item.desc[:20]
```

* list_display_link & search_fields & list_filter속성 정의

```python
# admin.py
@admin.register(Item)
class ItemAdmin(admin.ModelAdmin):
	list_display = ['pk', 'name', 'short_desc', 'proce', 'is_publish']
	list_display_links = ['name'] # name에 링크를 걸어 name부분을 클릭하여 내부를 볼 수 있게 설정 가능하다.
	search_field = ['name'] # search 말그대로 검색을 할 수 있게 도와주는 속성 필드이다. name 부분에 일부분을 입력하면 그 내용을 가진 name만 검색된다.
	list_filter = ['is_publish'] # 이 부분은 직접 실행해보면 오른쪽 부분에 따로 filter 부분이 생성된다. (직접 보고 확인 해보는게 빠를듯 하다)
	def short_desc(self, item):
		return item.desc[:20]
```