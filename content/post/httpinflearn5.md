---
title: "HTTP 웹 기본지식(5) - HTTP 헤더"
date: 2021-02-14T23:30:00+09:00
---


### HTTP 헤더

* HTTP 전송에 필요한 모든 부가 정보

* 예) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트,

  서버 정보, 캐시 관리 정보 ...

* 표준 헤더가 너무 많음

* 필요 시 임의의 헤더 추가 가능



### HTTP BODY

> message body - RFC7230(최신)

* Representation = representation Metadata + Representation Data
  * 표현 = 표현 메타데이터 + 표현 데이터
  * <img src="https://user-images.githubusercontent.com/66955409/107854772-94aa3d80-6e61-11eb-9869-710a2dd3aacc.png" alt="image" style="zoom:50%;" />
* 메시지 본문(message body)을 통해 표현 데이터 전달
* 메시지 본문 = 페이로드 (payload)
* 표현은 요청이나 응답에서 전달할 실제 데이터
* 표현 헤더는 표현 데이터를 해석할 수 있는 정보 제공
  * 데이터 유형(html, json), 데이터 길이, 압축 정보 등등



### 표현

> 리소스를 JSON / HTML로 전달할 거야, 이해할 수 있게 "표현"한다

* Content-Type : 표현 데이터의 형식
  * 미디어 타입, 문자 인코딩
  * text/html; charset=utf-8
  * application/json
* Content-Encoding : 표현 데이터의 압축 방식
  * 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가
  * 데이터를 읽는 쪽에서 인코딩헤더 정보 기반으로 압축 해제
* Content-Language : 표현 데이터의 자연 언어
  * ko, en, en-US
* Content-Length : 표현 데이터의 길이(바이트 단위)
  * Transfer-Encoding(전송 코딩)을 사용하면 Content-Length를 사용하면 안됨
* 표현 헤더는 전송, 응답 둘 다 사용



### 협상(콘텐츠 네고시에이션)

> 클라이언트가 선호하는 표현 요청

* Accept : 클라이언트가 선호하는 미디어 타입 전달
* Accept-Charset : 클라이언트가 선호하는 문자 인코딩
* Accept-Encoding : 클라이언트가 선호하는 압축 인코딩
* Accept-Language : 클라이언트가 선호하는 자연 언어
* 협상 헤더는 요청시에만 사용



### 협상과 우선순위1 Quality Values(q)

* Quality Values(q), 0~1, 클수록 높은 우선순위

* 생략하면 1

* >GET /event
  >
  >Accept-Langauge: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7

  * ko-KR;q=1 (q생략)
  * ko;q=0.9
  * en-US;q=0.8
  * en;q=0.7



### 협상과 우선순위2 Quality Values(q)

* 구체적인 것이 우선
* Accept: text/*, text/plain, text/plain;format=flowed, */\*
  * text/plain;format=flowed
  * text/plain
  * text/*
  * \*/*



### 전송 방식

* 단순 전송 Content-Length
* 압축 전송 Content-Encoding
* 분할 전송 Transfer-Encoding
* 범위 전송 Range, Content-Range



### 일반 정보

* From : 유저 에이전트의 이메일 정보
* Referer : 이전 웹페이지 주소
  * 현재 요청된 페이지의 이전 웹 페이지 주소
  * A->B로 이동할 경우 B를 요청할때 Referer:A를 포함해서 요청
  * Referer를 사용해서 유입경로 분석 가능
* User-Agent : 유저 에이전트 애플리케이션 정보
  * user-agent: Mozilla/5.0 (Macintosh; INtel Max OS X 10_15_7)
  * 클라이언트의 애플리케이션 정보
  * 통계 정보
  * 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
* Server : 요청을 처리하는 ORIGIN 서버의 소프트웨어 정보
  * Origin : 실제 요청을 처리하는 서버
  * 응답에서 사용
* Date : 메시지가 발생한 날짜와 시간
  * 응답에서 사용



### 특별한 정보

* Host : 요청한 호스트 정보(도메인)

  * > GET /search?q=hello&hl=ko HTTP/1.1
    >
    > Host: www.google.com

  * 필수로 요청에서 사용

  * 하나의 서버가 여러 도메인을 처리해야 할 때

  * 하나의 IP 주소에 여러 도메인이 적용되어 있을 때

* Allow : 허용 가능한 HTTP 메서드
  
  * 405 (Method Now Allowed) 에서 응답을 포함함
* Retry-After : 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간



### 인증

* Authorization : 클라이언트 인증 정보를 서버에 전달

  * Authorization : Basic xxxxxxxxxxxxx

* WWW-Authenticate : 리소스 접근 시 필요한 인증 방법 정의

  * 401 Unauthorized 응답과 함께 사용

  * WWW-AUthenticate: Newauth realm="apps", type=1,

    ​									title="Login to \\"apps\\"", Basic realm="simple"



