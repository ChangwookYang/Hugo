---
title: "HTTP 웹 기본지식(3) - HTTP 메서드"
date: 2021-02-13T17:30:00+09:00
---


### HTTP API

> URI 설계에 가장 중요한 것은 리소스 식별

* 리소스의 의미는 뭘까?
  * 회원을 등록하고 수정하고 조회하는게 리소스가 아니다.
  * 예) 미네랄을 캐라 -> 미네랄이 리소스
  * 회원이라는 개념 자체가 바로 리소스이다.

* 리소스를 어떻게 식별하는게 좋을까?

  * 회원을 등록하고 수정하고 조회하는 것을 모두 배제
  * 회원이라는 리소스만 식별하면 된다. -> 회원 리소스를 URI에 매핑

* 리소스 식별, URI 계층 구조 활용

  * 회원 목록 조회 /members
  * 회원 조회 /members/{id} -> 어떻게 구분하지? **<u>GET</u>**
  * 회원 등록 /members/{id}
  * 회원 수정 /members/{id}
  * 회원 삭제 /members/{id}

  > 참고 : 계층 구조상 상위를 컬렉션으로 보고 복수단어 사용 권장(member->members)

* 리소스와 행위를 분리

  > 가장 중요한 것은 리소스를 식별하는 것

  * URI는 리소스만 식별
  * 리소스와 해당 리소스를 대상으로 하는 행위를 분리
    * 리소스 : 회원
    * 행위 : 조회, 등록, 삭제, 변경
  * 리소스는 명사, 행위는 동사
  * 행위(메서드)는 어떻게 구분?



### HTTP 메서드 종류

* GET : 리소스 조회
* POST : 요청 데이터 처리, 주로 등록
* PUT : 리소스 대체, 해당 리소스가 없으면 생성
* PATCH : 리소스 부분 변경
* DELETE : 리소스 삭제



#### GET

	* 리소스 조회
	* 서버에 전달하고 싶은 데이터는 query(쿼리 파라미터, 쿼리 스트링)를 통해서 전달
	* 메시지 바디를 사용하여 데이터를 전달할 수 있지만 권장하지 않는다.



#### POST

* 요청 데이터 처리

* 메시지 바디를 통해 서버로 요청 데이터 전달

* 서버는 요청 데이터를 처리

  * 메시지 바디를 통해 들어온 데이터를 처리하는 모든 기능을 수행한다.

* 주로 전달된 데이터로 신규 리소스 등록, 프로세스 처리에 사용

* POST 메서드는 대상 리스소가 리소스의 고유한 의미 체계에 따라 요청에 포함된 표현을 처리....

  * 예를 들어 POST는 다음에 사용된다.
    * HTML 양식에 입력된 필드와 같은 데이터 블록을 데이터 처리 프로세스에 제공
      * ex) HTML FORM에 입력한 정보로 회원가입, 주문 등
    * 게시판, 뉴스 그룹, 메일링 리스트, 블로그
      * ex) 게시판 글쓰기
    * 서버가 아직 식별하지 않은 새 리소스 생성
      * ex) 신규 주문 ㅅ생성
    * 기존 자원에 데이터 추가
      * 한 문서 끝에 내용 추가하기
  * 정리 : 이 리소스 URI에 POST 요청이 오면 데이터를 어떻게 처리할지 리소스마다 따로 정해야함
    * 정해진 것이 없다!

* 정리

  1. 새 리소스 생성(등록)
     * 서버가 아직 식별하지 않은 새 리소스 생성

  2. 요청 데이터 처리

     * 단순히 데이터를 생성하거나 변경하는 것을 넘어, 프로세스를 처리하는 경우
       * 예) 결제완료 -> 배달 시작 -> 배달완료처럼 단순한 값 변경을 너머 프로세스 상태변경

     * POST 결과로 새로운 리소스가 생성되지 않을 수도 있다.

  3. 다른 메서드로 처리하기 애매한 경우

     * JSON으로 조회 데이터를 넘겨야하는데 GET메서드로 사용하기 어려운 경우
     * 애매하면 POST



