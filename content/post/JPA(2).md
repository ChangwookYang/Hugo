---
title: "JPA(2) - 인프런"
date: 2020-08-20T02:00:00+09:00
---
### JPA(2)-인프런

### [1] 상속관계 매핑(1, 조인전략, 2. 단일테이블, 3. 구현클래스)

##### 상속관계 매핑 : 객체의 상속과 구조와 DB의 슈퍼타입 서브타입 관계를 매핑
슈퍼타입 서브타입 논리 모델을 실제 물리 모델로 구현하는 방법

![image](https://user-images.githubusercontent.com/66955409/90657227-8cd85a80-e27d-11ea-9bf6-9c3f24d9e5f6.png)

##### 1) 조인 전략 - 각각 테이블로 변환
![image](https://user-images.githubusercontent.com/66955409/90657713-b0030a00-e27d-11ea-861a-52365519f312.png)

```java
@Entity
@Inheritance(strategy=InheritanceType.JOINED)
@Discriminator //DTYPE컬럼을 만들고 자식클래스 명이 들어간다.(ALBUM, MOVIE ..)
public class Item {
	...
}
@Entity
public class Album extends Item { ... }
```
> 장점
> 1) 테이블 정규화
> 2) 외래키 참조 무결성 제약조건 활용 가능(주문 테이블에서 ITEM 테이블만 보면 된다.)
> 3) 저장공간 효율화
>
> 단점
> 1) 조회 시 조인을 많이 사용하여 성능 저하
> 2) 데이터 저장 시 INSERT SQL 2번 호출

##### 2) 단일 테이블 전략 - 통합 테이블로 변환(한 테이블에 다 들어간다)

![image](https://user-images.githubusercontent.com/66955409/90658690-d5dcde80-e27e-11ea-964f-c62f9208bf0d.png)

```java
@Inheritance(strategy= InheritanceType.SINGLE_TABLE)
// 어느 자식클래스인지 구분하기 위해 DTYPE은 필수로 생성된다.
```
> 장점
> 1) 조회 쿼리가 단순하고 조인이 없어 성능이 좋다
>
> 단점
> 1) 자식 엔티티가 매핑한 컬럼은 모두 null 허용
> 2) 단일 테이블에 저장하므로 테이블이 커질 수 있다.


##### 3) 구현 클래스마다 테이블 전략 - 서브타입 테이블로 변환

