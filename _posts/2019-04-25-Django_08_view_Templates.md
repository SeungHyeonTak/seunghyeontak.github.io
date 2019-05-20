---
layout: post
title:  "Django_08.(로또만들기)view와Templates 연동"
comments: true
description: "Django_part08"
author: SeungHyeon Tak
date:   2019-04-25 20:30:00 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django.08 웹 개발

* app 폴더 urls.py 수정

```
urlspatterns = [
	...
	path('mysite/',views.index, name = index), # 이렇게 수정
]
```

* url을 수정했으니 views.py도 수정해준다.

```
def index(request):
	return render(request, 'mysite/default.html', {})
	# Templates 파일 경로 지정을 해준다.
```

* 템플릿 폴더 생성 방법
   * `mysite/templates/mysite/default.html` (템플릿 밑에 mysite 부분은 자신이 생성한 app 이름과 똑같이 해주어야한다.)
그리고 서버를 실행시키고 /mysite/ 를 붙여 접속하면 템플릿과 연동된 화면을 볼 수 있다.

`default.html` 내용 (bootstrap을 활용함 - 본인이 마음에 드는 디자인으로 변경 가능)

<https://github.com/SeungHyeonTak/HTML-study/blob/master/default.html>

* 정적 파일 연동
* static 파일 생성
   * `mysite/static/css/lotto.css` 로 경로 설정

`lotto.css` (css 디자인 변경 가능)

<https://github.com/SeungHyeonTak/CSS-study/blob/master/lotto.css>

css 내용추가 후 html 파일로 가서 연동 시켜줘야한다.

![aa](https://user-images.githubusercontent.com/46446165/57191731-ac9e1f00-6f63-11e9-8537-95fa241c55f0.png)


마지막으로 Django에서 static 파일이 생겼다는걸 알려주는 명령어

```
$ python manage.py collectstatic
```
