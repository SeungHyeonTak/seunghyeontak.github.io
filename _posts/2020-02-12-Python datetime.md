---
layout: post
title:  "python datetime"
comments: true
description: "Python datetime"
author: SeungHyeon Tak
date:   2020-02-12 16:56:00 +0700
categories: [python]
tags: [python]
keywords: "python datetime"
---
## python datetime

오늘 날짜를 나타내고 싶을때

```python
import datetime

# 오늘 날짜 시간 분 초 까지 나타냄
now = datetime.datetime.now() # type: datetime.datetime
# 2020-02-12 16:58:08.968836

# 가끔 datetime.date 형식으로 써야할때가 있는데 이땐 .now().date()를 써주면 된다.

# 년-월-일만 알고 싶을때
nowDate = now.strftime('%Y-%m-%d')
# 2020-02-12

# 시간만 알고 싶을때
nowTime = now.strftime('%H:%M:%S')
# 16:58:08

# 깔끔하게 년월일 시간만 출력
nowDateTime = now.strftime('%Y-%m-%d %H:%M:%S')
# 2020-02-12 16-58-08
```

#### 시간 계산

```python
from datetime import datetime, timedelta

# 현재 시간
now = datetime.now()

# 지정하려는 시간
time = datetime(2020, 1, 1, 17, 3)

# 시간 계산하기 큰 날짜에서 부터 작은 날짜를 빼야 원하는 시간이 나옴
print(now-time)
# ex) datetime.timedelta(41, 85907, 825387)

# 두 날짜의 차
print((now-time).days, '일')
print((now-time).seconds, '초')

print((now-time).seconds / 3600, '시간')

# 특정 날짜로 부터 계산하는 법

# 현재 시간부터 5일 뒤
print(time + timedelta(days=5))

# 현재 시간부터 3일 전
print(time + timedelta(days=-3))

# 현재 시간부터 1일 뒤의 2시간 전
print(time + timedelta(days=1, hours=-2))
```
