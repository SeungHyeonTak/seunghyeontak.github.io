---
layout: post
title:  "Django Extensions로 ERD 생성"
comments: true
description: "Django, Django-extensions"
author: SeungHyeon Tak
date:   2020-02-06 18:17:00 +0700
categories: [Django]
tags: [Django]
keywords: "Django, Django-extensions"
---
## Django extensions를 이용해서 ERD 만들기

### Django extensions install

pip를 이용해 django-extensions를 설치한다.

```!bash
$ pip install django-extensions
```

settings.py의 INSTALLED_APPS에 `django_extensions`를 넣어준다.

```python
INSTALLED_APPS = {
	...
	'django_extensions',
}
```

그룹 모델 지정하기

```python
# settings.py
GPAPH_MODELS = {
	'all_applications': True,
	'group_model': True,
}
```

기본적으로 Django-extensions에 있는 graph model 기능을 이용해 dot파일을 생성할 수 있다.

`$ ./manage.py graph_models -a > my_project.dot`

이렇게 하면 보기 불편하게 나올것이다.

그레서 좀 더 보기 좋게 하여면 `graphviz`설치가 필요하다.

#### graphviz

일단, brew를 통한 graphviz설치를 진행 해야한다.

`$ brew install graphviz`

이후 설치가 완료된 후 `$ pip install pygraphviz`

`$ ./manage.py graph_models -a -g -o my_project_visualized.png`

를 하면 django project 안에 한개의 png파일이 생성되며 우리가 알던 ERD화면으로 볼 수 있게 된다.

또한, 원하는 모델만 출력 가능하다.

`$ ./manage.py graph_models -a -I User,Company,WFmall -o my_project_want_model.png`

로 생성 가능하다.

