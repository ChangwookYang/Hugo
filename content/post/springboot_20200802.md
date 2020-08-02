---
title: "PathVariable & QueryParameter"
date: 2020-08-02T23:00:00+09:00
---

## 스프링와 스프링부트

[참고링크](https://sas-study.tistory.com/274)


### 스프링은 무엇인가?
스프링 프레임워크는 자바 생태계에서 가장 대중적인 응용프로그램 **개발 프레임워크**이다.

#### 1. 의존성 주입(DI) & 제어의 역전(Ioc)
의존성 주입(DI, Dependency Injection)과 제어의 역전(Inversion of Control)은 스프링에서 가장 중요한 특징 중 하나이다.
이들을 사용하여 좀더 결합도를 낮추는 방식으로 어플리케이션을 개발할 수 있다!

```Java
/*DI가 없는 예제*/
@RestController
public class MyController {
	private MyService service = new MyService();
	
	@RequestMapping("/welcome")
	public String welcome() {
		return service.retreiveWelcomeMessage();
	}
}
```
DI가 없는 예제에서는 MyController가 MyService라는 클래스에 의존한다.
인스턴스를 얻기위해서 MyService service = new MyService();를 사용하였다.
이로 인하여 객체간의 결합도가 올라가게 되었다.

```JAVA
/*DI가 있는 예제*/
@Component
public class MyService{
	public String retriveWelcomeMessage() {
		return "Welcome to Innovation";
	}
}

@RestController
public class MyController {
	@Autowired
	private MyService service;
	
	@RequestMapping("/welcome")
	public String welcome() {
		return service.retreiveWelcomeMessage();
	}
}
```
두 개의 어노테이션으로 결합도를 낮추고 MyService 객체의 인스턴스를 쉽게 얻을 수 있다.

> @Component : 스프링의 BeanFactory라는 팩토리 패턴의 구현체에서 bean이라는 객체로 두어 스프링 관리하에 두겠다는 어노테이션
> @Autowired : 스프링 프레임워크에서 관리하는 Bean객체와 같은 타입의 객체를 찾아서 자동으로 주입해주는것

스프링 프레임워크는 MyService에 대한 bean을 생성하고 MyController에 있는 service 변수에 주입해준다.

#### 2.  AOP 관점지향 프로그래밍(Aspect Oriented Programming)
프로젝트에 security나 logging 등을 추가하고 싶을 때 기존 비지니스 로직에 손대지 않고 AOP를 활용하여 추가할 수 있다.

특정 메소드 끝, 호출 직전, 메소드 리턴직후, 예외 발생 시 등 여러 상황에 활용할 수 있다.



### 스프링 부트는 왜 필요해졌을까?

* Transaction Manager
* Hibernate Datasource
* Entity Manager
* Session Factory

Spring MVC를 사용하며 위와 같은 세팅이 개발자들을 힘들게 하였다.


#### 스프링부트는 스프링의 설정 문제를 어떻게 해결하였을까?
##### 1. Auto Configuration
스프링부트는 자동설정(AutoConfiguration)을 이용하였고 개발에 필요한 모든 내부 Dependency를 관리한다.
개발자가 해야하는 건 단지 어플리케이션을 실행할 뿐이다.
스프링의 jar 파일이 클래스 패스에 있는 경우, Spring Boot는 Dispatcher Servlet으로 자동 구성한다.
마찬가지로 Hibernate의 jar파일이 클래스패스 내에 존재한다면 datasource로 자동설정하게 된다.

##### 2. SpringBoot Starter
웹 어플리케이션을 개발하는 동안 사용하려는 jar, 사용할 jar버전, 연결방법이 필요하다.
이러한 jar들의 서로 호환되는 버전들을 따로 선택해주었어야 했는데
이런 복잡도를 줄이기위해서 스프링부트는 SpringBoot Start를 도입하였다.
한번의 스타터 Dependency를 추가하면 스프링부트 스타터 웹프로젝트가 pre-packaged된 형태로 제공되어
개발자는 Dependency 관리와 호환버전에 대하여 고려할 필요가 없게 되었다.

```xml
<!-- 스프링 프로젝트의 의존성 관리 START -->
<dependency>
   <groupId>org.springframework</groupId>
   <artifactId>spring-webmvc</artifactId>
   <version>4.2.2.RELEASE</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.5.3</version>
</dependency>
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>5.0.2.Final</version>
</dependency>
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
<!-- 스프링 프로젝트의 의존성 관리 END -->

<!-- 스프링부트 프로젝트의 의존성 관리 START -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<!-- 스프링부트 프로젝트의 의존성 관리 END -->
```

