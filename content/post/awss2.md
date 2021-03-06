---
title: "AWS 정리(2)"
date: 2021-03-06T21:00:00+09:00
---

#### Amazon Virtual Private Cloud (VPC)

* AWS Cloud의 프라이빗 네트워크 공간
* 워크로드에 대한 논리적 격리를 제공한다.
* 리소스에 대한 사용자 지정 액세스 제어 및 보안 설정을 허용한다.
* 점유 리소스에 대한 특정 CIDR 범위를 생성할 수 있다.



##### 서브넷을 사용하여 VPC 분리

Amazon VPC를 사용하면 고객들이 가상 네트워크를 생성하고 이를 서브넷으로 분할할 수 있다.

VPC서브넷은 특정 가용 영역에 매핑된다.

따라서 서브넷 배치는 EC인스턴스를 여러 위칭 올바르게 분산할 수 있도록 보장하는 하나의 메커니즘이다.

서브넷을 생성할 때는 VP CIDR블록의 하위 집합에 속하는 CIDR블록은 지정해야한다.

각 서브넷은 하나의 가용역역 내에 모두 상주해야하며,

다른 영역으로 확장할 수는 없다.



##### 서브넷 : 키 속성

서브넷은 VPC CIDR 블록의 하위 집합이다.

서브넷 CIDR 블록은 중첩될 수 없다.

각 서브넷은 하나의 가용 영역 내에서만 존재한다.

가용역역에 서브넷이 여러개 포함될 수 있다.

AWS는 각 서브넷에서 5개의 IP 주소를 예약한다.



##### 라우팅 테이블 

* VPC 리소스 간에 트래픽을 연결하는데 필요하다.

* 각 VPC에는 주요 라우팅 테이블이 있다.

* 사용자 지정 라우팅 테이블을 생성할 수 있다.

* 모든 서브넷에는 연결된 라우팅 테이블이 있어야 한다.

  * 라우팅 테이블은 네트워크 트래픽이 향하는 방향을 결정하는데 사용되는 경로
* VPC를 생성하면, 자동으로 기본 라우팅 테이블이 생성된다.



##### 서브넷을 통해 허용되는 다양한 네트워크 격리

서브넷을 사용하여 인터넷 액세스 접근성을 정의한다.

* 퍼블릭 서브넷

  * 퍼블릭 인터넷에 대한 인바운드/아웃바운드 액세스를 지원하도록

    인터넷 게이트웨이에 대한 라우팅 테이블 항목을 포함한다.

* 프라이빗 서브넷

  * 인터넷 게이트웨이에 대한 라우팅 테이블 항목이 없다.

  * 퍼블릭 인터넷에서 직접 액세스 할 수 없다.

  * 일반적으로 제한된 아웃 바운드 퍼블릭 인터넷 액세스를 

    지원하기 위해 NAT 게이트웨이를 사용한다.



##### 퍼블릭 서브넷에 인터넷 연결

* 인터넷 게이트웨이
  *  VPC의 인스턴스와 인터넷 간에 통신을 허용한다.
  * 기본적으로 가용성이 뛰어나고 중복적이며, 수평적으로 확장된다.
  * 인터넷으로 라우팅 가능한 트래픽에 대한 서브넷 라우팅 테이블에 대상을 제공한다.



##### 프라이빗 서브넷에 인터넷 연결

* NAT 게이트웨이

  *  프라이빗 서브넷의 인스턴스가 

    인터넷 또는 다른 aws 서비스로의 아웃바운드 트래픽을 시작하도록 활성화한다.

  * 프라이빗 인스턴스가  인터넷에서 인바운드 트래픽을 수신하는 것을 차단한다.



##### 기본 서브넷 구성

* 서브넷을 설정하기 위한 가장 좋은 방법을 잘 모르는 경우

  * 가용 영역당 1개의 퍼블릭 서브넷과 1개의 프라이빗 서브넷으로 시작한다.

  * 서브넷은 인터넷 접근성을 정의하는데 사용되어야 하므로

    가용영역 당 1개의 퍼블릭 서브넷과 1개의 프라이빗 서브넷보다 많을 필요는 없다.

  * 인터넷에 직접 액세스해야하는 모든 리소스(퍼블릭 로드밸랜서, NAT 인스턴스, 등)은 퍼블릭

    모든 다른 인스턴스는 프라이빗 서브넷으로 간다.
