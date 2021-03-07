---
title: "AWS 정리(3)"
date: 2021-03-07T01:00:00+09:00
---

### 클라우드에서의 보안

##### 보안 그룹

* AWS 리소스에 대한 인바운드 및 아운바운드 트래픽을 제어하는 가상 방화벽
* 트래픽은 모든 IP프로토콜, 포트 또는 IP주소로 제한될 수 있다.
* 규칙은 상태 저장



##### 네트워크 ACL (액세스 제어 목록)

* 서브넷 경계의 방화벽
* 기본적으로 모든 인바운드 및 아웃바운드 트래픽을 허용한다.
* 상태 비저장이므로 인바운드 및 아웃바운드 트래픽 모두에 대한 명시적인 규칙이 필요하다.
* 사용자 지정 ACL은 기본적으로 모든 인바운드 / 아웃바운드 트래픽을 거부한다.



##### 가상 프라이빗 게이트웨이 (VGW)

* Amazon VPC와 다른 네트워크 사이에 프라이빗 연결(VPN)을 설정할 수 있다. 
* 온프레미스 네트워크를 AWS로 확장(VPN 연결)



##### AWS Direct Connect (DX)

* 전용 프라이빗 네트워크 연결을 제공
* 데이터 전송 비용 감소
* 예측 가능하게 속도 및 규모에 일관성을 가지고 액세스 가능
* VGW와 온프레미스 환경을 연결 가능



##### VPC 연결 - VPC 피어링

* 프라이빗 IP 주소를 사용하여 내부 및 리전간 지원
* 인터넷 게이트웨이 또는 가상 게이트웨이가 필요없다.



##### VPC 엔드포인트

* AWS를 벗어나지 않고 EC2 인스턴스를 VPC 외부 서비스와 프라이빗하게 연결한다.
* 인터넷 게이트웨이, VPN, NAT 디바이스 또는 방화벽 프록시를 사용할 필요가 없다.
* VPC와 다른 AWS 서비스 간에 프라이빗 연결이 가능하다.



---

### AWS 기반 로드 밸런싱

##### Elastic Load Balancing (ELB)

수신되는 애플리케이션 트래픽을 여러 Amazon EC2 인스턴스, 컨테이너 및 IP 주소를 걸쳐 

분산하는 관리형 로드 밸런싱 서비스

* HTTP, HTTPS, TCP, UDP, SSL 프로토콜을 사용
* 각 로드 밸런서에 DNS 이름이 부여된다.
* 비정상 인스턴스를 인식하고 대응한다.



##### Application Load Balancer

* 개방형 시스템 간 상호 연결 모델의 일곱번째 계층인 애플리케이션 계층에서 작동한다.
* EC2 인스턴스 또는 컨테이너에서 실행되는 웹사이트 및 모바일 앱은 ALB의 이점을 가진다.
* HTTP, HTTPS 트래픽의 고급 로드 밸런싱



##### Network Load Balancer

* 클라이언트로부터 수신되는 트래픽을 수락하고

  이 트래픽을 동일한 가용 영역 내 대상 전체로 분산한다.

* NLB는 연결 수준(계층 4)에서 작동하며 IP 프로토콜 데이터에 따라 연결을

  EC2 인스턴스, 컨테이너 및 IP 주소로 라우팅한다.

* TCP, TLS, UDP 트래피의 로드 밸런싱



##### ELB 사용이유

* 고가용성 : 트래픽을 가용영역 또는 여러 가용영역에 있는 여러 대상(EC2, 컨테이너, IP주소)에 자동분산
* 상태확인 : 주기적으로 핑을 보내어 상태확인
* 보안 기능 : ELB 로드 밸런서는 보안 그룹과 같은 네트워크 보안 그룹을 활용할 수 있다.



##### 고가용성 이란?

애플리케이션은 허용되는 성능 저하 시간 내에 장애로부터 복구하거나 보조 소스로 이동할 수 있다.

가능한 모든 지점에 중복성을 구현하여 단일 장애로 전체 시스템이 중단되지 않도록한다.



##### Amazon Route 53

* Route 53은 가용성과 확장성이 뛰어난 클라우드 DNS 서비스이다.

  

