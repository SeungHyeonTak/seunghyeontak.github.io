---
layout: post
title:  "Django_기본. Widget"
comments: true
description: "Django"
author: SeungHyeon Tak
date:   2019-06-20 10:30:00 +0700
categories: [Django]
tags: [Django]
keywords: "Django, Naver map"
---
#### Django.기본 웹 개발

#### Widget
* UI 입력 요소

* class Widget(attrs=None)
  * attrs : 해당 tag의 property 지정 사이즈나 타이틀과 같은 사전을 정의 해준다.
  * self.attrs를 통해 접근 가능

built-in widgets
* 텍스트 입력 - 한줄 입력박스, 입력제한, 제약들이 있음
* select/콤보 박스 - 체크박스같은 선택 할 수 있는 박스
* File upload - 파일 업로드 / 올린 파일에 대해서 삭제 가능함
* composite - 여러개의 히든을 적용 할 수 있으며, 데이터 타임을 자를 수 있는등등 많은 기능들이 있다.

#### 위젯 지정
* Widget클래스 혹은 Widget 인스턴스로 지정
방법1) Form Field 정의 시에 widget을 통해 지정

```python
class PostForm(forms.Form):
    lntlat = forms.CharField(max_length=50, widget=NaverMapPointWiget())
```

방법2) ModelForm Class의 Meta 내 widgets를 통해 widget만 변경

```python
class PostForm(forms.ModelForm):
    class Meta:
	widgets={
	    'lnglat': NaverMapPointWidget(), # 위젯을 재 정의할 수 있다.
	}
```

#### 커스텀 위젯
위젝을 변경한다고 해서, 서버로 전달되는 값이 변경되는 것은 아님 (단지 유저에게 UI의 편의성을 제공)

#### 네이버맵 경도/위도 입력 위젯 - NaverMapPointWidget

네이버 개발자 사이트에서 가입 및 설정을 마치고 ID/Key 값을 받는다.

* `최상위디렉토리/widgets/naver_map_point_widget.py`
그리고 __init__.py 를 만들어주기만 한다.

```python
import re
from django import forms
from django.conf import settings
from django.template.loader import render_to_string


class NaverMapPointWidget(forms.TextInput):
    BASE_LAT, BASE_LNG = '37.497921', '127.027636' # 강남역

    def render(self, name, value, attrs=None, renderer=None):
        width = str(self.attrs.get('width', 800))
        height = str(self.attrs.get('height', 600))

        if width.isdigit():
            width += 'px'

        if height.isdigit():
            height += 'px'

        context = { 'naver_client_id': settings.NAVER_CLIENT_ID,
                    'id': attrs['id'], # 현재 formfield의 html id
                    'width': width, 'height': height,
                    'base_lat': self.BASE_LAT, 'base_lng': self.BASE_LNG}
        if value:
            try:
                lng, lat = re.findall(r'[+-]?[\d\.]+', value)
                context.update({'base_lat': lat, 'base_lng': lng})
            except (IndexError, ValueError):
                pass

        attrs['readonly'] = 'readonly'
        parent_html = super().render(name, value, attrs)

        html = render_to_string('widgets/naver_map_point_widget.html', context)

        return parent_html + html
```
<br>
* `layout/widgets/naver_map_point_widgets.html`

<br>
![maps](https://user-images.githubusercontent.com/46446165/59721959-72f84d80-925d-11e9-83cd-21a8552662b2.png)
<br>

상단 script에 ncpClientId=nave_id 부분에 아까 받은 Client_id를 적어주면 된다.


