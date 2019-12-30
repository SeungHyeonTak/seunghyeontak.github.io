---
layout: post
title:  "Django admin site에 model등록"
comments: true
description: "Django admin"
author: SeungHyeon Tak
date:   2019-04-23 09:30:31 +0700
categories: [Django]
tags: [Django]
keywords: "Django admin"
---
#### Django admin

models.py에서 작성을 하였다면 이제 확인해볼 차례입니다.
http://127.0.0.1:8000/admin/ 으로 접속하면 
관리자 페이지가 하나 뜰건데 여기에 접속하기 위해선 Id와Pw가 필요 합니다.
앞서 만들었으면 그 계정으로 로그인 해도 되지만 
생성하지 않았으면

```
$ python3 manage.py createsuperuser
```

를 만들어 id와 pw를 생성 해줍니다.
email 부분은 그냥 Enter로 넘겨줄 수 있습니다.

우리가 만든 Numbers가 안보일겁니다.
이를 위해서 admin.py에서 Numbers 모델 클래스를 등록 합니다.

```
from .models import Numbers

admin.site.register(Numbers)
```

모델클래스를 등록 후 admin 사이트로 재접속 하면 등록된 모델 클래스를 확인 할 수 있습니다.
그리고 오른쪽 상단에 `NUMBERS 추가 +` 를 누르고 입력 할 부분 입력하고 저장을 누르면 추가된 정보 확인 가능

그리고 저장한 내용 표시가 된다.

![admin_s](https://user-images.githubusercontent.com/46446165/57124284-25647600-6dc0-11e9-9411-f9a5779a9a3e.png)


좀 더 이쁘게 표시하려면 models.py로 가서 __str__ 메소드를 추가하는 방법이 있다.

```
def __str__(self):
	return "%s %s" %(self.name, self.text)
```

를 추가 해주면 설정한 name과 text를 볼 수 있다.
