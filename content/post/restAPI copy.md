---
title: "REST API Designing guidelines"
date: 2020-07-18T16:32:21+09:00

---

[RESTful API Designing guidelines](https://wayhome25.github.io/etc/2017/11/26/restful-api-designing-guidelines/) 번역해주신거에서 정리해봤다.

API는 **개발자들이 데이터와 상호작용을 하기 위한 인터페이스**이다.    
잘 디자인된 API는 사용하기 쉽고, 개발자들의 삶을 편하게 만들어준다.  
API는 개발자들을 위한 GUI이며, 만약 사용하기 혼란스러우면 개발자들은 다른 API를 찾게될것이다.  
개발자 경험은 API 질을 측정함에 있어서 가장 중요한 기준이라 할 수 있다.  


### 1) 용어
다음은 REST API에 관련된 가장 중요한 용어들이다.

* **Resource**는 어떤 것의 대표 혹은 객체이다.  
  이는 관련 데이터를 갖고 있고, 이를 운용하기 위한 메소드 집합을 가질 수 있다.  
  ex) Animals, Schools, Employees는 Resources이고  
      delete, add, update는 Resources들에서 수행되는 operations이다.
* **Collections**는 Resouces의 집합이다.  
  ex) Companies는 Company resource의 집합이다.  
* **URL**(Uniform  Resource Locator)은 어느 Resource가 어디에 위치할 수 있고  
  어떤 action들이 수행될 수 있는지를 나타내는 경로이다.


### 2) API endpoint  
Employees를 가진 Companies에 대한 API를 작성해보자.  
**/getAllEmployees**는 employees의 리스트를 response하는 API이다.  

* Company 관련 API 목록
  * /addNewEmployee
  * /updateEmployee
  * /deleteEmployee
  * /deleteAllEmployee
  * /promoteEmployee
  * /promoteAllEmployee

업데이투 예정