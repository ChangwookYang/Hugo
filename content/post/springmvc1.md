---
title: "스프링 MVC (1)"
date: 2021-03-10T22:00:00+09:00우
---

---

---

  

  

## WEB / WAS

#### 모든 것이 HTTP

HTTP 메시지에 모든 것을 전송

* HTML, TEXT
* IMAGE, 음성, 영상, 파일
* JSON, XML (API)
* 거의 모든 형태의 데이터 전송 가능
* 서버 간에 데이터를 주고 받을 때도 대부분 HTTP 사용
* 지금은 HTTP 시대!



#### 웹 서버

* HTTP 기반으로 동작

* 정적 리스소 제공, 기타 부가기능

* 정적 HTML, CSS, JS, 이미지, 영상
* NGINX, APACHE



#### 웹 애플리케이션 서버(WAS - Web Application Server)

* HTTP 기반으로 동작
* 웹 서버 기능 포함 + (정적 리소스 제공가능)
* 프로그램 코드를 실행해서 애플리케이션 로직 수행
  * 동적 HTML, HTTP API(JSON)
  * 서블릿, JSP, 스프링 MVC
* 톰캣(Tomcat) Jetty, Undertow
* JAVA의 경우 서블릿 컨테이너 기능을 제공하면 WAS
* WAS는 애플리케이션 코드를 실행하는데 더 특화되어 있다.



#### 웹시스템 구성 - WEB, WAS, DB

* 효율적인 리소스 관리
  * 정적 리소스가 많이 사용 되면 Web 서버 증설
  * 애플리케이션 리소스가 많이 사용되면 WAS 증설





## 서블릿



#### 웹 애플리케이션 서버를 직접 구현한다면?



##### 웹 브라우저가 생성한 요청 HTTP 메시지

>POST /save HTTP/1.1
>
>Host: localhost:8080
>
>Content-Type: application/x-www-form-urlencoded
>
>
>
>username=kim&age=20



##### 서버에서 HTTP 응답 메시지 생성

>HTTP/1.1 200 OK
>
>Content-Type: text/html;charset=UTF-8
>
>Content-Length: 3423
>
>
>
><html>
>
>​	<body>...</body>
>
></html>

1. 서버 TCP/IP 연결  대기, 소켓 연결
2. HTTP 요청 메시지를 파싱해서 읽기
3. POST 방식, /save URL 인지
4. Content-Type 확인
5. HTTP 메시지 바디 내용 파싱
   * username, age 데이터를 사용할 수 있게 파싱
6. 저장 프로세스 실행
7. 비지니스 로직 실행  >>>>>>>> **의미있는 비지니스 로직**
   * 데이터베이스에 저장  요청
8. HTTP 응답 메시지 생성 시작
   * HTTP 시작 라인 생성
   * Header 생성
   * 메시지 바디에 HTML 생성해서 입력
9. TCP/IP에 응답 전달, 소켓 종료



#### !! 의미있는 비즈니스 로직을 제외하고 서블릿이 처리해준다.

#### 서블릿 - HTTP 요청, 응답 흐름

* HTTP 요청시
  * WAS는 Request, Response 객체를 새로 만들어서 서블릿 객체 호출
  * 개발자는 Request 객체에서 HTTP 요청 정보를 편리하게 꺼내서 사용
  * 개발자는 Response 객체에서 HTTP 응답 정보를 편리하게 입력
  * WAS는 Response객체에 담겨있는 내용으로 HTTP 응답정보를 생성










출처 : 인프런 - 스프링MVC(김영한 님)

