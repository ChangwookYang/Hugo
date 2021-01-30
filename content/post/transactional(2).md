---
title: "@Transactional(2)"
date: 2021-01-31T04:30:00+09:00
---


많은 비지니스 로직이 Controller에 절차지향적으로 짜여져 있는 경우가 많다.

business를 하나의 트랜잭션 단위로 Service쪽으로 옮기는 작업을 하면

객체지향적으로 코딩할 수 있고, 재사용이 가능하며 동시에 Transaction 단위로 개발하기 쉽다.



### 트랜잭션의 성질

1. 원자성 Atomicity

   한 트랜잭션 내에서 실행한 작업들은 하나로 간주한다. 즉 모두 성공 or 모두 실패

2. 일관성 Consistency

   트랜잭션은 일관성 있는 데이터베이스 상태를 유지한다.(data integrity 만족)

3. 격리성 Isolation

   동시에 실행되는 트랜잭션들이 서로 영향을 미치지 않도록 격리해야한다.

4. 지속성 Durability

   트랜잭션을 성공적으로 마치면 결과가 항상 저장되어야 한다.



### 스프링에서 트랜잭션 처리 방법

스프링에서는 트랜잭션 처리를 지원하는데 그중 어노테이션 방식으로

@Transactional을 선언하여 사용하는 방법이 일반적이며, 선언적 트랜잭션이라 부른다.

클래스, 메서드 위에 @Transactional이 추가되면, 

이 클래스에 트랜잭션 기능이 적용된 프록시 객체가 생성된다.

이 프록시 객체는 @Transactional이 포함된 메소드가 호출될 경우,

PlatformTransactionManager를 사용하여 트랜잭션은ㄹ 시작하고, 정상 여부에 따라

Commit 또는 Rollback 한다.



#### Transactional 설정방식

> JavaConfig 방식
```java
@EnableTransactionManagerment

public class AppConfig {

	@Bean
	public PlatformTransactionManager transactionManager() throws URISyntaxException
				, GeneralSecurityException, ParseException, IOException 	{
			return new DataSourceTransactionManager(dataSource());
	}

}
```

> 기본 적용 방식

```java
@Transactional
public boolean insertUser(User user) {
	...
}
```



#### 다수의 트랜잭션이 경쟁 시 발생할  수 있는 문제

트랜잭션이 처리중이고 아직 커밋되지 않았는데 다른 트랜잭션이 그 레코드에 접근하면

다음과 같은 문제가 발생될 수 있다.

1. **Problem 1 - Dirty Read**

   트랜잭션 A가 어떤 값을 1에서 2로 변경하고 아직 커밋하지 않은 상황에서

   트랜잭션 B가 같은 값을 읽는 경우 트랜잭션 B는 2가 조회된다.

   트랜잭션 B가 2를 조회한 후 A가 롤백된다면 트랜잭션 B는 잘못된 값을 읽게 된것이다.

   즉, 트랜잭션이 완료되지 않은 상황에서 데이터에 접근을 허용할 경우 발생할 수 있는 데이터 불일치다.

2. **Problem 2 - Non-Repeatable Read**

   트랜잭션 A가 어떤 값 1을 읽었다. 이후, A는 같은 쿼리를 또 실행할 예정인데,

   그 사이에 트랜잭션 B가 1을 2로 바꾸고 커밋해버리면 A는 같은 쿼리 두번을 날리는 사이에

   두 쿼리의 결과가 다르게 되어버린다.

   즉, 한 트랜잭션에서 같은 쿼리를 두번 실행했을 떄 발생할 수 있는 데이터 불일치이다.

3. **Problem 3 - Phantom Read**

   트랜잭션 A가 어떤 조건을 사용하여 특정 범위의 값들[0, 1, 2, 3, 4]를 읽었다.

   이후 A는 같은 쿼리를 실행할 예정인데, 그 사이에 트랜잭션 B가 같은 테이블에 값[5, 6, 7]을

   추가해버리면 같은 쿼리 두번을 날리는 사이에 두 쿼리의 결과가 다르게 되어버린다.

   즉, 한 트랜잭션에서 일정 범위의 레코드를 두번 이상 읽을 때 발생하는 데이터  불일치이다.

### 스프링 트랜잭션 속성

위와 같은 상황을 방지할 수 있는 속성, 이외 별도 속성들을 확인해보자.

