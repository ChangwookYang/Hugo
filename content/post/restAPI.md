---
title: "REST API"
date: 2020-07-17T23:32:21+09:00
---

## REST API란 무엇인가?  
  
[얄코 REST API](https://www.youtube.com/watch?v=iOueE9AXDQQ&feature=youtu.be) 이 동영상 보고 정리함!  

### REST란?  
REST(**RE**presentational **S**tate **T**ransfer)는 인터넷 상의 컴퓨터 시스템간 상호운용성을 제공하는 방법 중 하나!

REST는 HTTP 기반으로 필요한 자원에 접근하는 방식을 정해놓은 네트워크 아키텍쳐이다.
자원이란? 저장된 데이터(DBMS)는 물론, 이미지/동영상/문서(PDF등)/서비스(이메일전송, 푸쉬메시지 등) 모두 포함된다.

### API란?  
소프트웨어가 다른 소프트웨어로부터 지정된 형식으로 요청, 명령을 받을 수 있는 
수단을  
API(Application Programming Interface)라고 한다.  
  
* 자판기버튼, 키보드, 마우스는 인터페이스
* 기계와 기계 사이, 소프트웨어와 소프트웨어 사이에 Interface
* 로컬 브라우저는 Web API를 활용하여 자바스크립트로부터 특정 동작들을 지시받기도한다.
  
### REST API란?
REST기반으로 서비스 API를 구현한 것을 REST API라고 한다.
**프론트엔드 웹에서 서버에 데이터를 요청하거나 할때 사용되는 것이 REST란 형식의 API이다.**  

  ---



REST API의 가장 큰 특징은  
*각 요청이 어떤 동작이나 정보를 위한 것인지 **요청의 모습 자체로 추론가능**하는 것*이다.  
REST API와 같이 형식에 상관없이 기능만 가능하게 물론 만들 수 있으나 개발자는 혼자가 아니다.  
  
자원을 구조와 함께 나타내는 이런 형태의 구분자를 URI라고 한다.  
>   http://(도메인)/classes/1/name?count=10 (classes에서 1 idx에서 name에서 10개)  
  

 --- 

- 서버에 REST API로 요청을 할때는 **HTTP란 규약에 따라 신호를 전송**한다(HyperText Transfer Protocol)  
- 택배를 보낼때 등기, 택배, 우편 등 다양한 방법이 있듯이 HTTP로 요청을 보낼때도 여러 메소드가 있다.  
- REST API에서는 **GET, POST, DELETE, PUT, PATCH** 5가지를 사용한다.  
- CRUD : CREATE생성 READ조회 UPDATE수정 DELETE삭제  

- 소포가 편지보다 더 많은 것을 담을 수 있듯이  
 POST, PUT, PATCH에는 BODY가 있어서 정보들을 GET이나 DELETE보다 많이  
 비교적 안전하게 감춰서 신호를 보낼 수 있다.  
  
  
  
![image](https://user-images.githubusercontent.com/66955409/87797411-9852f300-c885-11ea-96ea-9a1c3922b632.png)

* GET, POST로 CRUD 모두 사용가능 하나  
  REST API에서는 각 요청에 따라 메소드들을 나누어 사용하여야 한다.(쉽게 파악하기 위해)  

CRUD | 기능
------------ | ------------- 
GET | READ, 조회
POST | CREATE, 새로운 정보 추가  
PUT, PATCH | UDPATE
DELETE | 삭제

> PUT은 해당 데이터들을 통째로 수정할때, PATCH는 정보 중 일부를 특정 방식으로 변경할때  
> ex) 도메인/classes/2/students/14 : POST에서는 14라는 idx가 필요없는데 PUT은 수정할 idx도 보냄
  
--- 

URI는 동사가 아닌 명사로 사용하여야하기때문에  
GET, POST만으로 모든것을 처리 할때 URL에 update, create등의 동작을 넣지 않는다.  
  
 POST로만 사용하게 된다면 URI에 동사들을 넣게되어 불필요할 것이다.  
ex) 도메인/classes/2/students/create, 도메인/classes/2/students/14/update  

--- 
    
결국 REST API란 HTTP 요청을 보낼때  
**어떤 URL에 어떤 메소드를 사용할지 개발자들 사이에 널리 지켜지는 약속**이다.  

형식이기 때문에 기술에 제약을 받지 않는다.  
어떤 언어든 APP, 웹 상관없이 **소프트웨어에 HTTP로 정보를 주고 받는 부분이 있다면 restful API 사용 가능!**  
인터넷에 restful api design guidelines을 검색하면 많이 나온다!  