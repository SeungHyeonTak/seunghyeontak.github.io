---
layout: post
title:  "python array"
comments: true
description: "Python"
author: SeungHyeon Tak
date:   2020-02-04 00:33:00 +0700
categories: [python]
tags: [python]
keywords: "python array"
---
## python array (배열)

#### 배열이란 무엇인가?

 - 데이터를 나열하고 각, 데이터를 인덱스에 대응하도록 주성한 데이터 구조
 - 파이썬에서는 리스트 타입이 배열 기능을 제공하고 있다.

#### 배열은 왜 필요한가?

 - 같은 종류의 데이터를 효율적으로 관리하기 위해
 - 같은 종류의 데이터를 순차적으로 저장

#### 배열의 장단점

 - 장점 
   - 빠른 접근이 가능하다 (인덱스 번호가 있어서 맨 앞의 주소만 알면 찾고자하는 데이터를 빠르게 찾을 수 있다.)

 - 단점
   - 미리 데이터의 수를 정해서 설정해야한다. (연관된 데이터의 추가가 힘들다)
   - 데이터의 추가/삭제가 쉽지 않다. (추가하려면 새로운 배열을 생성해야하고, 삭제하려면 기존의 배열 길이의 데이터가 손실난다.)


#### python과 C언어의 배열 예제

> C언어 예제

```c
#include<studio.h>

int main(int argc, char * arg[])
{
    char country[3] = "US";
    print("%s%s\n", country[0], country[1]);
    print("%s\n", country);
    return 0;
}
```

> python 예제

```python
country = "US"
print(country)
country = country + 'A'
print(country)
```

#### 파이썬과 배열

파이썬은 리스트를 활용한다.

* 1차원배열 리스트로 구현

``` python
# 1차원 배열 리스트로 구현
data = [1,2,3,4,5]
print(data)

# 2차원 배열 리스트로 구현
data = [[1,2,3],[4,5,6],[7,8,9,]
print(data[0][0])
```

이런식으로 구성되어있다. 

코드를 쳐서 직접 확인해보자.

예제 문제)

다음 dataset에서 전체이름안에  'M'이 몇번 나왔는지 빈도수 출력해보기

리스트는 제공 된다.

```python
dataset = ['Braund, Mr. Owen Harris',
           'Cumings, Mrs. John Bradley (Florence Briggs Thayer)',
           'Heikkinen, Miss. Laina',
           'Futrelle, Mrs. Jacques Heath (Lily May Peel)',
           'Allen, Mr. William Henry',
           'Moran, Mr. James',
           'McCarthy, Mr. Timothy J',
           'Palsson, Master. Gosta Leonard',
           'Johnson, Mrs. Oscar W (Elisabeth Vilhelmina Berg)',
           'Nasser, Mrs. Nicholas (Adele Achem)',
           'Sandstrom, Miss. Marguerite Rut',
           'Bonnell, Miss. Elizabeth',
           'Saundercock, Mr. William Henry',
           'Andersson, Mr. Anders Johan',
           'Vestrom, Miss. Hulda Amanda Adolfina',
           'Hewlett, Mrs. (Mary D Kingcome) ',
           'Rice, Master. Eugene',
           'Williams, Mr. Charles Eugene',
           'Vander Planke, Mrs. Julius (Emelia Maria Vandemoortele)',
           'Masselmani, Mrs. Fatima',
           'Fynney, Mr. Joseph J',
           'Beesley, Mr. Lawrence',
           'McGowan, Miss. Anna \"Annie\"',
           'Sloper, Mr. William Thompson',
           'Palsson, Miss. Torborg Danira',
           'Asplund, Mrs. Carl Oscar (Selma Augusta Emilia Johansson)',
           'Emir, Mr. Farred Chehab',
           'Fortune, Mr. Charles Alexander',
           'Dwyer, Miss. Ellen \"Nellie\"',
           'Todoroff, Mr. Lalio'
           ]
```

<details markdown="1">
<summary>접기/펼치기</summary>

```python
count = 0
for i in range(len(dataset)):
    for j in range(len(dataset[i])):
        if dataset[i][j] == 'M':
            count += 1
            print(count, dataset[i][j])
```

</details>

