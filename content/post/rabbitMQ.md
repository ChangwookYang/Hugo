---
title: "RabbitMQ"
date: 2021-01-21T00:05:00+09:00
---
## RabbitMQ

비동기 작업 큐를 사용하려면 중간 단계에 Broker라고 부르는 메시지큐가 등장한다

RabbitMQ, ActiveMQ, ZeroMQ, Kafka가 대표적이다

RabbitMQ는 얼랭(Erlang)으로 AMQP를 구현한 메시지 브로커 시스템이다.



### AMQP

클라이언트가 메시지 미들웨어 브로커와 통신할 수 있게 해주는 메세징 프로토콜이다.

메시지를 발행하는 Producer에서 

Broker의 Exchange로 메시지를 전달하면 Binding이라는 규칙에 의해 연결된 Queue로 메시지가 복사된다.

메시지를 받아가는 Consumer에서는 브로커의 Queue를 통해 매시지를 받아 처리한다.

![image](https://blog.dudaji.com/assets/rabbitmq/MessageFlow.png)

### Exchange와 Queue 를 연결하는 Bindings

모든 메시지는 Queue로 직접 전달되지 않고 반드시 Exchange에서 먼저받는다

그리구 Exchange Type과 Biding 규칙에 따라 적절한 Queue로 전달된다.

* Name : Exchange 이름
* Type : 메시지 전달방식
  * Direct Exchange
    * 메시지에 포함된 routing key를 기반으로 queue에 메시지를 전달한다.
    * 아래 그림에서 Exchange X는 2개의 Queue와 연결되어 있다.
    * Q1은 orange라는 binding key로 연결되고, Q2는 black, green 2개의 binding key로 연결된다.
    * Exchange X로 전달된 메시지의 routing key가 orange 일 경우 Q1으로 전달되고
    * black, green일 경우 Q2로 전달되고 그 밖의 다른 메시지는 무시된다.
    * ![image](https://www.rabbitmq.com/img/tutorials/direct-exchange.png)
  * Fanout Exchange
  * Topic Exchange
  * Headers Exchange
* Durability : 브로커가 재시작될때 남아있는지 여부(durable, transient)
* Auto-delete : 마지막 Queue 연결이 해제되면 삭제
![이미지](https://jonnung.dev/images/rabbitmq-exchange.png)



---

### 메시지를 보관하는 Queue

Consumer 어플리케이션은 Queue를 통해 메시지를 가져간다. 

Queue는 반드시 미리 정의해야 사용할 수 있다.

* Name : queue 이름. amq. 로 시작하는 이름은 예약되어 사용할 수 없다.

* Durability : durable은 브로커가 재시작 되어도 디스크에 저장되어 남아있고

  ​				transient으로 설정하면 브로커가 재시작되면 사라진다. 

  ​				단, Queue에 저장되는 메시지는 내구성을 갖지않는다.

* Auto delete : 마지막 Consumer 가 구독을 끝내는 경우 자동으로 삭제된다.
* Arguments : 메시지 TTL, Max Length 같은 추가기능을 명시한다.



----

### 하나의 연결을 공유하는 Channels

Consumer 어플리케이션에서 Broker로 많은 연결을 맺는 것은 바람직하지 않다.

rabbitMQ는 channel이라는 개념을 통해 하나의 TCP 연결을 공유해서 사용할 수 있는 기능을 제공한다.




---
---

> 참고
>
> https://jonnung.dev/rabbitmq/2019/02/06/about-amqp-implementtation-of-rabbitmq/
>
> https://blog.dudaji.com/general/2020/05/25/rabbitmq.html
> 
> https://velog.io/@hellozin/Spring-Boot%EC%99%80-RabbitMQ-%EC%B4%88%EA%B0%84%EB%8B%A8-%EC%84%A4%EB%AA%85%EC%84%9C

