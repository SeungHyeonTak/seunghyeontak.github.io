---
layout: post
title:  "Django_28.QuerySet"
comments: true
description: "Django_part28"
author: SeungHyeon Tak
date:   2019-05-13 21:46:00 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.28 웹 개발
<br>
<br>

####QuerySet

- SQL을 생성해주는 인터페이스
- queryset을 통하여 별도로 SQL을 작성할 필요 없이 DB로 부터 데이터를 가져오고 추가, 수정, 삭제가 가능함

```
documents = Document.objects.all() # SELECT * FROM documents와 같은 SQL문 생성
ex) documents = Document.objects.create() # INSERT INTO documents VALUES(...)와 같은 SQL문 생성 
return render(request, 'board/document_list.html', {'object_list':documents}) # context_value를 가져옴 {}-dic형태로 가져올 수 있다.
```
