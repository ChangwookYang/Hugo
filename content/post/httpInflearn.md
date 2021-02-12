---
title: "HTTP 웹 기본지식(1)"
date: 2021-02-12T22:30:00+09:00
---


#### 인터넷 네트워크

![image](https://user-images.githubusercontent.com/66955409/107770274-833b3580-6d7c-11eb-8fef-7ccf0a43d7f9.png)

복잡한 인터넷망을 이해하기 위해서는 IP에 대해서 알아야한다.



#### IP

![image](https://user-images.githubusercontent.com/66955409/107770397-aa920280-6d7c-11eb-95ff-9eb7cb5688b2.png)

* 지정한 IP 주소에 데이터 전달

* 패킷(Packet)이라는 통신 단위로 데이터 전달

  패킷 정보 : 출발지 IP, 목적지 IP, 기타 ...

  ![image](https://user-images.githubusercontent.com/66955409/107770559-f349bb80-6d7c-11eb-8c36-25303d38defe.png)

  ![image](https://user-images.githubusercontent.com/66955409/107770654-17a59800-6d7d-11eb-8665-cc747a71206e.png)



* IP 프로토콜의 한계
  * 비연결성 : 패킷을 받을 대상이 업서나 서비스 불능 상태여도 패킷 전송
  * 비신뢰성 : 중간에 패킷이 사라지면? 패킷이 순서대로 안오면?
    * Hello / World!  =>  World! / Hello 로 도착
  * 프로그램 구분 : 같은 IP를 사용하는 서버에서 통신하는 애플리케이션이 둘 이상이면?
  * IP 프로토콜로는 해결할 수 없다! TCP/UDP 두두둥장!



#### TCP / UDP

##### 인터넷 프로토콜 스택의 4계층

>애플리케이션 계층 - HTTP, FTP
>
>전송 계층 - TCP, UDP
>
>인터넷 계층 - IP
>
>네트워크 인터페이스 계층

![image](https://user-images.githubusercontent.com/66955409/107771585-4e2fe280-6d7e-11eb-97a8-aba2c82b9a34.png)

![image](https://user-images.githubusercontent.com/66955409/107771833-b7aff100-6d7e-11eb-93f8-cd77397ef0d5.png)



* TCP 특징 (전송 제어 프로토콜 Transmission Control Protocol)

  * 연결지향 - TCP 3 way handshake (가상 연결)
  * 데이터 전달 보증
  * 순서 보장
  * 신뢰할 수 있는 프로토콜
  * 현재는 대부분 TCP 사용

  * TCP 3 way handshake (SYN : 접속 요청, ACK : 요청 수락)

    1. 클라이언트 -> 서버 : SYN 

    2. 서버 -> 클라이언트 : SYN + ACK 

    3. 클라이언트 -> 서버 : SYN (둘이 가상 연결됨을 인식)

    4. 데이터 전송

  * <img src="https://user-images.githubusercontent.com/66955409/107772565-bcc17000-6d7f-11eb-8009-c26f7719db80.png" alt="image" style="zoom:50%;" />



* UDP 특징 (사용자 데이터그램 프로토콜 User Datagram Protocol)
  * 하얀 도화지에 비유(기능이 거의 없음)
  * 연결지향 - TCP 3 way hand shake X
  * 데이터 전달 보증 X
  * 순서 보장 X
  * 데이터 전달 및 순서가 보장되지 않지만, 단순하고 빠름
  * 정리
    * IP 와 거의 같다. PORT, 체크섬 정도만 추가
    * 애플리케이션에서 추가 작업 필요



#### PORT

<img src="https://user-images.githubusercontent.com/66955409/107773606-40c82780-6d81-11eb-95ba-5ad6aebe72ab.png" alt="image" style="zoom:50%;" />


서버안에서 돌아가는 애플리케이션을 구분하는 용도로 PORT

패킷정보

>출발지 IP, PORT / 목적지 IP, PORT / 전송 데이터 ...
>
>비유하자면 IP는 아파트명, PORT는 몇동 몇호

* 0 ~ 65535 할당 가능
* 0 ~ 1023 : 잘 알려진 포트 ,사용하지 않는 것이 좋음
  * FTP - 20, 21
  * TELNET - 23
  * HTTP - 80
  * HTTPS - 443



#### DNS (도메인 네임 시스템 Domain Name System)

* IP는 기억하기 어렵다, 변경될 수 있다.
* 전화번호부, 도메인 명을 IP 주소로 변환

<img src="C:\Users\욱창양\AppData\Roaming\Typora\typora-user-images\image-20210212223044040.png" alt="image-20210212223044040" style="zoom:50%;" />





#### URI (Uniform Resource Identifier 리소스를 식별하는 통합된 방법)

* URI? URL? URN?
* URI는 로케이터(Locator), 이름(Name) 또는 둘다 추가로 분류될 수 있다.

<img src="https://user-images.githubusercontent.com/66955409/107774586-8b966f00-6d82-11eb-839c-a2aa03c9d0ac.png" alt="image" style="zoom:50%;" />

<img src="https://user-images.githubusercontent.com/66955409/107774667-a1a42f80-6d82-11eb-9f0c-387d192f3048.png" alt="image" style="zoom:50%;" />

* URI 단어 뜻
  * Uniform : 리소스 식별하는 통일된 방식
  * Resource : 자원, URI로 식별할 수 있는 모든 것 
    * 실시간 교통정보 등 모든 정보
  * Identifier : 다른 항목과 구분하는데 필요한 정보
* URL, URN
  * URL - Locator : 리소스가 있는 위치를 지정
  * URN - Name : 리소스에 이름을 부여
  * 위치는 변할 수 있지만, 이름은 변하지 않는다.
  * URN은 보편적으로 사용하지 않아, URI를 URL로 같은 의미로 쓰일 수 있다.

* URL 전체 문법
  * scheme://[userinfo@]host\[:port]\[/path]\[?query][#fragment]
  * https://www.google.com:443/search?q=hello&hl=ko#3
    * 프로토콜 : https
    * 호스트명 : www.google.com
    * 포트번호 : 443
    * 패스 : /search
    * 쿼리 파라미터 : q=hello&hl=ko
    * fragment : html 내부 북마크 등에 사용, 서버에 전송하는 정보는 아니다.



#### 웹브라우저 요청 흐름











참고 : https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard