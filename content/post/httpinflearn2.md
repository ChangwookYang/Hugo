---
title: "HTTP 웹 기본지식(2) - HTTP 기본"
date: 2021-02-13T15:00:00+09:00
---
### 모든 것이 HTTP - HTTP 메시지에 모든 것을 전송

* HTML, TEXT
* Image, 음성, 영상, 파일
* JSON, XML
* 거의 모든 형태의 데이터 전송 가능
* 서버 간에 데이터를 주고 받을때도 대부분 HTTP 사용
* 지금은 HTTP 시대!



### HTTP 역사

* HTTP/0.9 1991년 : GET 메서드만 지원, HTTP 헤더 X
* HTTP/1.0 1996년 : 메서드, 헤더 추가
* HTTP/1.1 1997년 : **가장 많이 사용, 우리에게 가장 중요한 버전**
  * RFC2068 (1997) -> RFC2616(1999) -> RFC7230~7235(2014) : 개정
* HTTP/2, HTTP/3 : 성능개선



### 기반 프로토콜

* TCP : HTTP/1.1, HTTP/2
* UDP : HTTP/3
* 현재 HTTP/1.1 주로 사용
  * HTTP/2, HTTP/3도 점점 증가



### HTTP 특징

* 클라이언트 서버 구조
* 무상태 프로토콜(스테이스리스), 비연결성
* HTTP 메시지
* 단순함, 확장 가능



### 클라이언트 서버 구조

* Request / Response 구조
* 클라이언트는 서버에 요청을 보내고, 응답을 대기
* 서버가 요청에 대한 결과를 만들어서 응답



### 무상태 프로토콜

* 스테이스리스 Stateless
* 서버가 클라이언트의 상태를 보존 X
* 장점 : 서버 확장성 높음(스케일 아웃)
* 단점 : 클라이언트가 추가 데이터 전송



### Stateful, Stateless 차이

* 상태유지 - Stateful
  * 고객 : 이 노트북은 얼마에요?
  * 점원 : 100만원 입니다.
  * 고객 : 2개 주세요
  * 점원 : 200만원입니다. 신용카드 ,현금 중 뭘로할래요?
  * 고객 : 신용카드요
  * 점원 : 200만원 결제 완료요
* 상태유지 - Stateful   > 점원이 중간에 바뀌면?
  * 고객 : 이 노트북은 얼마인가요?
  * 점원A : 100만원 (노트북 상태 유지)
  * 고객 : 2개 주세요
  * 점원B : ??? 무엇을 2개 드릴까요? (노트북, 2개 상태 유지 X)
  * 고객 : 신용카드로 구매할께요
  * 점원C : ???? 무슨 제품을 몇개 신용카드로 구매하실까요? (노트북, 2개, 신용카드 상태 유지 X)
* 무상태 - Stateless
  * 고객 : 이 노트북 얼마인가요?
  * 점원 : 100만원입니다.
  * 고객 : 노트북 2개 구매하겠습니다.
  * 점원 : 노트북 2개는 200만원입니다. 신용카드, 현금 중에 어떤걸로 구매하실래요?
  * 고객 : 노트북 2개를 신용카드로 구매하겠습니다.
  * 점원 : 200만원 결제 완료되었습니다.
  
  > 점원이 바뀌어도 대화가 가능하다.
  >
  > 무상태에서는 고객이 필요한 데이터를 그때그때 다 넘겨준다.

* Stateful, Stateless 차이
  * 상태유지 : 중간에 다른 점원으로 바뀌면 안된다.
    * 항상 같은 서버가 유지되어야 한다.
    * 갑자기 해당 서버가 장애가 발생하면 클라이언트는 재시작해야한다.
  * 무상태 : 중간에 다른 점원으로 바뀌어도 된다.
    * 갑자기 고객이 증가해도 점원을 대거 투입할 수 있다.
    * 갑자기 클라이언트 요청이 증가해도 서버를 대거 투입할 수 있다.
    * 서버가 장애나더라도 나른 서버로 전달할 수 있다.
  * 무상태는 응답 서버를 쉽게 바꿀 수 있다. 
    * **무한한 서버 증설 가능(스케일아웃 - 수평확장)**

