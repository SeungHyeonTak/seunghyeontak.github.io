---
layout: post
title:  "Django_queryset에 대한 이해 2"
comments: true
description: "Django, SQLquery, ORM"
author: SeungHyeon Tak
date:   2019-11-05 21:30:33 +0700
categories: [Django]
tags: [Django]
keywords: "Django"
---
#### Django ORM / SQLquery 정리

<br>
> 만들어볼 DB 간단히 modeling 해보기

<br>
```python
class Site(models.Model):
    name = models.CharField(max_length=255,)

class Category(models.Model):
    name = models.CharField(max_length=255,)

class Article(models.Model):
    subject = models.CharField(max_length=255,)
    url = models.URLField(unique=True,)
    date = models.DateTimeField()
    site = models.ForeignKey(Site)
    category = models.ManyToManyField(Category)
    hit = models.IntegerField(default=0)
```

<br>

```text
CREATE TABLE `web_site` (
  `id` INT(11) NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(255) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=0

CREATE TABLE `web_category` (
  `id` INT(11) NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(255) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=INNODB AUTO_INCREMENT=0

CREATE TABLE `web_article` (
  `id` INT(11) NOT NULL AUTO_INCREMENT,
  `subject` VARCHAR(255) NOT NULL,
  `url` VARCHAR(200) NOT NULL,
  `date` DATETIME(6) NOT NULL,
  `site_id` INT(11) NOT NULL,
  `hit` INT(11) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `url` (`url`),
  KEY `web_article_site_id_8dbc8100_fk_web_site_id` (`site_id`),
  CONSTRAINT `web_article_site_id_8dbc8100_fk_web_site_id` FOREIGN KEY (`site_id`) REFERENCES `web_site` (`id`)
) ENGINE=INNODB AUTO_INCREMENT=0

CREATE TABLE `web_article_category` (
  `id` INT(11) NOT NULL AUTO_INCREMENT,
  `article_id` INT(11) NOT NULL,
  `category_id` INT(11) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `web_article_category_article_id_9ea0efa6_uniq` (`article_id`,`category_id`),
  KEY `web_article_category_category_id_d2d88408_fk_web_category_id` (`category_id`),
  CONSTRAINT `web_article_category_article_id_5b4eba1c_fk_web_article_id` FOREIGN KEY (`article_id`) REFERENCES `web_article` (`id`),
  CONSTRAINT `web_article_category_category_id_d2d88408_fk_web_category_id` FOREIGN KEY (`category_id`) REFERENCES `web_category` (`id`)
) ENGINE=INNODB AUTO_INCREMENT=0
```

<br>

#### SELECT
<br>

> ALL <br>

```
SQL
SELECT * FROM web_article;

ORM
Aticle.objects.filter(id=1)
```

*****

> WHERE <br>

```
SQL
SELECT * FROM web_article WHERE id=1;

ORM
Article.objects.filter(id=1)
```

*****

> LIMIT n <br>

```
SQL
SELECT * FROM web_article LIMIT 10;

ORM
Article.objects.all()[:10]
```

*****

> LIMIT n,n <br>

```
SQL
SELECT * FROM web_article LIMIT 5,5;

ORM
Article.objects.all()[5:10]
```

*****

> fetchone <br>

```
SQL
SELECT * FROM web_article WHERE id=1;

ORM
Article.objects.get(id=1)
```

*****

> fetchall <br>

```
SQL
SELECT * FROM web_article WHERE site_id=1;

ORM
Article.objects.filter(site_id=1)
```

*****

> AND <br>

```
SQL
SELECT * FROM web_article WHERE site_id=1 AND hit=0;

ORM
Article.objects.filter(site_id=1, hit=0)
```

*****

> OR <br>

```
SQL
SELECT * FROM web_article WHERE site_id=1 OR hit=0;

ORM
from django.db.models import Q
Article.objects.filter(Q(site_id=1)|Q(hit=0))
```

*****

> LIKE '%s%' <br>

```
SQL
SELECT * FROM web_article WHERE subject LIKE '%선생님%'

ORM
Article.objects.filter(subject__icontains='선생님')
```

*****

> LIKE 's%' <br>

```
SQL
SELECT * FROM web_article WHERE SUBJECT LIKE '홍길동%';

ORM
Article.objects.filter(subject__startswith='홍길동')
```

*****

> LIKE '%s' <br>

```
SQL
SELECT * FROM web_article WHERE SUBJECT LIKE '%의혹';

ORM
Article.objects.filter(subject__endswith='의혹')
```

*****

> `>=` 크거나 같다 <br>

```
SQL
SELECT * FROM web_article WHERE hit >= 2;

ORM
Article.objects.filter(hit__gte=2)
```

*****

> `<=` 작거나 같다. <br>

```
SQL
SELECT * FROM web_article WHERE hit <= 2;

ORM
Article.objects.filter(hit__lte=2)
```

*****

> `>` 크다 <br>

```
SQL
SELECT * FROM web_article WHERE hit > 1;

ORM
Article.objects.filter(hit__gt=1)
```

*****

#### INSERT

> 집어넣기 <br>

```
SQL
INSERT INTO web_site SET name='뉴스타파';

ORM
site = Site(name='뉴스타파')
site.save()
```

****

> 있으면 가져오고 없으면 집어 넣기 <br>

```
SQL
INSERT INTO web_site SET name='한겨레';

ORM
site = Site.objects.get_or_create(name='한겨레')
# save 메서드 호출 없어도 바로 입력됨
```

*****

> ForeignKey / ManyToManyField <br>

```
ORM
site, created = Site.objects.get_or_create(name='PPSS')
article = Article(subject='뉴스제목', url='http://news.com', date='2019-11-05 12:34:56', site=site)
article.save()
cate1, created = Category.objects.get_or_create(name='정치')
cate2, created = Category.objects.get_or_create(name='뉴스')
acticle.category.add(cate1)
acticle.category.add(cate2)
acticle.category.remove(cate2)
acticle.category.add(cate2)
```

*****

#### DELETE

```
ORM
Article.objects.get(id=2).delete()
```

*****

#### UPDATE

```
ORM
article = Article.objects.get(id=4)
article.subject = '제목변경'
article.save() 
```
<br>
