---
layout: post
title:  "JavaScript 내장함수"
comments: true
description: "javascript"
author: SeungHyeon Tak
date:   2019-10-28 21:34:00 +0700
categories: [javascript]
tags: [javascript]
keywords: "javascript"
---
#### JavaScript 배열 내장 함수
<br>

#### forEach

> `forEach`는 가장 쉬운 배열 내장함수이다. <br>
> 기존의 for문을 대체 시킬 수 있음. <br>
> `=>` 화살표 함수 사용 <br>


```javascript
const num = ['객체1', '객체2', '객체3', '객체4'];

superheroes.forEach(sum => {
    console.log(sum);
});
```
<br>

> forEach 함수의 파라미터로는, 각 원소에 대하여 처리하고 싶은 코드를 함수로 넣어준다. <br>
> 함수에 따라 파라미터 sum은 각 원소를 가르키게 된다. <br>
<br>
> 이렇게 함수형태의 파라미터를 전달하는 것을 콜백함수라고 부른다. <br>
> 함수를 등록 해주면, forEach가 실행을 해주는것이다. <br>

*****

#### map

> `map`은 배열안의 각 원소를 변환 할 때 사용 되며, 이 과정에서 새로운 배열이 만들어진다. <br>
> 만약 배열안의 모든 숫자를 제곱해서 새로운 배열을 만들고 싶다면 다음과 같이 하면 된다. <br>

```javascript
const array = [1,2,3,4,5,6,7,8];

const square = n => n*n;
const squared = array.map(square);
console.log(squared);

// 다른 코드로는

const squared = array.map(n => n*n);
console.log(squared);
```

*****

#### indexOf

> `indexOf`는 원하는 항목이 몇번째 원소인지 찾아주는 함수이다. <br>

```javascript
// 예를 들어 배열에서 찾고 싶은 항목이 몇번째인지 알고싶을때,

const objects = ['객체1', '객체2', '객체3', '객체4'];
const index = objects.indexOf('객체3');
console.log(index); // 결과값을 알 수 있을것이다.
```
<br>

*****

#### findIndex

> 만약에 배열 안에 있는 값이 int, string, boolean이라면 찾고자하는 항목이 몇번째 원소인지 알아내려면 indexOf를 사용하면 된다.<br>
> 하지만 배열 안에 있는 값이 객체이거나, 배열이라면 indexOf로 찾을 수 없다. <br>

```javascript
const todos = [
  {
    id: 1,
    text: '자바스크립트 입문',
    done: true
  },
  {
    id: 2,
    text: '함수 배우기',
    done: true
  },
  {
    id: 3,
    text: '객체와 배열 배우기',
    done: true
  },
  {
    id: 4,
    text: '배열 내장함수 배우기',
    done: false
  }
];

const index = todos.findIndex(todo => todo.id === 3);
console.log(index);
```
<br>

#### toUpperCase

소문자를 대문자로 변환 시켜준다.

```
// 함수를 이용하여 알아보기

function get_members(){
    return ['tak', 'seung', 'hyeon'];
}

members = get_members();

document.write(members[0].toUpperCase()+'<br />');
document.write(members[1].toUpperCase()+'<br />');
document.write(members[2].toUpperCase()+'<br />');
```

> 등등 많은 내장 함수들이 있다. <br>
> 더 많은 내용을 보고 싶다면 밑의 링크로 가서 참고 하면 된다. <br>

<br>
Link:[javascript 정리 블로그](https://learnjs.vlpt.us/basics/09-array-functions.html)
<br>


