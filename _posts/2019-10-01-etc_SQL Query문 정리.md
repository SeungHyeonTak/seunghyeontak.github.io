---
layout: post
title:  "(etc). SQL Query문 정리"
comments: true
description: "SQL Query"
author: SeungHyeon Tak
date:   2019-10-01 23:27:20 +0700
categories: [etc]
tags: [etc]
keywords: "SQL Query"
---
#### etc. SQL Query문

#### SELECT문
> (데이터를 불러오는 쿼리문)
  * SELECT 컬럼명 FROM 테이블명;
    * 해당 테이블의 해당컬럼의 데이터를 불러온다.
    * 컬럼 전체를 불러오고 싶을땐 컬럼명 부분에 '*'을 넣으면 된다.
> ex) SELECT * FROM My_table; --> My_table의 모든 칼럼 조회
> ex) SELECT Emp, Kor, Age FROM My_table; --> My_table의 Emp, Kor, Age 칼럼 조회

  * SELECT 컬럼명 FROM 테이블명 WHERE 컬럼명=값;
    * WHERE 구문을 추가하여 해당 조건이 참인 데이터만 불러온다.
    * WHERE 뒤에오는 컬럼명의 값이 지정한 값인 데이터 행의 컬럼명만 가져온다.
> ex) SELECT * FROM My_table WHERE Kor = '홍길동'; --> 이름이 홍길동인사람 검색
> ex) SELECT Kor,Age FROM My_table WHERE Age=25; --> 나이가 25살인 사원의 한국이름과 나이 조회
> ex) SELECT Kor,Age FROM My_table WHERE Age<>25; --> 나이가 25살이 아닌 사원 조회

  * SELECT 컬럼명 FROM 테이블명 WHERE 컬럼명=값 ORDER BY 컬럼명 ASC or DESC;
    * ORDER BY 뒤에 오는 컬럼명에 대하여 불러오는 데이터를 정렬한다.
    * ASC는 오름차순, DESC는 내림차순 / 공백을 입력하면 default 값으로 ASC가 정렬됨
> ex) SELECT * FROM My_table ORDER BY Age; --> My_table에서 Age필드의 값을 순서대로 정렬

  * SELECT 컬럼명 FROM 테이블명 WHERE 컬럼명=값 ORDER BY 컬럼명 ASC or DESC LIMIT 개수;
    * LIMIT 구문을 추가하여 데이터 행이 많을때 개수만큼 데이터를 불러온다.

#### INSERT문
> (데이터를 삽입할때 사용하는 쿼리문)
  * INSERT INTO 테이블명(컬럼명1, 컬럼명2, 컬럼명3) VALUES(값1, 값2, 값3);
    * 테이블명에 있는 컬럼명에 맞게 값을 입력한다. 컬럼명과 값의 갯수는 동일해야함
    * 문자열 입력시 작은 따옴표로 문자열을 감싸줘야한다.
> ex) INSERT INTO table_Student(Name, Class, Age) VALUES('Jane', 'A', '16')

  * INSERT INTO 테이블명 VALUES(값1,값2,값3);
    * INSERT문에서 테이블명 다음에 컬럼명을 입력하지 않아도 된다. 하지만 테이블에 있는 모든 컬럼의 수에 맞게 값을 입력 해야한다.
> ex) 컬럼이 Name, Class, Age 이렇게 3개인 테이블에서
> INSERT INTO table_Student VALUES('jane', 'A', '16'); -> (o)
> INSERT INTO table_Student VALUES('jane', 'A'); -> (x)

#### UPDATE문
> (이미 존재하는 데이터를 수정할때 사용하는 쿼리문)
  * UPDATE 테이블명 SET 컬럼명 = 변경할 값;
    * 테이블에 있는 모든 데이터의 컬럼의 값을 변경한다. 특정한 데이터만 수정하고 싶으면 WHERE절을 사용해야한다.
  * UPDATE 테이블명 SET 컬럼명 = 변경할 값 WHERE 컬럼명=값;
    * WHERE절에 맞는 데이터만 변경한다.
  * UPDATE 테이블명 SET 컬럼명1 = 변경할 값1,컬럼명2 = 변경할 값2 WHERE 컬럼명=값;
    * 변경해야할 컬럼이 여러개일 때 콤마(,)를 사용하여 여러개의 값을 변경할 수 있다.

#### DELETE문
> 테이블에 있는 데이터를 삭제할때 사용한다.
  * DELETE FROM 테이블명;
    * 테이블에 있는 모든 데이터를 삭제한다.
  * DELETE FROM 테이블명 WHERE 컬럼명=값;
    * WHERE절에 맞는 데이터만 삭제한다.
  * WHERE절 조건 여러개 추가하기
    * WHERE절을 통해서 쿼리문에 조건을 추가할 수 있다. 만약 여러가지의 조건을 사용하고 싶을땐 and를 사용하면된다.
> ex) WHERE 컬럼명1=값1 and 컬럼명2=값2

> ※ 참고 UPDATE 쿼리문의 콤마(,)와 다름※


