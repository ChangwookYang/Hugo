---
title: "Spring boot Batch"
date: 2021-02-07T23:00:00+09:00
---

#### Spring Boot Batch 강점

* 자동화
* 대용량 처리
* 견고성 : 예측못한 상황이나 동작에 대한 예외 처리도 정의 가능
* 재사용성  : 공통적인 작업을 단위별로 재사용



#### Spring Boot Batch 고려사항

* 단순하게 : 복잡한구조, 로직을 피해야한다.
* 안전하게 : 예외적인 상황이 일어나지 않도록 데이터 무결성 보장
* 가볍게 : 최대한 적은 횟수로 데이터를 가져와서 처리하고 저장
* 스케쥴러가 아니다



#### 기본구조

> Read 가져와서 : 원하는 조건의 데이터 레코드를 DB에서 읽어온다.
>
> Processing 처리하고 : 읽어온 데이터를 비즈니스 로직을 따라 처리한다.
>
> Write 저장한다 : 처리한 데이터를 DB에 업데이트 한다.

![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcAIq4F%2FbtqwDJgdhl7%2FjQ7EL3kaCVjlNRffCHEGhK%2Fimg.png)



참고 : https://ahndy84.tistory.com/18