* Stateless - 실무 한계
  * 모든 것을 무상태로 설계할 수 있는 경우도 있고 없는 경우도 있다.
  * 무상태 - 로그인이 필요 없는 단순한 서비스 소개 화면
  * 상태 - 로그인
  * 로그인한 사용자의 경우 로그인했다는 상태를 서버에 유지
  * 일반적으로 브라우저 쿠키와 서버 세션등을 사용해서 상태 유지
  * 상태 유지는 최소한만 사용
  * 최대한 무상태로 설계하지만, 어쩔수 없을 경우에 상태로 설계해야한다.





### 비연결성 (connectionless)

<img src="https://user-images.githubusercontent.com/66955409/107841529-dfdf3480-6dfe-11eb-97a3-57af9cc29fc1.png" alt="image" style="zoom:50%;" />

<img src="https://user-images.githubusercontent.com/66955409/107841551-0b621f00-6dff-11eb-8d53-e052899e7d81.png" alt="image" style="zoom:50%;" />

* 비 연결성

  * HTTP는 기본이 연결을 유지하지 않는 모델

  * 일반적으로 초 단위 이하의 빠른 속도로 응답

  * 1시간 동안 수천명이 서비스를 사용해도 실제 서버에서 동시에 처리하는 요청은

    수십개 이하로 매우 작음

    예) 웹브라우저에서 계속 연속해서 검색 버튼을 누르지는 않는다.

  * 서버 자원을 매우 효율적으로 사용할 수 있다.

* 비 연결성 단점

  * TCP/IP 연결을 새로 맺어야 함 - 3 way handshake 시간 추가

  * 웹브라우저로 사이트를 요청하면 HTML 뿐만 아니라 자바스크립트, css, 추가 이미지 등

    수많은 자원이 함께 다운로드

  * 지금은 HTTP 지속연결(Persistent Connections)로 문제 해결

  * HTTP/2, HTTP/3에서 더 많은 최적화

<img src="https://user-images.githubusercontent.com/66955409/107841607-9ba06400-6dff-11eb-9bd7-c80d789ca25f.png" alt="image" style="zoom:50%;" />
<img src="https://user-images.githubusercontent.com/66955409/107841626-bc68b980-6dff-11eb-86e1-c6a475c24382.png" alt="image" style="zoom:50%;" />

### 스테이스리스를 기억하자

** 서버 개발자들이 어려워하는 업무

* 정말 같은 시간에 딱 맞추어 발생하는 대용량 트래픽
* 예) 선착순 이벤트, 명절 KTX 예약, 학과 수업 등록
* 예) 저녁 6:00 선착순 1000명 치킨 할인 이벤트 - 수만명 동시 요청



---

---

### HTTP 메시지

![image](https://user-images.githubusercontent.com/66955409/107841711-9ee81f80-6e00-11eb-9f47-81ecc5872673.png)

> 시작줄, 머리, 공백, 바디

### 시작 라인 - 요청 메시지

> GET /search?q=hello&hl=ko  HTTP/1.1
>
> Host: www.google.com

* start-line = request-line / status-line
* request-line = method SP(공백) request-target SP HTTP-version CRLF(엔터)
* HTTP 메서드 (GET : 조회)
* 요청 대상 (/search?q=hello&hl=ko)
* HTTP Version
* /search? 는 절대경로( "/"로 시작하는 경로 )



### 시작 라인 - 응답 메시지

> HTTP/1.1 200 OK
>
> Content-Type : text/html;charset=UTF-8
>
> Content-Length: 3423
>
> <html>
>
> ​    <body>..</body>
>
> </html>

* status-line = HTTP-version SP status-code SP reason-phrase CRLF
* HTTP 버전
* HTTP 상태 코드 : 요청 성공, 실패를 나타냄
  * 200 : 성공
  * 400 : 클라이언트 요청 오류
  * 500 : 서버 내부 오류
* header-field = field-name ":" OWS field-value OWS (OWS : 띄어쓰기 허용)
* field-name은 대소문자 구분 없음



### HTTP 헤더

* HTTP 전송에 필요한 모든 부가 정보

* 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증

  요청 클라이언트(브라우저) 정보 ,서버 애플리케이션 정보, 캐시 관리 정보 ...



### HTTP 메시지 바디

* 실제 전송할 데이터
* HTML 문서, 이미지, 영상, JSON 등등 byte로 표현할 수 있는 모든 데이터 전송 가능