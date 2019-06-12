---
layout: post
title:  "Django_기본.Form만들기"
comments: true
description: "Django"
author: SeungHyeon Tak
date:   2019-06-02 21:56:00 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.기본 웹 개발

#### 장고 Form

* Model클래스와 유사하게 Form클래스 정의
* 주요 역할 : 커스텀 Form클래스를 통해
  * 입력폼 HTML생성 : as_table(), as._p(), as_ul() 기본 제공
  * 입력폼 값 검증 (validation) 및 값 변환
   (입력에 맞게 정해줬는지 체크를 할 수 있다)
  * 검증을 통과한 값들을 사전타입으로 제공
   (사전타입 - cleadned_data)

**********
GET과POST의 차이에 대해
GET과 POST는 HTTP프로토콜을 이용해서 서버에 무언가를 전달할 때 사용하는 방식이다.
GET과 POST의 차이에 대해 알아보기 전에 쉽게 전혀 모르는 사람이 이해할 수 있게 글을 적어본다.
약간의 이해를 돕기 위해
* GET은 주소(즉,url)에 값들이 ?뒤에 이어붙어있고 POST는 숨김상태로 보내진다.
  * GET은 이어붙여져서 가기 때문에 길이제한이 있어 많은 양의 데이터를 보내지는 못한다고 한다.
  * POST는 많은 양을 보내도 적합하다고 한다(GET보다는 많지만 용량 제한은 있음)
* Django를 배우고 계신 분들에게 맞춰서
http://naver.com/list.html?id=a&page=3 의 방식이 GET방식이고
form을 이용해 submit하는 형태가 POST방식이다.
그러므로 GET은 가져오는것이고 POST는 수행하는것임을 알 수 있게 됩니다.
**********

* GET방식으로 요청될 때
  * 입력폼을 보여준다.
* POST방식으로 요청될 때
  * 데이터를 입력받아 유효성 검증 과정을 거친다.(validation)
  * 검증 성공시 : 해당 데이터를 저장하고 SUCCESS URL로 이동
  * 검증 실패시 : 오류메세지와 함께 입력폼을 다시 보여준다.


#### *Form 써보기*
1. 일단 models.py에 있는 정보로 form을 작성한다.
2. forms.py를 생성 후 내용 작성

```python
from django import forms
from .models import Dojo

def min_length_3_validator(value):
    if len(value) < 3:
        raise forms.ValidationError('3글자 이상 입력해주세요.')


class DojoForm(forms.Form):
    title = forms.CharField(validators=[min_length_3_validator])
    content = forms.CharField(widget=forms.Textarea)

    def save(self, commit=True):
        dojo = Dojo(**self.cleaned_data)
        if commit:
            dojo.save()
        return dojo

'''
여기서 궁금한점?
모델과 폼의 차이에 대해서
content type을 모델에선 TextField였는데 form에선 CharField로 준다?
 - 모델은 데이터베이스에 대한 인터페이싱 즉, 필드 타입(길이제한이 있는, 없는 문자열로 나누어진다.)
Type은 같지만 위젯은 다르게 보여질 수 있다.
아직 확실히 정리 되진 않았지만 widget을 써주면 된다고 함
'''
```

3. urls.py에서 url경로 생성
4. views.py 작성

```python
from django.shortcuts import render,redirect
from .models import Dojo
from .forms import DojoForm


def dojo_list(request):
    if request.method == "POST":
	# POST / FILES 는 이 순서대로 써줘야함
	# FILES는 해당 데이터를 쓸경우엔 꼭 넣어줘야함
        form = DojoForm(request.POST,request.FILES)
        if form.is_valid():
            # 방법1
            # dojo = Dojo()
            # dojo.title = form.cleaned_data['title']
            # dojo.content = form.cleaned_data['content']
            # dojo.save()

            # 방법2
            # dojo = Dojo(title=form.cleaned_data['title'],
            #             content=form.cleaned_data['content'])
            # dojo.save()

            # 방법3
            # dojo = Dojo.objects.create(title=form.cleaned_data['title'],
            #                            content=form.cleaned_data['content'])
            # dojo.save()

            # 방법4
            # dojo = Dojo.objects.create(**form.cleaned_data) # DB에 저장
            Dojo=form.save()
            return redirect('/')
        else:
            form.errors
    else:
        form = DojoForm()

    return render(request,'dojo/dojo_form.html',{'form':form})

# 여러 방법 중 본인이 사용하기 편한 방법을 쓰면됨
```

5. templates 에 dojo_form.html을 생성하여 내용 추가
<br>
![12311](https://user-images.githubusercontent.com/46446165/59342584-08b74880-8d45-11e9-9e18-1b6ea6c88cd1.png)

6. admin.py에서 register를 생성하여 값이 잘 저장되는지 확인
7. runserver로 사이트 열어서 확인 값 입력 후 submit으로 넘기면 admin 사이트에 저장되나 확인


#### Form Fields
* Model Field와 비슷하다
  * Model Fields - Database Field를 파이썬 클래스 화
  * Form Fields - HTML Form Field를 파이썬 클래스 화

#### Form의 큰 흐름
<br>
![form1](https://user-images.githubusercontent.com/46446165/59144569-dcce5700-8a14-11e9-97bc-792448995a7d.png)
