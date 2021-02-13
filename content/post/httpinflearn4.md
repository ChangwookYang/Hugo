---
title: "HTTP 웹 기본지식(4) - HTTP 상태코드"
date: 2021-02-14T00:30:00+09:00
---



### 상태코드

>  클라이언트가 보낸 요청의 처리 상태를 응답에서 알려주는 기능

* 1xx (Informational) : 요청이 수신되어 처리중(거의 사용X)
* 2xx (Successful) : 요청 정상 처리
* 3xx (Redirection) : 요청을 완료하려면 추가 행동이 필요
* 4xx (Client Error) : 클라이언트 오류, 잘못된 문법등으로 서버가 요청을 수행할 수 없음
* 5xx (Server Error) : 서버 오류, 서버가 정상 요청을 처리하지 못함



### 2xx - 성공

> 클라이언트의 요청을 성공적으로 처리

* 200 OK
* 201 Created
* 202 Accepted
  * 요청이 접수되었으나 처리가 완료되지 않음
  * 배치 처리 같은 곳에서 사용
  * 요청 접수 후 1시간 뒤에 배치 프로세스가 요청을 처리함
* 204 No Content
  * 서버가 요청을 성공적으로 수행했지만, 응답 페이로드 본문에 보낼 데이터가 없음
  * 예) 웹 문서 편집기에서 save 버튼
    * save 버튼의 결과로 아무 내용이 없어도 된다.
    * save 버튼을 눌러도 같은 화면을 유지해야한다.
    * 결과 내용이 없어도 204 메시지(2xx) 만으로 성공을 인식할 수 있다.



### 3xx - Redirection

> 요청을 완료하기 위해 유저 에이전트(웹브라우저)의 추가 조치 필요
>
> 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동(리다이렉트)

<img src="https://user-images.githubusercontent.com/66955409/107847389-6d864880-6e2e-11eb-8133-21d4dccaa4e2.png" alt="image" style="zoom:50%;" />

* 300 - Multiple Choices
* 301 - Moved Permanently
* 302 - Found
* 303 - See Other
* 304 - Not Modified
* 307 - Temporary Redirect
* 308 - Permanent Redirect
* 리다이렉션 종류
  * 영구 다이렉션 - 특정 리소스의 URI가 영구적으로 이동
    * 301, 308
    * 리소스의 URI가 영구적으로 이동
    * 원래의 URL을 사용 X, 검색 엔진 등에서도 변경 인지
    * 301 Moved Permanently 리다이렉트 요청 메서드가 GET으로 변하고 본문이 제거될수 있음
    * 308 Permanent Redirect 301과 기능은 같으나 리다이렉트 시 요청 메서드와 본문 유지
    * 예) /members -> /users
    * 예) /event -> /new-event
  * 일시 다이렉션 - 일시적인 변경
    * 302, 307, 303
    * 리소스의 URI가 일시적으로 변경
    * 따라서 검색 엔진 등에서 URL을 변경하면 안됨
    * 주문 완료 후 주문 내역 화면으로 이동
    * 302 Found
      * 리다이렉트 시 요청 메서드가 GET으로 변하고 본문이 제거될 수 있음
    * 307 Temporary Redirect
      * 리다이렉트 시 요청 메서드와 본문 유지
    * 303 See Other
      * 리다이렉트 시 요청 메서드가 GET으로 변경
    * PRG: Post / Redirect / Get
      * POST로 주문 후에 웹 브라우저를 새로고침하면?
      * 새로고침은 다시 요청
      * 중복 주문이 될 수 있다.
      * 해결을 위해 PRG 사용(일시적인 리다이렉션)
        * POST로 주문후에 새로고침으로 인한 중복 주문 방지
        * POST로 주문 후에 주문 결과 화면을 GET 메서드로 리다이렉트
        * 새로고침해도 결과 화면을 GET으로 조회
        * 중복 주문 대신에 결과 화면만 GET으로 다시 요청
        * PRG 사용 전
        * <img src="https://user-images.githubusercontent.com/66955409/107854104-151a6f80-6e5d-11eb-8054-8fa09806c1da.png" alt="image" style="zoom:50%;" />
        * PRG 사용 후
        * <img src="https://user-images.githubusercontent.com/66955409/107854130-43984a80-6e5d-11eb-87f9-df9405c5be24.png" alt="image" style="zoom:50%;" />
        * PRG 이후 리다이렉트
          * URL 이 이미 POST->GET으로 리다이렉트 됨
          * 새로 고침해도 GET으로 결과 화면만 조회
  * 특수 리다이렉션
    
    * 결과 대신 캐시를 사용
    
    * 304 Not Modified
    
      * 캐시를 목적으로 사용
      * 클라이언트에게 리소스가 수정되지 않았음을 알려준다.
      * 따라서 클라이언트는 로컬 PC에 저장된 캐시를 재사용한다.(캐시로 리다이렉트)
      * 304 응답은 응답에 메시지 바디를 포함하면 안된다.(로컬 캐시를 사용해야하므로)
      * 조건부 GET, HEAD 요청 시 사용
    
      
    

### 4XX (Client Error)

> 클라이언트 오류 (다시 보내도 클라이언트 에러이기에 잘될 수 없다.)

* 400 Bad Request
  * 클라이언트가 잘못된 요청을 해서 서버가 요청을 처리할 수 없음
  * 요청 구문, 메시지 등등 오류
  * 요청 파라미터가 잘못 되거나, API 스펙이 맞지 않을때
* 401 Unauthorized
  * 클라이언트가 해당 리소스에 대한 인증이 필요함
  * 401 오류 발생 시 응답에 WWW-Authenticate 헤더와 함께 인증 방법을 설명
  * 참고
    * 인증(Authentication) : 본인이 누구인지 확인(로그인)
    * 인가(Authorization)  : 권한부여(Admin 권한 처럼 특정 리소스에 접근할 수 있는 권한)
* 403 Forbidden
  * 서버가 요청을 이해했지만 승인을 거부함
  * 주로 인증 자격 증명은 있지만, 접근 권한이 불충분할 경우
* 404 Not Found
  * 요청 리소스가 서버에 없음
  * 또는 클라이언트가 권한이 부족한 리소스에 접근할 때 해당 리소스를 숨기고 싶을 때



### 5XX (Server Error)

> 서버 오류 (서버에 문제가 있기 떄문에 재시도하면 성공할수도 있음)

* 500 Internal Server Error
  * 서버 내부 문제로 오류 발생
  * 애매하면 500 오류

* 503 Service Unavailable
  * 서비스 이용 불가
  * 서버가 일시적인 과부하 또는 예정된 작업으로 잠시 요청을 처리할 수 없음
  * Retry-After 헤더 필드로 얼마뒤에 복구되는지 보낼 수도 있음