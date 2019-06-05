---
layout: post
title:  "Django_기본.Django Templates"
comments: true
description: "Django"
author: SeungHyeon Tak
date:   2019-06-05 21:08:11 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.기본 웹 개발

#### 템플릿 사용하는 이유
* 코드만으로 직접 복잡한 문자열을 조합하기 어려움
* 복잡한 문자열을 편리하게 조합할 수 있게해줌

* 템플릿 엔진 활용 주요코드
  * django.shortcuts.render -> HttpResponse
  * django.temlate.loader.render_to_string -> str

##### CBV에서의 템플릿 처리
* django.template.reponse.SimpleTemplateResponse 클래스에서 로직 구현
  * 특정 CBV에서 template_name 인자 지정을 통한, 템플릿 지정
  * CBV 종류에 따라, 모델 이름을 따라 template_name 자동 지정
     * Listview에서의 Post 모델 -> "앱이름/post_list.html"
     * 각 CBV마다의 기본 템플릿 경로를 활용하면 코드를 줄일 수 있다.

#### settings.TEMPLATES 설정 리스트
* BACKEND : 템플릿 엔진 지정
* DIRS : 템플릿을 둘 디렉토리 경로 리스트
* APP_DIRS : 앱별 templates 경로 추가 여부


#### 최소한의 템플릿 settings
* 각 앱들을 위한 템플릿은 각 "앱/templates/" 경로에 배치하여야 함
  * 이유: 장고 앱은 재사용성에 포커스가 맞춰져 있기 때문에
* 프로젝트 전반적으로 사용할 템플릿은 DIRS에 명시한 경로에 배치

```python
TEMPLATES = [
	{
		'BACKEND': 'django.template.backends.django.DjangoTemplates',
		'DIRS': [
			os.path.join(BASE_DIR, '프로젝트디렉토리명', 'templates'),
		],
		'APP_DIRS': True,
		'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
			],
		},
	},
]
```

<br>

#### Django Templates Tag/filter
* 유틸리티 성격의 장고 템플릿 내에서 호출할 수 있는 함수 목록들
  * Django Template Tag : "{""%" 태그명 "인자1" "인자2"와 같은 방식으로 호출
  * Django Template Filter : "{""{"값|필터1:인자|필터2:인자|필터3와 같은 방식으로 호출
<br>
#### 디폴트 에러 템플릿 경로
* django/views/defaults.py 내에서 다음경로 지정
  "400.html","403.html","404.html","500.html"
  실제 서비스에서는 구현을 권장함
