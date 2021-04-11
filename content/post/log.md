---
title: "log 간단정리"
date: 2021-04-11T23:00:00+09:00
---

운영 시스템에서는 sysout을 사용하면 안된다.

로깅라이브러리로는 spring-boot-starter-logging가 함께 포함된다.

로그 라이브러리는 Logback, Log4J, Log4J2 등등 수많은 라이브러리가 있는데

그것을 통합해서 인터페이스로 제공하는 것이 SLF4J 라이브러리이다.

```java
// 로그 선언
private Logger log = LoggerFactory.getLogger(getClass());
private static final Logger log = LoggerFactory.getLogger(Xxx.class);
// +++ @Slf4j 롬복 사용 가능
```

##### 로그 호출

```java
log.info("hello");
System.out.println("hello");
```

시스템 콘솔로 직접 출력하는 것보다 로그를 사용하면 많은 장점이 있다.

##### @RestController

* @Controller 반환 값이 String이면 뷰 이름으로 인식된다. 뷰를 찾고 뷰가 렌더링된다.
* @RestController는 반환값으로 뷰를 찾는게 아니라 HTTP 메시지 바디에 바로 입력한다.

##### 로그 레벨

```properties
# application.properties
# 전체 레벨 설정
logging.level.root=info

#hello.springmvc 패키지와 그 하위 로그 레벨 설정
logging.level.hello.springmvc=debug
```

##### 로그 사용 시 장점

* 쓰레드 정보, 클래스 이름같은 부가 정보를 볼 수 있고, 출력 모양을 조정할 수 있다.
* 로그 레벨에 따라 개발서버에서는 모든 로그를 출력하고
* 운영서버에서는 출력하지 않는 등의 로그를 상황에 맞게 조절할 수 있다.
* 시스템 아웃 콘솔에만 출력하는 것이 아니라, 파일이나 네트워크 등 로그를 별도의 위치에 남길수있다.
* 파일로 남길때는 일별, 특정 용량에 따라 로그를 분할하는 것도 가능하다.
* 성능도 System.out보다 좋다.(내부 버퍼링, 멀티 쓰레드 등등) 그래서 실무에서는 꼭 로그를 쓴다.