### 쿠키

* Set-Cookie : 서버에서 클라이언트로 쿠키 전달(응답)
* Cookie : 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청 시 서버로 전달

* Stateless
  * HTTP는 무상태 프로토콜이다
  * 클라이언트와 서버가 요청과 응답을 주고 받으면 연결이 끊어진다.
  * 클라이언트가 다시 요청하면 서버는 이전 요청을 기억하지 못한다.
  * 클라이언트와 서버는 서로 상태를 유지하지 않는다.
* 대안 - 모든 요청에 사용자 정보 포함
  * 보안 등등 문제가 많다.
  * 브라우저를 완전 종료하고 다시 열면?
* <img src="https://user-images.githubusercontent.com/66955409/107865452-d232b900-6ea9-11eb-897f-4239e8d5284c.png" alt="image" style="zoom:50%;" />![image](https://user-images.githubusercontent.com/66955409/107865462-fd1d0d00-6ea9-11eb-950c-cbfd1170780b.png)
* <img src="https://user-images.githubusercontent.com/66955409/107865462-fd1d0d00-6ea9-11eb-950c-cbfd1170780b.png" alt="image" style="zoom:50%;" />

* set-cookie: sessionId=abcde1234; expires=Sat, 26-Dec-2020 00:00:00 GMT;

  ​					path=/; domain=.google.com; Secure

* 사용처

  * 사용자 로그인 세션관리

    * 매번 user정보를 보내면 보안에 위험하므로 sessionId로 관리한다.
    * 광고 정보 트래킹

  * 쿠키 정보는 항상 서버에 전송됨

    * 네트워크 트래픽 추가 유발

    * 최소한의 정보만 사용(세션 id, 인증 토큰)

    * 서버에 전송하지 않고, 웹브라우저 내부에 데이터를 저장하고 싶으면 

      웹스토리지(localStorage, sessionStorage)

  * 주의!

    * 보안에 민감한 데이터는 저장하면 안됨(주민번호, 신용카드 번호 등등)

* 쿠키 생명주기
  * Set-Cookie : expires=Sat, 26-Dec-2020 04:39:21 GMT
    * 만료일이 되면 쿠키 삭제
  * Set-Cookie : max-age=3600 (3600초)
    * 0이나 음수를 지정하면 쿠키 삭제
  * 세션 쿠키 : 만료 날짜를 생략하면 브라우저 종료시 까지만 유지
  * 영속 쿠키 : 만료 날짜를 입력하면 해당 날짜까지 유지
* 쿠키 - 도메인
  * domain=example.org
  * 명시 : 명시한 문서 기준 도메인 + 서브 도메인 포함
    * domain=example.org를 지정해서 쿠키 생성
      * example.org는 물론이고
      * dev.example.org도 쿠키 접근
  * 생략 : 현재 문서 기준 도메인만 적용
    * example.org에서 쿠키를 생성하고 domain 지정을 생략
      * example.org에서만 쿠키 접근
      * dev.example.org는 쿠키 미접근

* 쿠키 - 경로
  * path=/home
  * 이 경로를 포함한 하위 경로 페이지만 쿠키 접근
  * 일반적으로 path=/ 루트로 지정
  * path=/home 지정
    * /home -> 가능
    * /home/level1 -> 가능
    * /home/level1/level2 -> 가능
    * /hello -> 불가능
* 쿠키 - 보안
  * Secure
    * 쿠키는 http, https를 구분하지 않고 전송
    * Secure를 적용하면 https인 경우에만 전송
  * HttpOnly
    * XSS 공격 방지
    * 자바스크립트에서 접근 불가(document.cookie)
    * HTTP 전송에만 사용
  * SameSite
    * XSRF 공격 방지
    * 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송



### 캐시와 조건부 요청

* 캐시 기본 동작
* <img src="https://user-images.githubusercontent.com/66955409/107865673-6d2c9280-6eac-11eb-9a9c-74dd8d15d93a.png" alt="image" style="zoom:50%;" />
* 캐시가 없을 때
  * 데이터가 변경되지 않아도 계속 네트워크를 통해서 데이터를 다운로드 받는다.
  * 인터넷 네트워크는 매우 느리고 비싸다
  * 브라우저 로딩 속도가 느리다.
* <img src="https://user-images.githubusercontent.com/66955409/107865705-c3013a80-6eac-11eb-8a1f-6227a8ce0080.png" alt="image" style="zoom:50%;" />
* 캐시 적용
  * 캐시 덕준에 캐시 가능 시간동안 네트워크를 사용하지 않아도 된다.
  * 비싼 네트워크 사용량을 줄일 수 있다.
  * 브라우저 로딩 속도가 매우 빠르다.
  * 캐시 유효 시간이 초과되면, 서버를 통해 데이터를 다시조회하고 캐시를 갱신한다.
  * 이때 다시 네트워크 다운로드가 발생한다.



### 검증 헤더와 조건부 요청

* 캐시 시간 초과

  * 캐시 유효 시간이 초과되어서 서버에 다시 요청하면 다음 두가지 상황이 나타난다.
    * 서버에서 기존 데이터를 변경함
    * 서버에서 기존 데이터를 변경하지 않음
  * 캐시 만료후에도 서버에서 데이터를 변경하지 않은 경우
    * 데이터를 갱신하는 대신에 저장해 두었던 캐시를 재사용 할 수 있다.
    * 단 클라이언트의 데이터와 서버의 데이터가 같다는 사실을 확인할 수 있는 방법이 필요

* 검증 헤더 추가

  * <img src="https://user-images.githubusercontent.com/66955409/107865769-8255f100-6ead-11eb-8fc5-141d27043b1d.png" alt="image" style="zoom:50%;" />
  * <img src="https://user-images.githubusercontent.com/66955409/107865788-9dc0fc00-6ead-11eb-9442-0614932106f8.png" alt="image" style="zoom:50%;" />

  * 캐시 유효 시간이 초과해도 서버의 데이터가 갱신되지 않으면
  * 304 Not Modified + 헤더 메타 정보만 응답(바디X)
  * 클라이언트는 서버가 보낸 응답 헤더 정보로 캐시의 메타 정보를 갱신
  * 클라이어느는 캐시에 저장되어 있는 데이터 재활용
  * 결과적으로 네트워크 다운로드가 발생하지만 용량이 적은 헤더 정보만 다운로드

* 검증 헤더

  * 캐시 데이터와 서버 데이터가 같은지 검증하는 데이터
  * Last-Modified, ETag

* 조건부 요청 헤더

  * 검증 헤더로 조건에 따른 분기

  * 조건이 만족하면 200 OK

  * 조건이 만족하지 않으면 304 Not Modified

  * If-Modified-Since : Last-Modified 사용

    * 이후에 데이터가 수정되었으면?
    * 데이터 미변경 예시
      * 304 Not Modified
      * 전송 용량 : 헤더 0.1M
    * 데이터 변경
      * 200 OK
      * 전송 용량 : 헤더 0.1M, 바디 1.0M
    * 단점
      * 날짜 기반의 로직 사용
      * 데이터가 수정해서 날짜가 다르지만, 같은 데이터를 수정해서 데이터 결과가 같은 경우
      * 스페이스나 주석처럼 크게 영향 없는 변경에서 캐시를 유지하고 싶은 경우

  * If-None-Match : ETag 사용 (Entity Tag)

    * 캐시용 데이터에 임의의 고유한 버전 이름을 달아둠

      * 예) ETag: "v1.0", ETag: "a2jiodwjekjl3"

    * 데이터가 변경되면 이 이름을 바꾸어서 변경함(Hash를 다시 생성)

      * 예) ETag: "aaaaa" -> ETag: "bbbbb"

    * ETag만 보내서 같으면 유지, 다르면 다시 받기

    * 캐시 제어 로직을 서버에서 완전히 관리

    * 클라이언트는 단순히 이 값을 서버에 제공(클라이언트는 캐시 메커니즘을 모름)

    * 예) 서버는 배타 오픈 기간인 3일동안 파일이 변경되어도 ETag를 동일하게 유지

      ​	애플리케이션 주기에 맞추어 ETag 모두 갱신

      ![image](https://user-images.githubusercontent.com/66955409/107865993-913da300-6eaf-11eb-81a0-233b5225f754.png)

  

### Cache-Control 캐시 지시어

*  Cache-Control: max-age
  * 캐시 유효 시간, 초 단위
* Cache-Control: no-cache
  * 데이터는 캐시해도 되지만, 항상 원(origin) 서버에 검증하고 사용
* Cache-Control: no-store
  * 데이터에 민감한 정보가 있으므로 저장하면 안됨(메모리에서 사용하고 최대한 빨리 삭제)



### 프록시 캐시 도입

<img src="https://user-images.githubusercontent.com/66955409/107879365-bbc54580-6f1b-11eb-9009-7e81be4cb29f.png" alt="image" style="zoom:50%;" />

* Cache-Control : public
  * 응답이 public 캐시에 저장되어도 됨
* Cache-Control : private
  * 응답이 해당 사용자만을 위한 것임. private 캐시에 저장해야함
* Cache-Control : s-maxage
  * 프록시 캐시에만 적용되는 max-age
* Age : 60 (HTTP 헤더)
  * Origin 서버에서 응답 후 프록시 캐시 내에 머문 시간
* Cache-Control: no-cache, no-store, must-revalidate
  * 예) 통장 잔고

  



  