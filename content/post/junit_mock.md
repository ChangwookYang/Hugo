---
title: "TDD, JUnit, TestDouble"
date: 2020-09-16T23:00:00+09:00
---

---

### TDD란? (Test-Driven Development)

테스트를 중점으로 두고, 테스트를 선 작성하여 통과하는 코드를 후에 개발하는 Agile 개발 방법론

![img](https://wikidocs.net/images/page/224/tdd_flow.jpg)

**given -> when -> then**

TDD의 기법 중 JUnit,  테스트 더블(Mock, Stub)에 대해 알아보자

---



### JUnit이란?

JUnit은 자바용 단위 테스트 작성을 위한 산업 표준 프레임워크다.

프로그램을 작성하여 번거롭게 디버깅할 필요가 없어진다.

JDK 1.4에서 추가된 assertXXX를 사용하여 Test를 진행하며

테스트 결과를 확인하는 것 이외에 최적화된 코드를 유추해내는 기능도 제공한다.

테스트 결과를 단순 텍스트로 남기는게 아니라 Test 클래스로 남기게되어 History 관리에도 용이하다.

---



### JUnit 사용하기


##### Calculator 클래스 구현

```java
package com.calculator;

public class Calculator {
    public int sum(int num1, int num2){
        return num1 + num2;
    }
}
```

##### Calculator 테스트 클래스 구현

```java
// 1. com.calculator.test 패키지를 만든다.
// 2. com.calculator.test 패키지에서 마우스 오른쪽버튼 -> New -> JUnit Test Case 선택
// 3. Name : class명 + Test / Undertest : com.calculator.Calculator

package com.calculator.test;
public class CalculatorTest {
    @Test
    public void testSum() {
        Calculator calculator = new Calculator();
        asserEquals(30, calcultaor.sum(10,20));
    }
}

// 실행방법 : 테스트클래스 마우스 오른쪽버튼 -> RunAs -> JUnit Test
```


---
---

### 테스트 더블
  
![img](https://miro.medium.com/max/381/1*8I31UX1fKaPZAbkSF6DG0w.gif)

단위 테스트는 독립적으로 작성될 수 있어야한다.

테스트 대상이 의존하는 것으 실제가 아닌 다른 것으로 대체 할 수 있다.

이를 테스트 더블(Test Double)이라 하고 Mock/Stub/Fack 등이 있으며

Test Double의 어원은 Stunt Double(대역)이다.


##### 사용시점

개발하다보면 테스트 대상 코드가 다른 클래스에 종속되어 있는 경우가 있다.

Database, 파일 시스템, 외부 리소스 등등 런타임 환경에 의존적인 어플리케이션을 단위테스트하기는 어렵다.

또는 팀 단위 프로젝트에서 다른 부분이 준비 안되었을때 가짜를 만들어 시뮬레이션 할 수 있다.



---

### Mock 객체란?

가짜 객체를 제공하는 테스트 방식으로 목(Mock) 객체를 사용할 수 있다.

목 오브젝트는 단지 모조품이므로 처음부터 실제 객체로 테스트 가능하다면 목(Mock) 오브젝트를 쓰지말자!

```java

public class MockCategoryServiceImpl implements CategoryService {
    
    @Override
    public List<Category> getCategoriesByBookNo(int no) {
        return new ArrayList<Category>() {
            {
            	add(new Category(1, "유아동"));
            	add(new Category(2, "레저/취미"));
            	add(new Category(3, "패션"));
            }
        };
    }
    
}
```

---


### Stub 함수란?


가짜 함수(속이 빈 함수)를 제공하는 테스트 방식으로 Stub 함수를 사용할 수 있다.

함수가 반환하는 값을 하드코딩으로 임의 생성하는 형식이다.

복잡한 논리 흐름을 가지는 경우 테스트를 단순화할 목적으로 사용한다.

```java
@Test
public void calculate() {
    // given
    given(userService.getUserByNo(4)).willReturn(Optional.empty());	// TEST STUB
    
    // when
    int result = calculator.calculate(4, 1);
    
    // then
    assertThat(result).isEqualTo(0);
}

public class UserServiceImple implements UserService {
    @Overrider
    public Optional<User> getUserByNo(int userNo){
        if(useNo == 1) {
         	User user = new User("changwook", "seoul");
            return Optional.of(user);
        } else if(userNo == 4) {
         	User user = new User("test", "incheon");
            return Optional.of(user);    
        }
        return Optional.empty();
    }
    
}
```

