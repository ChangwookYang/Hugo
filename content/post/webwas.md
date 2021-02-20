---
title: "WEB, WAS?"
date: 2021-02-17T23:30:00+09:00
---



### 웹서버(WEB)란?

> 웹서버는 작성된 html페이지 등을 네트워크 망에 종속되지 않고, 
>
> 웹서비스 할 수 있도록하는 어플리케이션

- 웹서버(소프트웨어) : 웹 브라우저 클라이언트로부터 HTTP 요청을 받아들이고,

  ​					HTML문서와 같은 웹 페이지에서 흔히 찾아 볼 수 있는 자료 콘텐츠에 따라 

  ​					HTTP에 반응하는 컴퓨터 프로그램

* 웹서버(하드웨어) : 위에 언급한 기능을 제공하는 컴퓨터 프로그램을 실행하는 컴퓨터



### 웹 애플리케이션 서버(WAS, Web Application Server)란?

> 웹 서버 + 웹 컨테이너

인터넷 상에서 HTTP를 통해 사용자 컴퓨터나 장치에 애플리케이션을 수행해주는 미들웨어(소프트웨어 엔진)

웹 애플리케이션 서버는 동적 서버 콘텐츠를 수행하는 것으로 일반적인 웹서버와 구별이 되며

주로 데이터베이스 서버와 같이 수행된다.

웹 서버 + 웹 컨테이너로 웹 상에서 사용하는 컴포넌트를 올려놓고 사용하게 되는 서버이다.

여기서 웹 컨테이너란, JSP와 Servlet을 실행시킬 수 있는 SW를 웹 컨테이너라고 한다. 

* 기본 기능
  * 프로그램 실행 환경과 데이터베이스 접속 기능을 제공한다.
  * 여러 개의 트랜잭션을 관리한다.
  * 업무를 처리하는 비즈니스 로직을 수행한다.
  * Web Service 플랫폼으로서의 역할

![image](https://t1.daumcdn.net/cfile/tistory/999FA3335B7E4A5924)



### 웹 컨테이너 (Web Container)

JSP와 서블릿을 실행시킬 수 있는 소프트웨어를 웹 컨테이너 혹은 서블릿 컨테이너라고 한다.

웹서버에서 JSP를 요청하면 톰캣에서는 JSP파일을 서블릿으로 변환하여 컴파일을 수행하고

서블릿 수행결과를 웹서버에게 전달하게 된다.

JSP 컨테이너가 탑재되어 있는 WAS는 JSP 페이지를 컴파일 해 동적인 페이지를 생성한다.

Servlet 컨테이너, JSP 컨테이너, EJB 컨테이너 등의 종류가 있다.





### WEB과 WAS 종류

#### apache란?

apache란 것은 소프트웨어 단체 이름이다.

그리고 우리가 흔히 부르는 아파치서버라는 것은 이 재단에서 후원하는

오픈소스 프로젝트 커뮤니티에서 만든 http 웹서버를 말한다.

http 웹서버는 http 요청을 처리할 수 있는 웹서버이고,

아파치 http서버는 http 요청을 처리하는 웹서버인 것이다.

클라이언트가 GET, POST, DELETE 등등의 메소드를 이용해 요청을하면 

이 프로그램이 어떤 결과를 돌려주는 기능을 한다.

즉, 아파치는 웹서버이다.



#### tomcat이란?

tomcat은 흔히 WAS(Web Application Server)라고 말한다.

WAS는 웹서버와 웹 컨테이너의 결합으로 다양한 기능을 컨테이너에 구현하여 다양한 역할을 수행할 수 있는 서버를 말한다.

클라이언트의 요청이 있을 때 내부의 프로그램을 통해 결과를 만들어내고 이것을 다시 클라이언트에 전달해주는 역할을 하는 것이 바로 웹 컨테이너 이다.

앞에서 본 아파치 웹서버와 차이는 이 컨테이너 기능이 가능하냐의 차이가 가장 크다.



### WEB과 WAS의 비교

Web Container의 유무로 WEB과 WAS를 나눌 수 있으며,

WEB 서버는 HTML 문서같은 정적 컨텐츠를 처리하는 것이고(HTTP프로토콜을 통해 읽힐 수 있는 문서)

WAS 서버는 asp, php, jsp 등 개발 언어를 읽고 처리하여 동적 컨텐츠, 웹 으용프로그램 서비스를 처리한다.

* 차이점

  목적이 다르다.

  웹서버는 정적인 데이터를 처리하는 서버이다. 이미지나 단순 html 파일과 같은

  리소스를 제공하는 서버를 웹서버를 통하면 WAS 를 이용하는것보다 빠르고 안정적이다.

  WAS는 동적인 데이터를 처리하는 서버이다. 

  DB와 연결되어 데이터를 주고 받거나 프로그램으로 데이터 조작이 필요한 경우에는 WAS를 활용해야한다.

* 우리가 만드는 웹페이지는 정적 컨텐츠와 동적 컨텐츠를 함께 노출하게 한다.

  WAS가 정적 데이터를 처리하게 되면, 동적 컨텐츠 처리가 지연될 것이고 

  이로 인해 페이지 노출시간이 늘어나게 된다.

  WAS는 동적 처리에 최적화되어있는 서비스이기 때문에

  처리속도를 위해 정적처리는 웹서버에서, 동적 컨텐츠는 WAS에서 처리하게 된다.

* 사용자가 클라이언트(브라우저)에 요청을 하게 되면 이를 웹서버에서 반응하여

  WAS의 처리를 거쳐 웹 페이지로 다시 웹서버에서 클라이언트에 응답 메시지를 준다.



### Web Server와 WAS의 구성에 따른 분류

* WAS와 WebServer를 분리하지 않는 경우

  모든 컨텐츠를 한 곳에 집중시켜 웹서버와 WAS의 역할을 동시에 수행,

  스위치를 통한 로드 밸런싱, 사용자가 적을 경우 효육적

* WAS와 WebServer를 분리한 경우

  웹서버와 WAS의 기능적 분류를 통해 효과적인 분산을 유도,

  정적인 데이터는 웹서버에서 처리, 동적인 데이터는 WAS가 처리

* WAS 여러개와 WebServer를 분리한 경우

  WAS 단을 프리젠테이션 로직과 비즈니스 로직으로 구분하여 구성,

  특정 logic의 부하에 따라 적절한 대응을 할 수 있지만

  설계단계 유지보수 단계가 복잡해질수있다.

* WAS와 WebServer를 분리하는 이유

  * 기능을 분리하여 서버의 부하 방지
  * 물리적으로 분리하여 보안 강화
  * 여러대의 WAS를 연결 가능 (로드밸런싱 역할 및 fail over, fail back 처리에 유리)
  * 여러 웹어플리케이션을 서비스 가능(java서버, c#서버 등 하나의 웹서비스를 통해 서비스가능)





참고 : https://helloworld-88.tistory.com/71