### PUT

* 리소스를 대체
  * 리소스가 있으면 대체
  * 리소스가 없으면 생성
  * 쉽게 이야기하여 덮어버림
* 중요! 클라이언트가 리소스를 식별
  * 클라이언트가 리소스 위치를 알고 URI 지정
  * POST와의 차이점이다.
* 주의! 리소스를 완전히 대체한다.



### PATCH

* 리소스를 부분변경
* PUT고 다르게 완전히 대체하지 않고 부분 변경한다.



### DELETE

* 리소스 제거



### HTTP 메서드의 속성

* 안전(Safe Methods)

  * 호출해도 리소스를 변경하지 않는다.

* 멱등(Idempotent Methods)

  * f(f(x)) = f(x)

  * 한 번 호출하든 두 번 호출하든 100번 호출하든 결과가 똑같다.

  * 멱등 메서드

    * GET : 한번 조회하든, 두번하든 결과는 같다.
    * PUT : 결과를 대체한다. 따라서 같은 요청을 여러번 해도 최종 결과는 같다.
    * DELETE:  결과를 삭제하므로 여러번해도 같다.
    * POST : 멱등이 아니다. 두번 호출하면 같은 결제가 중복해서 발생할 수 있다.

  * 활용

    * 자동 복구 메커니즘

    * 서버가 TIMEOUT 등으로 정답 응답을 못주었을때,

      클라이언트가 같은 요청을 다시해도 되는가?의 판단 근거

  * 멱등은 외부 요인으로 중간에 리소스가 변경되는 것까지는 고려하지 않는다.

    * 사용자 1: GET -> username:A, age:20
    * 사용자 1: GET -> username:A, age:21 (1년 뒤)

* 캐시가능(Cacheable Methods)

  * 응답 결과 리소스를 캐시해서 사용해도 되는가?
  * GET, HEAD, POST, PATCH 캐시 가능
  * 실제로는 GET, HEAD 정도만 캐시로 사용
    * POST, PATCH는 본문 내용까지 캐시 키로 고려해야하는데 구현이 쉽지 않다.

![image](https://user-images.githubusercontent.com/66955409/107843745-da8ae580-6e10-11eb-8935-a6b2c7d661ff.png)



### 클라이언트에서 서버로 데이터 전송
> 전달 방식은 크게 2가지

* 쿼리 파라미터를 통한 데이터 전송
  * GET
  * 주로 정렬 필터(검색어)
* 메시지 바디를 통한 데이터 전송
  * POST, PUT, PATCH
  * 회원 가입, 상품 주문, 리소스 등록, 리소스 변경
* 클라리언트에서 서버로 데이터 전송 4가지 상황
  * 정적 데이터 조회
    * 이미지, 정적 텍스트 문서
    * 조회는 GET 사용
    * 정적 데이터는 일반적으로 쿼리 파라미터 없이 리소스 경로로 단순하게 조회 가능
  * 동적 데이터 조회
    * 주로 검색, 게시판 목록에서 정렬 필터(검색어)
    * 조회는 GET, 쿼리 파라미터 사용 (?q=hello&hl=ko)
    * 조회 조건을 줄여주는 필터, 조회 결과를 정렬하는 정렬 조건에 주로 사용
  * HTML Form을 통한 데이터 전송
    * POST전송 - 회원 가입, 상품 주문, 데이터 변경
    * Content-Type: application/x-www-form-urlencoded 사용
      * form의 내용을 메시지 바디를 통해서 전송(key=value, 쿼리 파라미터 형식)
      * 전송 데이터를 url encoding 처리
    * 예) abc김 -> abc%EA%B9%80
      * HTML Form은 GET전송도 가능
      * Content-Type: multipart/form-data
        * 파일 업로드 같은 바이너리 데이터 전송 시 사용
        * 다른 종류의 여러 파일과 폼의 내용과 함꼐 전송 가능(multipart)
      * HTML Form 전송은 GET, POST만 지원
    * <img src="https://user-images.githubusercontent.com/66955409/107843938-c0520700-6e12-11eb-8059-ab62430da4b0.png" alt="image" style="zoom:50%;" />
    * <img src="https://user-images.githubusercontent.com/66955409/107843995-23dc3480-6e13-11eb-98bc-b3178d792bc5.png" alt="image" style="zoom:50%;" />
    * <img src="https://user-images.githubusercontent.com/66955409/107844066-b7ae0080-6e13-11eb-83d1-727a84b62332.png" alt="image" style="zoom:50%;" />
  * HTTP API를 통한 데이터 전송
    * <img src="https://user-images.githubusercontent.com/66955409/107844160-71a56c80-6e14-11eb-9552-ea6eb40710e7.png" alt="image" style="zoom:50%;" />
    * 서버 to 서버
      * 백엔드 시스템 통신
    * 앱 클라이언트
      *  아이폰, 안드로이드
    * 웹 클라이언트
      *  HTML에서 Form 전송 대신 자바 스크립트를 통한 통신에 사용(Ajax)
      *  ex) React, VueJS 같은 웹 클라이언트와 API 통신
    * POST, PUT, PATCH : 메시지 바디를 통해 데이터 전송
    * GET : 조회, 쿼리 파리미터로 데이터 전달
    * Content-Type : application/json을 주로 사용(사실상 표준)
      * TEXT, XML, JSON 등등



### HTTP API 설계 예시

* HTTP API - 컬렉션
  * POST 기반 등록
  * 예) 회원 관리 API 제공
* HTTP API - 스토어
  * PUT 기반 등록
  * 예) 정적 컨텐츠 관리, 원격 파일 관리
* HTML FORM 사용
  * 웹 페이지 회원 관리
  * GET, POST 만 지원



### 회원 관리 시스템

> POST - 신규 자원 등록 특징

* 클라이언트는 등록될 리소스의 URI를 모른다.

  * 회원 등록 /members -> POST
  * POST /members

* **서버가 새로 등록된 리소스 URI를 생성해준다.**

  * HTTP/1.1 201 Created

    Location: **/members/100**

* 컬렉션(Collection)

  * 서버가 관리하는 리소스 디렉토리
  * 서버가 리소스의 URI를 생성하고 관리
  * 여기서 컬렉션은 /members



### 파일 관리 시스템

> PUT 기반 등록

* 클라이언트가 리소스 URI를 알고 있어야한다.
  * 파일 등록 /files/{filename} -> PUT
  * PUT /files/star.jpg
* 클라이언트가 직접 리소스의 URI를 지정한다.
* 스토어(Store)
  * 클라이언트가 관리하는 리소스 저장소
  * 클라이언트가 리소스의 URI를 알고 관리
  * 여기서 스토어는 /files



### HTML FORM 사용

* HTML FORM은 GET, POST만 사용
* AJAX 같은 기술을 사용해서 해결 가능
* GET, POST만 사용할 수 있으므로 제약이 있다.
  * 해결하기 위하여 동사로 된 리소스 경로 사용
  * 컨트롤 URI
    * /new, /edit, /delete
    * HTTP 메서드로 해결하기 어려운 경우 사용



### 참고하면 좋은 URI 설계 개념

* 문서 document
  * 단일 개념(파일 하나, 객체 인스턴스, 데이터베이스 row)
  * 예) /members/100, /files/star.jpg
* 컬렉션 collection
  * 서버가 관리하는 리소스 디렉터리
  * 서버가 리소스의 URI를 생성하고 관리
  * 예) /members
* 스토어 store
  * 클라이언트가 관리하는 자원 저장소
  * 클라이언트가 리소스의 URI를 알고 관리
  * 예) /files
* 컨트롤러(Controller), 컨트롤 URI
  * 문서, 컬렉션, 스토어로 해결하기 어려운 추가 프로세스 실행
  * 동사를 직접 사용
  * 예) /members/{id}/delete

