---
title: "Spring 3대 특징"
date: 2020-10-26T23:10:00+09:00
---

# Spring 3대 특징



[출처](https://asfirstalways.tistory.com/334)

### - POJO (Plain Old Java Object)

Servlet 형식에 맞추지 않고 추상화되어 라이브러리에 들어가 있으므로

개발자는 비지니스 로직에만 집중하면 된다.



### - IOC와 AOP를 지원하는 경량의 컨테이너 프레임워크

* 컨테이너란?

  컨테이너는 특정 객체의 생성과 관리를 담당하며 객체 운용에 필요한 다양한 기능을 제공한다.

  애플리케이션 운용에 필요한 객체를 생성하고 객체 간의 의존관계를 관리한다는 점에서 스프링도 컨테이너이다.

* 스프링 컨테이너의 종류

  BeanFactory와 이를 상속한 ApplicationContext 두 가지 컨테이너를 제공한다.

  BeanFactory는 스프링 설정파일에 등록된 bean 객체를 생성하고 관리하는 가장 기본적인 컨테이너 기능을 제공한다.

  컨테이너가 구동될 때 객체가 생성하는 것이 아니라 클라이언트로부터의 요청에 의해서만 객체를 생성한다.(lazy loading)

  이를 확장한 ApplicationContext 컨테이너는 이 기능에 더해 트랜잭션 관리나 메시지 기반의 다국어 처리 등 다양한 기능을 지원한다.

  또한 컨테이너가 구동되는 시점에 bean에 등록되어 있는 클래스들을 객체화하는 즉시 로딩 방식으로 동작한다.

  * 정리하자면 스프링 컨에티너는

    bean 저장소에 해당하는 xml 설정파일을 참조하여 bean의 생명주기를 관리하고 여러 서비스를 제공한다.



* IoC / DI (Inversion of Control / Dependency Injection) - 제어의 역전 / 의존성 주입

  비즈니스 컴포넌트를 개발할 때 항상 신경쓰는 것이 낮은 결합도와 높은 응집도 이다.

  스프링은 제어의 역행을 통해 애플리케이션을 구성하는 객체 간의 느슨한 결합, 즉 낮은 결합도를 유지한다.

  스프링의 IoC는 객체 생성을 자바 코드로 직접 처리하는 것이 아니라 컨테이너가 대신 처리하게 한다.

  그리고 객체와 객체 사이의 의존 관계 역시 컨테이너가 처리한다.

* 의존성 주입(DI)은 무엇인가?

  의존성 관계란 객체와 객체의 결합 관계이다.

  하나의 객체에서 다른 객체의 변수나 메소드를 이용해야 한다면, 

  이용하려는 객체에 대한 객체 생성과 생성된 객체의 레퍼런스 정보가 필요하다.

  즉, 쉽게 말해서 이존성은 new다. A라는 객체 생성자에서 new B();를 했다면 A는 B에 의존하게 된다.

  주입이란 외부에서라는 뜻을 내포하고 있다.

  즉 A라는 객체에서 B를 생성하는 것이 아니라 외부에서 생성된 B를 A에 주입함으로써 의존관계를 없앨 수 있다.

  이것을 의존성 주입이라고 한다. 





* AOP (Aspect Oriented Programming)

  관점지향 프로그래밍

  DI가 의존성에 대한 주입이라면 AOP는 로직 주입이다.

  코드를 작성하다보면 다수의 모듈에 공통적으로 나타나는 부분이 존재하는데 이를 횡단 관심이라한다.

  그리고 모듈 각각 고유한 로직을 핵심 관심사라고 한다.

  즉 각 모듈을 구성하고 있는 코드는 핵심관심사와 횡단관심사가 합쳐진 것이다.

  AOP는 모듈마다 중복되는 부분을 걷어내는 것이 주목적이다.






* PSA (Portable Service Abstraction)
  
  일관성 있는 서비스 추상화이다.
  
  JDBC처럼 어댑터 패턴을 적용하여 같은 일을 하는 다수의 기술을 
  
  공통의 인터페이스로 제어할 수 있게 한것을 서비스 추상화라고 한다.
  
  스프링 프레임워크에서는 서비스 추상화를 위해 다양한 어댑터를 제공한다.
  
  스프링은 OXM, ORM, 캐시, 트랜잭션 등 다양한 기술에 대한 PSA 즉 API를 제공한다.
  
  
  
  