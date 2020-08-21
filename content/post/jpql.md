---
title: "JPQL - 인프런"
date: 2020-08-21T18:50:00+09:00
---
### 객체지향 쿼리 언어(JPQL)


### [1] JPQL 소개

- 가장 단순한 조회 방법

  EntityManager.find()

  객체 그래프 탐색 (a.getB().getC())

- JPQL은 객체지향 쿼리언어, 테이블 대상이 아닌 entity 객체를 대상으로 검색

- JPQL은 SQL을 추상화해서 특정 DB SQL에 의존하지 않는다.

* JPQL은 결국 SQL로 변환된다.

```Java
// 검색
String jpql = "select m From Member m where m.age > 18";
List<Member> result = em.createQuery(jpql, Member.class).getResultList();

// 실행 SQL
select
    m.id as id,
	m.age as age,
	m.USERNAME as USERNAME,
	m.TEAM_ID as TEAM_ID
from
    Member m
where
    m.age > 18
```



### [2] Criteria 소개

* 문자가 아닌 자바코드로 JPQL을 작성할 수 있다.
* JPQL 빌더 역할
* JPA의 공식기능
* 너무 복잡하고 실용성이 없어 QueryDSL 사용을 권장

```Java
// Criteria 사용 준비
CriteriaBuilder cb = em.getCriteriaBuilder();
CriteriaQuery<Member> query = cb.createQuery(Member.class);

// 루트 클래스(조회 시작 클래스)
Root<Member> m = query.from(Member.class);

// 쿼리 생성 
CriteriaQuery<Member> cq = query.select(m).where(cb.equal(m.get("username"), "kim"));
List<Member> resultList = em.createQuery(cq).getResultList();
```



### [3] QueryDSL 소개

* 문자가 아닌 자바코드로 JPQL을 작성할 수 있다.
* JPQL 빌더 역할
* 컴파일 시점에 문법 오류를 찾을 수 있다.
* 동적쿼리 작성이 편리하다.
* 단순하고 쉬워 실무사용에 권장된다.

```Java
// JPQL
// select m from Member m where m.age > 18
JPAFactoryQuery query = new JPAQueryFactory(em);
QMember m = QMemeber.member;

List<Member> list = query.selectFrom(m)
    					.where(m.age.gt(18))
    					.orderBy(m.name.desc())
    					.fetch();
```



### [4] 네이티브 SQL 소개

* JPA가 제공하는 SQL을 직접 사용가능하다.

* JPQL로 해결할 수 없는 특정 DB에 의존적이다.

  ex) 오라클 CONNECT BY, 특정 DB만 사용하는 sql 힌트





---

### [5] JPQL 소개

* JPQL은 객체지향 쿼리 언어이다.

* 테이블을 대상으로 쿼리하는 것이 아니라 엔티티 객체를 대상으로 쿼리한다.

* JPQL 문법

  select m from Member as m where m.age >  18

  엔티티와 속성은 대소문자 구분 (Member, age)

  JPQL 키워드는 대소문자 구분X (SELECT, From, where)

  엔티티 이름 사용, 테이블 이름이 아니다!

  별칭은 필수(m) - as는 생략가능

* TypeQuery, Query

  * TypeQuery : 반환 타입이 명확할 때 사용

  * Query : 반환 타입이 명확하지 않을 때 사용

    ```java
    TypedQuery<Member> query =
        em.createQuery("SELECT m FROM Member m", Member.class);
    Query query =
        em.createQuery("SELECT m.username, m.age from Member m");
    ```

* 결과 조회 API

  * query.getResultList() : 결과가 하나 이상일때 리스트반환(결과가 없으면 빈 리스트 반환)
  * query.getSIngleResult() : 결과가 정확히 하나, 단일 객체 반환
    * 결과가 없으면 : javax.persistence.NoResultException
    * 둘 이상이면 : javax.persistence.NonUniqueResultException

* 파라미터 바인딩 - 이름기준, 위치 기준

  ```JAVA
  SELECT m FROM Member m where m.username=:username
  query.setParameter("username", usernameParam);
  
  SELECT m FROM Member m where m.username=?1
  query.setParameter(1, usernameParam);
  ```

* 프로젝션

  SELECT 절에 조회할 대상을 지정하는 것

  프로젝션 대상 : 엔티티, 임베디드 타입, 스칼라 타입(숫자, 문자 등 기본데이터 타입)

  ```XML
  SELECT m FROM Member m  // 엔티티 프로젝션
  SELECT m.team FROM Member m // 엔티티 프로젝션
  SELECT m.address FROM Member m // 임베디드 타입 프로젝션
  SELECT m.username, m.age fROM Member m // 스칼라 타입 프로젝션
  ```

  SELECT m.username, m.age FROM Member m

  * Query 타입으로 조회

  * Object[] 타입으로 조회

  * new 명령어로 조회

  * 단순 값을 DTO로 바로 조회(패키지 명을 포함한 전체 클래스명 입력)

    SELECT new jpabook.jpql.UserDTO(m.username, m.age) FROM Member m

* 페이징 API

  setFirstResult(int startPosition) : 조회 시작위치(0부터 시작)

  setMaxResult(int maxResult): 조회할 데이터 수

  DB 방언에 맞추어 페이징 변환(Oracle : rownum, MySQL : LIMIT)

  ```java
  String jpql = "select m from Member m order by m.name desc";
  List<Member> resultList = em.createQuery(jpql, Member.class)
      			.setFirstResult(10)
      			.setMaxResults(20)
      			.getResultList();
  ```

  

