---
title: "Design Pattern #2"
date: 2020-10-07T21:45:00+09:00
---

### # Chapter 5 - Singleton 패턴

- 클래스의 인스턴스가 단 하나만 필요할 경우

  ex) 컴퓨터 자체를 표현한 클래스, 윈도우 시스템 등

- 프로그래머가 주의를 기울여 인스턴스를 1개만 생성되는 것이 아니라

  지정된 클래스의 인스턴스가 **절대로** 1개 밖에 존재하지 않는 것을 보증하고 싶을 때

  인스턴스가 1개만 존재하는 것을 표현하고 싶을 때 Singleton 패턴을 사용한다.

  ```java
  // Singleton 클래스
  public class Singleton {
      private static Singleton singleton = new Singleton();
      private Singleton() {
          sysout("인스턴스 생성");
      }
      public static Singleton getInstance() {
          return singleton;
      }
  }
  
  // Main 클래스
  public static main(String[] args){
      Singleton obj1 = Singleton.getInstance();
      Singleton obj2 = Singleton.getInstance();
      // obj1 == obj2 : true
  }
  ```

* Singleton의 역할

  Singleton 의 역할은 유일한 인스턴스를 얻기위한 static 메소드를 갖는다.

  이 메소드는 언제나 동일한 인스턴스를 반환한다.

  복수의 인스턴스가 존재하면 인스턴스들이 서로 영향을 미치고 뜻하지 않은 버그가 발생할 수 있다.

* 유일한 인스턴스는 언제 생성되는가?

  프로그램 실행 개시 후 최초로 getInstance() 메소드를 호출했을 때 Singleton 클래스는 초기화된다.

  static 필드의 초기화가 이루어지고 유일한 인스턴스가 생성된다.



### # Chapter 6 - Prototype

* 복사해서 인스턴스 만들기

* new 라는 Java 언어 키워드를 사용해서 클래스 이름을 지정해서 인스턴스 생성

  그러나 클래스 이름을 지정하지 않고 인스턴스를 생성하는 경우도 있다.

  1) 종류가 너무 많아 클래스로 정리되지 않는 경우

  ​	오브젝트 종류가 너무 많아 별도 클래스를 만들어 다수 소스파일을 만들어야 하는 경우

  2) 클래스로부터 인스턴스 생성이 어려운 경우 (인스턴스가 복잡한 작업을 거쳐 만들어지는 경우)

  ​	만들어진 인스턴스를 일단 저장해두고 만들고 싶은 경우에 그것들을 복사

  3) framework와 생성할 인스턴스를 분리

  ​	인스턴스를 생성할 때의 framework를 특정 클래스에 의존하지 않도록 만들고 싶은 경우

* 프로토타입

  클래스로부터 인스턴스를 생성하는 것이 아닌 인스턴스로부터 별도의 인스턴스를 만드는 **Prototype패턴**

  원형이 되는, 모법이 되는 인스턴스를 기초로 새로운 인스턴스를 만든다.



-- 추가 업데이트 예정..