#### 1. isolation 격리 수준

   트랜잭션에서 일관성 없는 데이터를 허용하도록 하는 수준을 말한다.

   * Default : 기본 격리 수준(기본설정, DB의 Isolation Level을 따름)

   * READ_UNCOMMITED : 커밋되지 않는 데이터에 대한 읽기를 허용

     어떤 사용자가 A라는 데이터를 B라는 데이터로 변경하는 동안 다른 사용자는 B라는 아직 완료 되지 안흔 데이터 B를 읽을 수 있다.

     Problem1 - Dirty Read 발생

   * READ-COMMITTED (level 1)

     트랜잭션이 커밋 된 확정 데이터만 읽기 허용

     어떠한 사용자가 A라는 데이터를 B라는 데이터로 변경하는 동안 다른 사용자는 해당 데이터에 접근X

     Problem1 - Dirty Read 방지

   * REPEATABLE_READ (level 2)

     트랜잭션이 완료될 떄까지 select 문장이 사용되는 모든 데이터에 shared lock이 걸리므로

     다른 사용자는 그 영역에 해당되는 데이터에 대한 수정이 불가능하다.

     선형 트랜잭션이 읽은 데이터는 트랜잭션이 종료될 떄까지 후행 트랜잭션이 갱신하거나

     삭제가 불가능하기때문에 같은 데이터를 두번 쿼리했을 떄 일관성 있는 결과를 리턴한다.

   * SERIALIZABLE (level 3)

     데이터의 일관성 및 동시성을 위해 MVCC(Multi Version Concurrency Control)을

     사용하지 않음, MVCC는 다중 사용자 DB성능을 위한 기술로 데이터 조회 시 LOCK을 사용하지 않고

     데이터의 버전을 관리해 데이터의 일관성 및 동시성을 높이는 기술

#### 2. Propagation 전파 옵션

트랜잭션 동작 도중 다른 트랜잭션을 호출하는 상황에서 선택할 수 있는 옵션이다.

@Transaction의 propagation 속성을 통해 피호출 트랜잭션의 입장에서는 호출한 쪽의

트랜잭션을 그대로 사용할 수도 있고, 새로운 트랜잭션을 생성할 수도 있다.

##### REQUIRED

- 디폴트 속성, 부모 트랜잭션 내에서 실행하며 부모 트랜잭션이 없을 경우 새로운 트랜잭션을 생성

##### SUPPORTS

* 이미 시작된 트랜잭션이 있으면 참여하고 그렇지 않으면 트랜잭션 없이 진행

##### REQUIRES_NEW

* 부모 트랜잭션을 무시하고 무조건 새로운 트랜잭션이 생성
* 이미 진행 중인 트랜잭션이 있으면 잠시 보류 시킨다.

##### MANDATORY

* REQUIRED와 비슷하게 이미 시작된 트랜잭션이 있으면 참여한다.
* 반면에 트랜잭션이 시작된 것이 없으면 새로 시작하는 대신 예외를 발생시킨다.
* 혼자서는 독립적으로 트랜잭션을 진행하면 안되는 경우에 사용한다.




참고 : https://goddaehee.tistory.com/167



### @Transactional 롤백은 언제 되는걸까?

### 예외 발생에도 DB반영이 된다구?



스프링은 @Transactional 애노테이션이 붙은 클래스에 프록시를 생성한다.

프록시는 트랜잭션 로직을 메서드 앞뒤에 넣어준다.



아래 코드는 스프링에게 런타임 예외가 발생하면 롤백하라고 말한다.

```java
@Transactional(rollbackFor = {RuntimeException.class})
public void saveBook(BookRequest bookRequest){
    bookRepository.save(new Book(bookRequest.getBookName()));
}
```

만약 rollbackFor 속성을 주지 않는다면

스프링 디폴트로 UnCheckedException과 Error에 대해 롤백 정책을 설정한다.

```java
@Transactional
public void saveBook(BookRequest bookRequest){
    bookRepository.save(new Book(bookRequest.getBookName()));
}
// 다음과 같이 이해한다.
@Transactional(rollbackFor={RuntimeException.class, Error.class})
public void saveBook(BookRequest bookRequest){
    bookRepository.save(new Book(bookRequest.getBookName()));
}
```

하지만 이렇게해서 예외를 던져서 catch했다고 가정하자.

```java
@Transactional
public void saveBook(BookRequest bookRequest){
    try {
	    bookRepository.save(new Book(bookRequest.getBookName()));
    	throw new IOException("Force Checked Exception");    
    } catch (Exception e){
        sout("Exception");
    }
}
```

위같은 경우는 롤백되지 않는다. 데이터베이스에 값이 들어간 상태로 끝나게 된다.

즉, 모든 예외상황에서 롤백이 되는 것은 아니다.

참고로 NullPointerException은 RuntimeException(UncheckedException)이므로 디폴트로 롤백된다.



RollbackFor는 그럼 언제쓰나?

먼저 스프링이 RuntimeExecption을 관리하는 것을 잊지 말자.

만약 checkedException이 발생했을 떄 트랜잭션이 롤백되지 않고 디비에 변경이 되는 것을 모르면 문제된다.

모든 예외에 대해서 전부 트랜잭션을 롤백하고 싶다면 아래처럼 설정한다.

```java
rollbackFor = {Exception.class}
```





참고 : https://pjh3749.tistory.com/269









