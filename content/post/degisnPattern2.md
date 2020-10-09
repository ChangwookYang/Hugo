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



---

### # Chapter 6 - Prototype

* **복사해서 인스턴스 만들기**

* new 라는 Java 언어 키워드를 사용해서 클래스 이름을 지정해서 인스턴스 생성

  그러나 클래스 이름을 지정하지 않고 인스턴스를 생성하는 경우도 있다.

  **1) 종류가 너무 많아 클래스로 정리되지 않는 경우**

  ​	오브젝트 종류가 너무 많아 별도 클래스를 만들어 다수 소스파일을 만들어야 하는 경우

  **2) 클래스로부터 인스턴스 생성이 어려운 경우** (인스턴스가 복잡한 작업을 거쳐 만들어지는 경우)

  ​	만들어진 인스턴스를 일단 저장해두고 만들고 싶은 경우에 그것들을 복사

  **3) framework와 생성할 인스턴스를 분리**

  ​	인스턴스를 생성할 때의 framework를 특정 클래스에 의존하지 않도록 만들고 싶은 경우

  

* 프로토타입

  클래스로부터 생성하지 않고 인스턴스로부터 별도 인스턴스를 만드는 **Prototype패턴**

  원형이 되는, 모범이 되는 인스턴스를 기초로 새로운 인스턴스를 만든다.

<img alt="1111" src="https://user-images.githubusercontent.com/66955409/95545158-0d266b00-0a38-11eb-964b-f2edb48ec0e4.PNG">

* Product 인터페이스

  **java.lang.Cloneable 인터페이스를 상속**하여 복제가능하다.

  Cloneable 인터페이스를 구현하고 있는 클래스의 인스턴스는

  clone메소드를 사용해서 자동적으로 복제할 수 있다.

  ```java
  package framework;
  public interface Product extends Cloneable {
      public abstract void use(String s);
      public abstract Product createClone();
  }
  ```

* Manager 클래스

  Product 인터페이스를 이용해서 **인스턴스의 복제를 실행**

  Manager 클래스에서는 구체적인 개개의 클래스 이름을 쓰지 않고

  단지 Product 인터페이스 이름만 사용하여 밀접도를 줄인다.

  ```java
  package framework;
  
  public class Manager {
      private HashMap showcase = new HashMap();
      public void register(String name, Product proto){
          showcase.put(name, proto);
      }
      public Product create(String protoname){
          Product p = (Product) showcase.get(protoname);
          return p.createClone();
      }
  }
  ```

* MessageBox 클래스

  ```java
  import framework.*;
  
  public class MessageBox implements Product {
  
  	private char decochar;
    	public MessageBox(char decochar){
  		this.decochar = decochar;	
  	}
  	public void use(String s){
          ...
      }
      public Product createClone(){
          Product p = null;
          try {
              p = (Product) clone();
              // clone() : 자기 자신을 복제하는 메소드
              // 복제를 생성할 때 인스턴스가 가지고 잇는 필드값도 그대로 복사.
          } catch (CloneNotSupportedException e){
              e.printStackTrace();
          }
          return p;
      }
  }
  ```

* Main 클래스

  ```java
  // 준비
  Manager manager = new Manager();
  MessageBox mbox = new MessageBox('*');
  MessageBox sbox = new MessageBox('/');
  manager.register("warning box", mbox);
  manager.register("slash box", sbox);
  // 생성
  Product p1 = manager.create("warning box");
  p1.use("Hello World");
  Product p2 = manager.create("slash box");
  p2.use("Hello world");
  ```
  
  
  
  

---

### # Chapter 7 - Builder

* **복잡한 인스턴스 조립하기**

* 일반적으로 복잡한 구조물을 세울 때 한번에 완성시키기는 어렵다

  전체를 구성하는 각 부분을 만들고 단계를 밟아 만들어간다.

* 예제) '문서'를 작성하는 프로그램

  > 문서형식
  >
  > 1) 타이틀을 포함
  >
  > 2) 문자열을 포함
>
  > 3) 개별 항목을 포함

  **Builder 클래스**는 문서를 구성하기 위한 메소드를 결정한다.

  **Director 클래스**는 그 메소드를 사용해서 구체적인 하나의 문서를 만든다.
  
  Builder 클래스는 추상메소드만 선언되어 있고 구체적인 구현은 하위 클래스가 한다.
  
  

![image](https://user-images.githubusercontent.com/66955409/95548611-6abeb580-0a40-11eb-8905-cccc4e3e48af.png)

* Builder 클래스

  Builder 클래스는 '문서'를 만들 메소드들을 선언하고 있는 추상클래스

  ```java
  public abstract class Builder {
      public abstract void makeTitle(String title);
      public abstract void makeString(String str);
      public abstract void makeItems(String[] items);
      public abstract void close();
  }
  ```

* Director 클래스

  Director 클래스에서는 Builder 클래스로 선언되어 있는 메소드를 사용해서 문서를 만든다.

  Director 클래스의 생성자 인수는 Builder 형이나 실제로 Builder 클래스는 추상클래스여서

  인스턴스로 주어지는 경우는 없고 Builder클래스의 하위 클래스의 인스턴스가 주어진다.

  ```java
  public class Director {
      private Builder builder;
      public Director(Builder builder) {
          // Builder의 하위 클래스의 인스턴스가 주어지므로
          // builder 필드에 저장해둔다.
          this.builder = builder;
      }
      
      // contruct 메소드는 문서를 만드는 메소드
      public void construct() {
          builder.makeTitle("Greeting");
          builder.makeString("아침과 낮에");
          builder.makeItems(new String[]{
             "좋은 아침입니다.",
             "안녕하세요.",
          });
          builder.makeString("밤에");
          builder.makeItems(new String[]{
             "안녕하세요",
             "안녕히주무세요",
             "안녕히 계세요"
          });
          builder.close();
      }
  }
  ```

* TextBuilder 클래스

  TextBuilder 클래스는 Builder 클래스의 하위 클래스입니다.

  일반 텍스트를 사용해서 문서를 구축하고 결과는 String으로 반환합니다.

  ```java
  public class TextBuilder extends Builder {
      private StringBuffer buffer = new StringBuffer();
      public void makeTitle(String title){
          buffer.append("==========================\n");
          buffer.append("*" + title + "*\n");
          buffer.append("\n");
      }
      // makeString makeItems 생략...
         
      public String getResult() {
          return buffer.toString();
      }
  }
  ```

* Main 클래스

  ```java
  TextBuilder textbuilder = new TextBuilder();
  Director director = new Director(textbuilder);
  director.construct();
  String result = textbuilder.getResult();
  sysout(result);
  ```

  