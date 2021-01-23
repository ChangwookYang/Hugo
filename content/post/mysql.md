---
title: "MySQL"
date: 2021-01-23T21:20:00+09:00
---


#### Oracle vs MySQL

1. 컬럼값이 null일때 대체해주는 방법이 다르다.

   > Oracle : SELECT NVL(USER_ID, '') FROM USER;
   >
   > MySQLSELECT IFNULL(USER_ID, '') FROM USER;

2. 현재날짜 확인

   > Oralce : SYSDATE
   >
   > MySQL : NOW()

3. 날짜포맷 변환방법

   > Oracle : SELECT TO_CHAR(SYSDATE, 'YYYYMMDD HH24:MI:SS') FROM DUAL;
   >
   > MySQL : SELECT DATE_FORMAT(NOW(), '%Y%m%d %H:%i:%s');
   >
   > ​		%Y : 2021, %y : 21, %H : 00 ~ 24, %h : 00 ~ 12

4. 문자 합치기

   > Oracle : 'changwook' || 'Yang'
   >
   > 
   >
   > Mysql : CONCAT('%', 'changwook', 'Yang')

5. 형변환

   > Oracle : TO_CHAR(632), TO_NUMBER('632')
   >
   > MySQL : CAST(1234 AS CHAR), CAST('1234' AS NUMBER)

6. 페이징 처리

	> Oracle : ROWNUM BETWEEN 0 AND 10
	>
	> MySQL : LIMIT 0, 10
	
7. 시퀀스 다음번호

   > Oracle : 시퀀스명.NEXTVAL
   >
   > MySQL : 시퀀스명.CURRVAL



#### Workbench 단축키

보기좋게 정렬 : Ctrl + b

쿼리 다중 실행 : Ctrl + Shift + Enter



#### Table 구조 명령어

테이블 구조 확인 : Desc 테이블명

SELECT * FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_SCHEMA = '테이블명';

SELECT * FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_SCHEMA = '테이블명';




#### 사용자 정의 변수

SET @변수이름 = 대입값;

ex) SET @start = 15, @finish = 20;

SELECT * FROM employee WHERE id between @start and @finish;



### USE 데이터베이스명

현재 사용중인 Database를 변경한다.

USE test;

Use mysql;





참고 : https://junghyun100.github.io/OracleVsMySql/

http://blog.devez.net/149