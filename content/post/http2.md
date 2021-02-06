### HTTP 프로토콜



#### 정의 : 인터넷에서 데이터를 주고 받을 수 있는 규칙

---

#### 설명

* 요청 : 데이터 주세요! client -> server : request
* 응답 : 여기 데이터~! server -> client : response

이러한 응답과 요청을 일정한 규칙으로 통신하는데 이 규칙을 http라고 한다.

![image](https://media.vlpt.us/images/jch9537/post/ae79ffd3-360a-4782-abc2-5cdd11071422/image.png)

왼쪽은 주문서, 오른쪽은 영수증 정도로 이해하면 된다.



#### 상세

* 요청

  ![image](https://media.vlpt.us/images/jch9537/post/9932af0c-2a25-4558-8b9f-93f01375f580/image.png)

* 응답

  ![image](https://media.vlpt.us/images/jch9537/post/ca86f5c2-068a-47c9-aad2-a10a64706041/image.png)



---



---

#### HTTP 프로토콜이란?

HTTP 프로토콜은 웹 브라우저와 웹 서버 사이의 데이터 통신 규칙을 말한다.

![image](https://t1.daumcdn.net/cfile/tistory/2104874555A214D53A)

즉 HTTP 통신 규약에 맞추어서 요청한다면

HTML 파일을 요청하면 HTML파일을 보내주고, 이미지 파일을 요청하면 이미지 파일은 보내준다.



http 요청 응답에 대한 내부적으로 어떻게 동작하고 있는가?



#### 요청과정(Request)

요청라인, 요청헤더, 공백라인과 요청 데이터로 구성

GET /web04/member/list HTTP/1.1

Host:localhost:9999

Cache-Control: max-age=0

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,\*/\*;q=0.8

User-Agent: Mozila/5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko)

Chrome/43.0.2357.132 Safari/537.36

Accept-Encoding: gzip, deflate, sdch

Accept-Language: ko-KR,ko;q=0.8,en-US;q=0.6,en;q=0.4



##### 요청 라인(Request-Line)

![image](https://t1.daumcdn.net/cfile/tistory/233A984555A214D718)

##### 요청 헤더

* 요청이나 응답 모두에 적용할 수 있는 일반헤더(General Header)
* 요청 또는 응답 둘 중에 하나에만 적용할 수 있는 요청 헤더 또는 응답헤더
* 보내거나 받는 본문 데이터를 설명하는 엔티티 헤더







참고 : https://velog.io/@jch9537/WEB-HTTP