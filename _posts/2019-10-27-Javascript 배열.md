---
layout: post
title:  "javascript 배열"
comments: true
description: "javascript"
author: SeungHyeon Tak
date:   2019-10-27 20:43:00 +0700
categories: [javascript]
tags: [javascript]
keywords: "javascript"
---
#### JavaScript 배열
<br>

#### 배열

> 객체는 한 변수 혹은 상수에 여러가지 정보를 담기 위함이였다면, 배열은 여러개의 항목들이 들어있는 리스트와 같다. <br>
<br>

* 배열 선언 해보기

<br>
```javascript
const array = [1,2,3,4,5];
```
<br>

> 배열을 선언 할 때에는 `[]`안에 감싸주면 된다. <br>
> 어떤 값이던지 넣을 수 있음.<br>

* 객체 배열

```javascript
const objects = [{name: '객체1'}, {name: '객체2'}];
```

<br>

> 배열을 선언하고 조회 하고 싶다면 `objects[n]` 이렇게 괄호안에 보고 싶은 값의 index를 넣어주면 된다. <br>
> 참고 : 0부터 시작한다. <br>

<br>

*****

#### 배열에 새 항목 추가하기

> 배열에 새로운 항목을 추가 할 때에는 배열이 지니고 있는 내장 함수인 `push` 함수를 사용 <br>
> 다음과 같이 코드로 나타낼 수 있다.

```javascript
const objects = [{name: '객체1'}, {name: '객체2'}];

objects.push({
    name: '객체3'
});

console.log(objects); // 추가 된거 확인
```
<br>

*****

#### 배열의 크기 알아내기

> 배열의 크기를 알아낼 때에는 배열의 `lenght`값으로 확인 가능 <br>

```javascript
const objects = [{name: '객체1'}, {name: '객체2'}];

console.log(objects.length);

objects.push({
    name: '객체3'
});

console.log(objects.length);
```

<br>

####  function안에서 배열 호출하기

```javscript
function get_members(){
    return ['abcd','1234','가나다라']
}

const members = get_members();
document.write(members[0]);  // 'abcd'
document.write(members[1]);  // '1234'
document.write(members[2]);  // '가나다라'
```


