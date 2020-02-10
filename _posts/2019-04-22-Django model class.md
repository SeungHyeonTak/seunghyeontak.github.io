---
layout: post
title:  "Django model"
comments: true
description: "Django model 작성하는 법"
author: SeungHyeon Tak
date:   2019-04-22 23:59:30 +0700
categories: [Django]
tags: [Django]
keywords: "Django model 작성하기"
---
## Django model class

*Django - model class 만들기*

* Django에서 model은 DB에 데이터를 저장하는 역할을 합니다.
* MTV에서 M(model)을 담당하고 있습니다.
* Model은 DB에서 데이터를 저장하거나 불러오기 위해 Model을 사용합니다.
* 그리고 models.py에서 클래스를 만들어주고 난후 Django에게 알려주는 작업을 해야합니다.

* models.py를 작성 안하고 실행시키면 아무 변화 없다고 나올겁니다.

```
$ python manage.py makemigtrations
$ python manage.py migrate
```

※ 일단 위를 실행하기전에 models.py를 작성해보겠습니다.

```
class Numbers(models.Model):
	name = models.CharField(max_length = 24)
    	lottos = models.CharField(max_length = 255,default = '[1,2,3,4,5,6]')
    	text = models.CharField(max_length = 255) # text는 한줄 메모장 처럼 간단하게
    	num_lotto = models.IntegerField(default=5) # 게임 수
    	update_data = models.DateTimeField()

	def generate(self):
        	self.lottos = ""
        	origin = list(range(1,46))
        	for _ in range(0,self.num_lotto):
            		random.shuffle(origin)
            		guess = origin[:6]
            		guess.sort()
            		self.lottos += str(guess) + '\n'
        	self.update_data = timezone.now()
        	self.save() # 객체를 DB에 저장

```
* 필드 타입
model class 는 Field를 정의하기 위해 instance 변수가 아닌 class 변수를 사용한다.
왜냐하면 이 변수들이 테이블 필드 내용이 아니라 테이블 칼럼 메타 데이터를 정의하기 때문이다.
그래서 Field를 정의하는 각각 class 변수들은 타입에 맞는 Field class 객체를 생성하여 할당 한다.

![Fieldtype](https://user-images.githubusercontent.com/46446165/57124044-17622580-6dbf-11e9-97a0-6b9128e35f91.png)

자세한 내용은 여기서 확인할 수 있습니다.
<https://docs.djangoproject.com/en/1.11/ref/models/fields/#field-types>

* 필드 옵션
model의 필드는 필드 타입에 따라 여러 옵션을 가질수 있는데, 예를 들어서 CharField는 문자열 최대 길이를 의미하는 max_length라는 옵션을 갖는것 처럼 다른 필드들도 여러 옵션을 가질 수 있다. 
자세한 내용은 여기서 확인할 수 있습니다.
<https://docs.djangoproject.com/es/1.11/ref/models/fields/#module-django.db.models.fields.related>


#### 참고

이후 많은 프로젝트가 커지고 나선 위에서 한 방법 처럼 makemigration하는 방법은 안좋은 영향을 미칠수 있기에 해당 작업을 한 app-name만 따로 migrations 해주는게 효율적이다. 

아무리 복잡한 폴더를 타고 있어도 migrations 폴더가 담겨져 있는 상위 폴더만 app-name에 적어주면 된다.