---
title: "JPA(1) - 인프런"
date: 2020-08-13T02:50:00+09:00
---
### JPA(1) - 인프런



#### 1. 객체를 관계형 DB에 관리, SQL에 의존적이다.

##### 객체 필드추가

```JAVA
public class Member {
	private String memberId;
	private String name;
	// tel필드 추가 시, Insert, update, select 모두추가필요
	private String tel;	
}
```
##### 객체 vs 관계형 데이터베이스

객체와 관계형 DB의 차이점  
i) 상속  
ii) 연관관계  
iii) 데이터타입  
iv) 데이터식별방법  
-> 객체를 관계형 DB에 저장하기 위해 개발자가 SQL Mapper 역할을 한다.

##### 객체지향에 SQL을 구현하기 위해서는 부가적인 코드가 필요하다
> **컬렉션처럼 처리할 수는 없을까?**


---
#### 2. JPA

* Java Persistence API, 자바 진영의 ORM 기술 표준
* ORM? Object-Relational Mapping : 객체와 관계형 DB의 매핑을 도와주는 프레임워크

![image](https://user-images.githubusercontent.com/66955409/90041895-e7653a00-dd04-11ea-9769-bd194c961136.png)

* JPA를 왜 쓰는가?
  i) 생산성
  *	저장 : jpa.persist(member)
  *	조회 : jpa.find(memberId)
  *	수정 : member.setName("changedName")
  *	삭제 : jpa.remove(member)

  ii) 패러다임 불일치 해결

  * 상속  
    Album extends Item(Inheritance : Joined)  
    저장 : jpa.persist(album)   
    조회 : jpa.find(Album.class, albumId)  
    
    ```XML
    <!-- JPA가 알아서 처리 -->
    <!-- 저장 -->
    INSERT INTO ITEM
    INSERT INTO ALBUM
    <!-- 조회 -->
    SELECT I.*, A.*
    FROM ITEM I
    JOIN ALBUM A ON I.ITEM_ID = A.ITEM_ID
    ```
    
   * 연관관계 저장
     ```JAVA
     // teamId로 관리할 필요없이 연관관계 객체로 저장한다.
     member.setTeam(team); 
     jpa.persist(member);
     ```
   *  객체 그래프 탐색
      ```JAVA
      Member member = jpa.find(Member.class, memberId);
      Team team = member.getTeam();
      ```
   * 비교
  
     ```javascript
     String memberId = "100";
     Member member1 = jpa.find(Member.class, memberId);
     Member member2 = jpa.find(Member.class, memberId);
      member1 == member2; // true
     ```
  


---
#### 3. JPA 구동 방식

![image](https://user-images.githubusercontent.com/66955409/90043709-6fe4da00-dd07-11ea-9d24-af82dee30df4.png)

* EntityManagerFactory는 하나만 생성되어 애플리케이션 전체에서 공유된다.
* EntityManager는 쓰레드 간에 공유되지 않는다.
* JPA의 모든 데이터 변경은 트랜잭션 안에서 실행된다.

  
---
#### 4. 영속성 관리

##### JPA에서 가장 중요한 2가지

1) 객체와 관계형 DB 매핑하기(Object Relational Mapping)
2) 영속성 컨텍스트(논리적인 개념으로 "**엔티티를 영구 저장하는 환경**")

EntityManager를 통해서 영속성 컨텍스트에 접근할 수 있다.

```JAVA
EntityManager.persist(entity) // DB에 저장하는 것이 아니라 영속성 컨텍스트에 저장!
```
##### Entity 생명주기
![image](https://user-images.githubusercontent.com/66955409/90044430-96efdb80-dd08-11ea-9168-72dddc528ba5.png)

1) 비영속 상태(객체를 생성한 상태)
    ```JAVA
    Member member = new Member();
    ```