![image](https://user-images.githubusercontent.com/66955409/90659512-b85c4480-e27f-11ea-9bf7-c7a618253bab.png)

```java
@Inheritance(strategy = InheritanceType.TABLE_PER_CLASS)
// Item Class는 Abstract 추상 테이블로 만들어준다.
// 클래스별로 분리되므로 @Disciminator는 무의미하다.
```

> 쓰지 말아야할 전략!!!  
> 테이블을 묶어낼 수가 없다. 조회하려면 모두 UNION ALL해야한다.  
> 컬럼 추가 시에도 모두 추가해줘야한다.  



---



### [2] @MappedSuperclass

공통 매핑정보가 필요할때 사용(id, name, regDt, regUsrId, updDt, updUsrId)

DB는 완전 다른데 속성값을 공통으로 사용한다.(추상 클래스 사용)

![image](https://user-images.githubusercontent.com/66955409/90660113-754ea100-e280-11ea-80a5-c91479497879.png)

```java
@MappedSuperClass
class abstract BaseEntity{
	LocalDateTime regDt;
	String regUsrId;
}
```

---



### [3] 프록시와 연관관계 관리


##### 1) 프록시 기초
- em.find vs em.getReference()
- em.find() : DB를 통해 실제 엔티티 객체 조회
- em.getReference() : DB 조회를 미루는 가짜(프록시) 엔티티 객체 조회

![image](https://user-images.githubusercontent.com/66955409/90661341-0a05ce80-e282-11ea-88b9-2dfc242505de.png)

> 껍데기에 아이디 값만 갖고 있다.



##### 2) 프록시 특징

* 실제 클래스를 상속
* 실제 클래스와 겉모양이 같다
* 사용 입장에서는 진짜 객체인지 프록시 객체인지 구분하지 않고 사용한다.

![image](https://user-images.githubusercontent.com/66955409/90661575-54874b00-e282-11ea-8505-edc2d4b42448.png)

* 프록시 객체는 실제 객체의 참조(target)를 보관
* 프록시 객체를 호출하면 프록시 객체는 실제 객체의 메소드를 호출한다.

  ![image](https://user-images.githubusercontent.com/66955409/90661790-8d272480-e282-11ea-9104-8ac861359ffc.png)

##### 3) 프록시 객체의 초기화

```Java
Member member = em.getReference(Member.class, "id1");
member.getName();
```
![image](https://user-images.githubusercontent.com/66955409/90669307-6968dc00-e28c-11ea-9c8a-42e33a3979e7.png)

(2. 초기화요청)은 Member target에 값이 없을 때 초기화를 요청한다.



##### 4) 프록시 특징

* 프록시 객체는 처음 사용할 때 한번만 초기화한다.

* 프록시 객체 초기화 시에 프록시 객체가 실제 엔티티로 바뀌는 것이 아니라

  초기화 되면 프록시 객체를 통해서 실제 엔티티에 접근이 가능하다.

  (프록시 객체를 호출하면 프록시 객체는 실제 객체의 메소드를 호출한다.)

* 프록시 객체는 원본 엔티티를 상속받는다. 비교 시에는 == 대신에 instance of를 사용한다.

* **영속성 컨텍스트에 찾는 엔티티가 있으면 em.getReference()를 호출해도 실제 엔티티를 반환한다.**

  (JPA에서는 같은 영속성에서 같은 id를 조회하면 항상 같은 결과를 반환한다.)

* 영속성 컨텍스트의 도움을 받을 수 없는 준영속 상태일 때 프록시를 초기화하면 문제 발생!

  (프록시는 무조건 영속성 컨텍스트를 통하므로 준영속 상태에서 사용할 수 없다.)

    

##### 5) 프록시 확인

* 프록시 인스턴스의 초기화 여부 확인

  PersistenceUtil.isLoaded(Object entity);

* 프록시 클래스 확인 방법

  entity.getClass().getName() 출력 : javasist .. or HibernateProxy ...

* 프록시 강제 초기화

  하이버네이트 : org.hibernate.Hibernate.initialize(entity);

  JPA표준 : 강제초기화 없으므로 강제호출로 초기화 member.getName();



---



### [4] 즉시 로딩과 지연 로딩

##### 1) Member를 조회할 때 Team도 함께 조회해야 할까?  
단순히 member정보만 사용할 경우 Team까지 조회하면 낭비가 된다.  
그러므로 지연로딩 LAZY를 사용해서 프록시로 조회할 수 있다.(즉시 로딩은 EAGER)

```java
@Entity
public class Member {
    @Id
    @GeneratedValue
    private Long id;
    
    @Column(name = "USERNAME")
    private String name;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "TEAM_ID")
    private Team team;
}
```

![image](https://user-images.githubusercontent.com/66955409/90664028-69b1a900-e285-11ea-8488-cbf1e8a15e98.png)

```JAVA
Team team = member.getTeam();
team.getName(); // 실제 team을 사용하는 시점에 초기화(DB조회)
```



##### 2) 프록시와 즉시로딩 주의

* 가급적 지연로딩만 사용하는게 좋다
* 즉시 로딩을 적용하면 예상하지 못한 SQL이 발생한다.
* 즉시 로딩은 JPQL에서 N+1문제를 일으킨다.(하나 실행했는데 딸린 컬렉션들이 즉시로딩되어버림)
* @ManyToOne, @OneToOne (N:1) 인 애들이 디폴트가 즉시로딩이다. -> LAZY로 설정
* @OneToMany, @ManyToMany는 기본이 지연로딩이다.

---



### [5] 영속성 전이 CASCADE

특정 엔티티를 영속 상태로 만들 때 연관된 엔티티도 함꼐 영속 상태로 만들고 싶을 때 사용한다.

예) 부모 엔티티를 저장할 때 자식 엔티티도 함께 저장(연관 아이들을 함께 persist)

```java
@OneToMany(mappedBy="parent", cascade=CascadeType.PERSIST)
```
![image](https://user-images.githubusercontent.com/66955409/90669608-de3c1600-e28c-11ea-8012-7abba9eb1191.png)


영속성 전이는 연관관계를 매핑하는 것과 아무 관련이 없다.

엔티티를 영속화 할 때 연관된 엔티티도 함께 영속화하는 편리함을 제공할 뿐!

---



### [6] 고아 객체

고아객체 제거 : 부모 엔티티와 연관관계가 끊어진 자식 엔티티를 자동으로 삭제한다.

> orphanRemoval = true;

부모를 제거하면 자식은 고아가 된다.

고아 객체 제거기능을 활성화하면, 부모를 제거할 때 자식도 함께 제거된다.

CascadeType.REMOVE처럼 동작한다.

---



### [7] 영속성 전이 + 고아객체, 생명주기

CascadeType.ALL + orpahnRemoval = true

스스로 생명주기를 관리하는 엔티티는 em.persist()로 영속화, em.remove로 제거

두 옵션을 모두 활성화하면 부모 엔티티를 통해서 자식의 생명 주기를 관리할 수 있다.

---



### [8] 값 타입

##### 1) JPA의 데이터 타입 분류

* 엔티티 타입

  * @Entity로 정의하는 객체

  * 데이터가 변해도 식별자로 지속해서 추적 가능

  * 예) 회원 엔티티의 키나 나이값을 변경해도 식별자로 인식가능

* 값 타입

  * int, Integer, String처럼 단순 값으로 사용하는 자바 기본타입이나 객체

  * 변경 시 추적불가

  * 예) 숫자 100을 200으로 변경하면 완전히 다른 값으로 대체

  * 값타입 종류

    * 기본값 타입 : int, double, Integer, String

    * 임베디드 타입 : x, y좌표처럼 복합 값 타입

    * 컬렉션 값 타입

      

##### 2) 기본값 타입

* 생명 주기를 엔티티에 의존 : 회원을 삭제하면 이름, 나이 필드도 함께 삭제

* 기본 타입은 항상 값을 복사한다.

  

##### 3) 임베디드 타입(복합 값 타입)

* 주로 기본값 타입을 모아서 만들었다.
* @Embeddable : 값 타입을 정의하는 곳에 표시
* @Embedded : 값 타입을 사용하는 곳에 표시

![image](https://user-images.githubusercontent.com/66955409/90669748-12afd200-e28d-11ea-8fff-ab9a34cb00b9.png)

* 임베디드 타입은 엔티티의 값일 뿐이므로, 임베디드 타입 사용 전후의 매핑 테이블은 동일하다.

---



### [9] 값 타입과 불변 객체
  
##### 1. 값 타입 공유 참조
* 임베디드 타입 같은 값 타입을 여러 엔티티에서 공유하면 위험하다.  
* 아래 그림은 회원 1의 city만 OldCity에서 NewCity로 바꾸고 싶었는데 회원 2의 city도 같이 변경된다.  
(주소값이 같으므로)

![image](https://user-images.githubusercontent.com/66955409/90669942-65898980-e28d-11ea-966b-2e1c8ff87da7.png)


따라서 값타입의 실제 인스턴스 값을 복사해서 사용하여야한다.


```java
setCity(new Address("NewCity"))
```
![image](https://user-images.githubusercontent.com/66955409/90669978-7803c300-e28d-11ea-8e14-95b640576530.png)


> 객체 타입의 한계
>
> * 임베디드 타입처럼 직접 정의한 값 타입은 객체 타입이다.
> * 기본 타입에 값을 대입하면 값을 복사하지만 객체 타입은 참조 값을 직접 대입하게 된다.
> * 객체의 공유 참조는 피할 수 없다.
> ```javascript
> Address a = new Address(“Old”);
> Address b = a; //객체 타입은 참조를 전달
> b. setCity(“New”)
> ```



##### 2) 불변 객체

* 객체를 수정할 수 없게 만들면 부작용을 원천 차단

* 값 타입은 불변 객체(immutable object)로 설계해야한다.
* 불변 객체 : 생성 시점 이후 절대 값을 변경할 수 없는 객체
* 생성자로만 값을 설정하고 수정자(Setter)를 만들지 않으면 된다.
  
* Integer, String은 자바가 제공하는 대표적인 불변 객체이다.
* 만약 변경하고 싶다면 new Address("") 로 값을 선언한 후에 member.setAddress에 인자값을 넘겨준다.



##### 3) 값 타입의 비교

* 동일성 (identity) 비교 : 인스턴스의 참조값을 비교, == 사용

* 동등성 (equivalence) 비교 : 인스턴스의 값을 비교, equals() 사용

---



### [10] 값 타입 컬렉션
![image](https://user-images.githubusercontent.com/66955409/90670114-ac777f00-e28d-11ea-987a-b00eb15c21f8.png)

##### 1) 값 타입 컬렉션

* 값 타입을 하나 이상 저장할 떄 사용

* DB는 컬렉션을 같은 Table에 저장할 수 없다.

* 컬렉션을 저장하기 위한 별도의 Table이 필요하다.

  

* 값타입의 생명주기는 별도로 없다.

* 값타입 컬렉션은 영속성 전이(CASCADE) + 고아객체 제거 기능을 필수로 가진다.

* 값타입 컬렉션들은 지연로딩 방식이다.

```java
@ElementCollection
@CollectionTable(name = "FAVORITE_FOOD"
                ,joinColumns=@JoinColumn(name="MEMBER_ID"))
private Set<String> favoriteFoods = new HashSet<>();

// 수정
findMember.getFavoriteFoods().remove("치킨");
findMember.getFavoriteFoods().add("한식");
findMember.getAddressHistory().remove(new Address("old", "street", "10000"));
findMember.getAddressHistory().add(new Address("new", "street", "10000"));
// 값 타입 컬렉션에 변경사항이 발생하면 주인 엔티티와 연관된 모든 데이터를 삭제하고
// 값 타입 컬렉션에 있는 현재값을 모두 다시 저장한다.
// 따라서 값 타입 컬렉션 대신에 일대다 관계를 고려하는 것이 바람직하다.
// 일대다 관계에서 영속성전이(cascade) + 고아객체 제거를 사용해서 값 타입 컬렉션처럼 사용가능하다.
```



##### 2) 엔티티 타입의 특징

* 식별자 O
* 생명주기 관리
* 공유

##### 3) 값 타입의 특징

* 식별자 X
* 생명 주기를 엔티티에 의존
* 공유하지 않는 것이 안전(복사해서 사용)
* 불변 객체로 만드는 것이 안전



> 값타입은 정말 값타입이라 판단될떄만 사용한다!
>
> 식별자가 필요하고 지속해서 값을 추적, 변경해야한다면 그것은 값타입이 아닌 엔티티이다.