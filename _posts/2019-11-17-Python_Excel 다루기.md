---
layout: post
title:  "python Excel 다루기"
comments: true
description: "Python, excel"
author: SeungHyeon Tak
date:   2019-11-17 19:42:00 +0700
categories: [python]
tags: [python]
keywords: "python"
---
#### python Excel
<br>

#### Excel관련 모듈
여러가지 모듈중에 `openpyxl`을 소개하려한다. <br>

> Openpyxl install <br>

```python
$ pip install openpyxl

import openpyxl 해주기
```

<br>
#### Excel 파일 쓰기

```python
from openpyxl import Workbook

write_wb = Workbook()

# 이름이 있는 시트를 생성
# write_ws = write_wb.create_sheet('생성시트')

# sheet1에다 입력
write_ws= write_wb.active
write_ws['A1'] = '첫번째 행'

# 행 단위로 추가
write_ws.append(['1','2','3'])

# 셀 단위로 추가
write_ws.cell(5,5, '5행5열')
write_wb.save('파일경로및 이름')

##################################

from openpyxl import Workbook

wb = Workbook()

# 파일 이름을 정하고, 데이터를 넣을 시트를 활성화한다.
sheet1 = wb.active
file_name = 'sample.xlsx'

# 시트의 이름을 정한다.
sheet1.title = 'samplesheet'

abc = ['A','B','C','D','E','F','G','H','I','J','K','L']

# cell 함수를 이용해 넣을 데이터의 행렬 위치를 지정해준다.
for row_index in range(1, 11):
    sheet1.cell(row=row_index, column=1).value =row_index
    sheet1.cell(row=row_index, column=2).value = abc[(row_index-1)]

wb.save(filenem=file_name)
```

<br>