2) 영속(비영속 상태의 객체를 저장한 상태)
    ```JAVA
    Member member = new Member();
    member.setId("member1");
    ```

    EntityManager em = EntityManagerFactory.createEntityManager();
    em.getTransaction().begin();
    
    // 객체를 저장한 상태(영속)
    em.persist(member);
    ```

3) 준영속(영속성 컨텍스트에서 분리)
    ```JAVA
    // 회원 엔티티를 영속성 컨텍스트에서 분리, 준영속 상태
    em.detach(member);
    // 객체를 삭제한 상태(삭제)
    em.remove(member);
    ```

##### 영속성 컨텍스트의 이점

**1) 1차 캐시**

  ```JAVA
  Member member = new Member();
  member.setId("member1");
  member.setUsername("회원1");
  //1차 캐시에 저장됨
  em.persist(member);
  //1차 캐시에서 조회(SELECT문을 타지 않는다.)
  Member findMember = em.find(Member.class, "member1");

  // 1차 캐시에 없는 경우(DB조회 -> 1차캐시에 저장 -> 반환)
  Member findMember2 = em.find(Member.class, "member2");
  ```

**2) 동일성 보장**

```JAVA
Member a = em.find(Member.class, "member1");
Member b = em.find(Member.class, "member1");
System.out.println(a == b); //동일성 비교 true (1차 캐시)
```

**3) 트랜잭션을 지원하는 쓰기지연**

```java
EntityManager em = emf.createEntityManager();
EntityTransaction transaction = em.getTransaction();
//엔티티 매니저는 데이터 변경시 트랜잭션을 시작해야 한다.
transaction.begin(); // [트랜잭션] 시작
em.persist(memberA);
em.persist(memberB);
//여기까지 INSERT SQL을 데이터베이스에 보내지 않는다.(쓰기지연 SQL저장소에 저장)
//커밋하는 순간 데이터베이스에 INSERT SQL을 보낸다.
transaction.commit(); // [트랜잭션] 커밋
```

![image](https://user-images.githubusercontent.com/66955409/90045529-21850a80-dd0a-11ea-8ab5-8bf3e958da07.png)

**4) 변경 감지**(Dirty Checking)

```java
// 영속 엔티티 조회
Member memberA = em.find(Member.class, "memberA");
// 영속 엔티티 데이터 수정
memberA.setUsername("hi");
memberA.setAge(10);
//em.update(member) 이런 코드가 필요없다 -> 마치 Collection처럼 set으로만 해결한다.
//기존 id에 대한 스냅샷과 비교하여 변경감지
```



##### 플러시란?  
영속성 컨텍스트의 변경내용을 DB에 반영!

> 플러시가 발생되는 경우
> * em.flush()  - 직접 호출
> * 트랜잭션 커밋 - 플러시 자동 호출
> * JPQL 쿼리 실행 - 플러시 자동 호출
---
#### 5. 연관관계

**객체의 참조 vs 테이블의 외래키 매핑**  
예시로 Member 객체에서 Team을 알기위해서 teamId 속성을 갖고 있다면   
teamId 속성으로 Team 객체를 다시 조회해야 하므로 객체지향적인 방법이 아니다.(데이터 중심 모델링)  
따라서, 객체는 참조를 통해 연관객체를 찾는다.

![image](https://user-images.githubusercontent.com/66955409/90047858-93128800-dd0d-11ea-87b4-fb1bd4b8a162.png)

```JAVA
@Entity
public class Member {
	@Id @GeneratedValue
 	private Long id;
 
    @Column(name = "USERNAME")
 	private String name;
 	private int age;
	
    // @Column(name = "TEAM_ID")
	// private Long teamId;
 	@ManyToOne
 	@JoinColumn(name = "TEAM_ID")
 	private Team team;
}

@OneToMany

//조회
Member findMember = em.find(Member.class, member.getId());
//참조를 사용해서 연관관계 조회
Team findTeam = findMember.getTeam();

// 새로운 팀B
Team teamB = new Team();
teamB.setName("TeamB");
em.persist(teamB);
// 회원1에 새로운 팀B 설정
member.setTeam(teamB);
```



##### 양방향 연관관계

![image](https://user-images.githubusercontent.com/66955409/90048188-2055dc80-dd0e-11ea-8578-89a94f2cd93d.png)

테이블 연관관계는 TEAM_ID만으로 이미 양방향 성립  
객채에서의 양방향 연관관계는 회원->팀, 팀->회원 연관관계가 **두개**이다!(사실 양방향이 아니다.)



##### 연관관계의 주인과 mappedBy

JPA에서는 두 테이블 중 하나로 외래 키를 관리해야 한다.(양쪽 다 수정할 수 없다.)

> 양방향 매핑 규칙
> * 객체의 두 관계 중 하나를 연관관계의 주인으로 지정
> * 연관관계의 주인만이 외래키를 관리(등록, 수정)
> * 주인이 아닌쪽은 읽기만 가능
> * 주인이 아니면 mappedBy 속성으로 주인 지정