* 조인

  * 내부 조인 : select m FROM Member m JOIN m.team t
  * 외부 조인 : select m FROM Member m LEFT JOIN m.team t
  * ON절을 활용한 조인 : select m, t from Member m LEFT JOIN m.team t on t.name = 'A'

  * 서브쿼리 : select m from Member m where m.age > (select avg(m2.age) from Member m2)

    ​			(FROM 절의 서브쿼리는 현재 JPQL에서 불가능)



* 경로 표현식

  * 상태필드(state field) : 단순히 값을 저장하기 위한 필드

  * 단일값 연관 필드 : 묵시적 내부 조인 발생, 경로 표현식 탐색 O
  * 컬렉션 값 연관필드 : 묵시적 내부 조인 발생, 경로 표현식 탐색X(값이므로)
  * 묵시적 내부조인을 안일으키는 것이 좋다. 실무에선 금지!(쿼리 튜닝이 어렵다)

  ``` XML
  select m.username // 상태필드
    from Member m
    join m.team t	  // 단일 값 연관필드
    join m.orders o // 컬렉션 값 연관필드
   where t.name = '팀A';
  
  select o.member.team from Order o; 	// 성공 - 단일 값 연관필드
  select t.members from Team t;		// 성공 - 컬렉션 값 연관필드
  select t.members.username from Team t; // 실패
  select m.username from Team t join t.member m // 성공(명시적 조인)
  ```



---

### [6] JPQL - 페치 조인(Fetch Join)

* JPQL에서 성능 최적화를 위해 제공하는 기능

* 연관된 엔티티나 컬렉션을 SQL 한번에 함께 조회하는 기능

* 페치 조인 ::= [LEFT [OUTER] | INNER ] JOIN FETCH 조인경로

* 회원을 조회하면서 연관된 팀도 함께 조회(**SQL 한번에**)

  ```XML
  <!-- JPQL -->
  select m from Member m join fetch m.team
  
  <!-- SQL -->
  SELECT M.*, T.* FROM MEMBER M
  INNER JOIN TEAM T ON M.TEAM_ID = T.ID
  ```

  ```JAVA
  String jpql = "select m from Member m join fetch m.team";
  List<Member> members = em.createQuery(jpql, Member.class).getResultList();
  
  for(Member member : members){
      // 페치 조인으로 회원과 팀을 함께 조회해서 지연로딩 X
      sout("username = " + member.getUsername() + ","
         + "teamName = " + member.getTeam().name());
  }
  ```

  

* 컬렉션 페치 조인

  일대다 관계, 컬렉션 페치 조인

  ```XML
  <!-- JPQL -->
  select t
  from Team t join fetch t.members
  where t.name = '팀A'
  
  <!-- SQL -->
  SELECT T.*, M.*
  FROM TEAM T
  INNER JOIN MEMBER M ON T.ID = M.TEAM_ID
  WHERE T.NAME = '팀A'
  ```

![image](https://user-images.githubusercontent.com/66955409/90870538-af30bc00-e3d4-11ea-8e6a-a581aa03f285.png)

* DISTINCT가 중복제거 - 같은 식별자를 가진 Team엔티티 제거

  (일반 SQL에서는 데이터가 다르므로 SQL결과에서 중복제거 실패)

```XML
select distinct t
  from Team t join fetch t.members
  where t.name = '팀A'
```

![image](https://user-images.githubusercontent.com/66955409/90871747-83aed100-e3d6-11ea-947f-36b6f5bac8bf.png)

* 패치 조인과 일반 조인의 차이

  * 일반 조인 실행 시에는 연관된 엔티티를 함께 조회하지 않음
  * 페치 조인을 사용할 때만 연관된 엔티티도 함께 조회(즉시 로딩)
  * 페치 조인은 객체 그래프를 SQL 한번에 조회하는 개념  
  
  
### [7] 페치 조인의 한계

* 페치 조인에는 별칭을 줄 수 없다.(별도 where절 사용 불가)

  * 그러면 상위 5개까지 갖고 오고싶을 때는 어떻게할까?
  * 연관관계 키로 별도로 조회해야한다.
  * JPA는 컬렉션에 해당하는 값들을 모두 가져올 것이라 인식하기 때문이다.(데이터 정합성에 문제)

* 둘 이상의 컬렉션은 페치 조인 할 수 없다.

* 컬렉션을 조인하면 페이징 API를 사용할 수 없다.

  * 다대일 같은 경우 페이징은 가능하다
  * 일대다 같은 경우에는 데이터가 뻥튀기 되므로 데이터 정합성을 맞추기 어려움)

  * 페이징 사용 시 하이버네이트는 경고 로그를 남기고 메모리에서 페이징을 하는데  
    페이징 없이 모든 데이터를 조회한 후 페이징을 하게 되므로 매우 위험하다.



### [8] 페치 조인 정리

* 모든 것을 페치 조인으로 해결할 수 없음

* 페치 조인은 객체 그래프를 유지할 때 사용하면 효과적이다.

* 여러 테이블을 조인해서 엔티티가 가진 모양이 아닌 다른 결과를 내야하면

  페치 조인보다는 일반 조인을 사용하고 필요한 데이터들만 조회하여 DTO로 반환하는 것이 효과적이다.