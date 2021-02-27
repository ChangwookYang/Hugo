---
title: "생활코딩 - AWS(1)"
date: 2021-02-27T22:00:00+09:00
---


> 클라우드 컴퓨팅 : 인터넷에 연결되어 있는 거대한 컴퓨터를 사용한다.



#### Region 지역 

전세계에 아마존의 컴퓨터가 여러 곳에 흩어져 있다.

거리가 멀수록 경유지가 많아 병목이 발생할 수 있어 네트워크 속도에 영향을 준다.

주고객의 위치에 따라 Region의 위치를 정해야한다.

http://www.cloudping.info 를 통해서 위치에 따라 웹브라우저 기준 어떤 지역이 속도가 빠른지 알 수 있다.



#### Availabilty zone 가용 영역

Region1, Region2가 있다.

각각의 Region에는 여러 건물이 있다. az1, az2, ... (자연 재해 등 백업 기능을 위해)

각각의 건물 사이는 전용선으로 연결되어 데이터를 주고 받는 것이 같은 건물에 있는 것처럼 빠르다.



#### AWS EC2 (Elastic Compute Cloud)

> 독립된 컴퓨터를  통쨰로 임대해 주는 서비스

* 인스턴스 : 컴퓨터 한 대를 인스턴스
  * 임대하고 있는 컴퓨터 현황을 볼 수 있다.
* 인스턴스 타입
  * Choose AMI(Amazon Machine Image) : 운영체제를 선택(Unix / Window)
  * Choose an Instance Type : 사양을 선택
    * vCPUs : 몇개의 CPU (v : virtual)
    * Memory(GiB) : 메모리 크기
* 가격 정책
  * 온 디맨드 인스턴스 (필요할때마다 키고 끄고 할 수 있는 인스턴스)







