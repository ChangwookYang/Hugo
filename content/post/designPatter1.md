---
title: "Design Pattern #1"
date: 2020-10-06T01:45:00+09:00
---


### # 디자인 패턴

백설 공주를 연기하는 연기자가 누구이든 왕자와 백설공주는 사랑을 한다.

디자인 패턴은 '역할'에 집중한다.

디자인 패턴은 구성원이 다르더라도 부품이 어떻게 조립되어있고,

각각의 부품이 어떻게, 관련해서 큰 기능을 발휘하는지를 표현한 것이다.


---
### # Chapter 1 - Iterator 패턴

Iterator 순서대로 지정해서 처리하기

Iterator 패턴이란, 무엇인가 많이 모여있는 것들을

순서대로 지정하면서 전체를 검색하는 처리를 실행하기 위한 것


---
### # Chapter 2 - Adapter 패턴

이미 제공되어 있는 것을 그대로 사용할 수 없을 때 필요한 형태로 교환하고 사용하는 일

'이미 제공되어 있는 것'과 '필요한 것' 사이의 '차이'를 없애주는 패턴이 Adapter 패턴이다.  
(Wrapper패턴이라고도 한다)



##### Adapter 패턴의 두 종류

1. 클래스에 의한 Adapter(상속)
2. 인스턴스에 의한 Adapter(위임) : 어떤 메소드의 실제처리를 다른 인스턴스의 메소드에 맡긴다.

##### Adapter 패턴의 사용 경우

- 부품으로 재이용할 수 있다. 기존의 클래스를 개조하여 필요한 클래스를 만든다.
- 기존 클래스는 버그가 없으므로 Adapter 역할의 클래스를 중점적으로 검사한다.
- 구버전과 신버전을 공존시키고 유지보수를 편하게 하기위해 사용된다.(호환성)

##### 예제 - Banner 클래스를 사용하여 Print 충족

1. 클래스에 의한 Adapter(상속)
```java
public class PrintBanner extends Banner implements Print {
	public PrintBanner(String string){
        super(string);
    }
    public void printWeak() {
        showWithParen();
    }
    public void printString() {
        showWithAster();
    }
}
```
2. 인스턴스에 의한 Adapter(위임)

```java
public class PrintBanner extends Print {
    private Banner banner;
    public PrintBanner(String string){
        this.banner = new Banner(string);
    }
    public void printWeak() {
        banner.showWithParen();
    }
    public void printString() {
        banner.showWithAster();
    }
}
```


---
### # Chapter 3 - Template Method 패턴

**하위클래스에서 구체적으로 처리하기**

상위 클래스 쪽에 템플릿에 해당하는 메소드가 정리되어 있고, 그 메소드에는 추상 메소드가 사용되고 있다.

하위 메소드에서 실제 로직을 구현한다.

```java
public abstract class AbstractDisplay  {
    public abstract void open();
    public abstract void print();
    public abstract void close();
    public final void display() {
        // 추상 클래스에서 구현되고 있는 메소드 display
        open();
        print();
        close();
    }
}

// 메인에서 호출
AbstractDisplay d1 = new StringDisplay("하이하이ㅎㅎ");
d1.display();	// 템플릿 메소드 호출
```



##### Template Method 패턴의 이점

상위 클래스에서 템플릿 메소드의 알고리즘이 기술되어 하위 클래스에서 다시 기술할  필요가 없다.

Template Method를 구현하지 않고 여러개의 Concrete Class를 만들면 에러발생 시 모두 수정해야한다.



##### Template Method 패턴의 단점

상위 클래스에서 기술을 많이하게 되면 하이클래스에서 기술이 편하지만 자유성이 많이 줄게된다.

상위 클래스에서 기술을 적게하면 하위 클래스의 기술이 어렵게 되고

하위클래스의 기술이 중복될 수 있다.


---
### # Chapter 4 - Factory Method 패턴

인스턴스를 생성하는 공장을 Template Method 패턴으로 구성


##### framework
| Factory         | --------------------> creates | Product |
| --------------- | ----------------------------- | ------- |
| create          |                               | use     |
| createProduct   |                               |         |
| registerProduct |                               |         |

##### idcard

| IdCardFactory (Factory 상속) | --------------------> creates | IDCard (Product 상속) |
| ---------------------------- | ----------------------------- | --------------------- |
| owners (필드)                |                               | owner (필드)          |
| createProduct                |                               | use                   |
| registerProduct              |                               | getOwner              |
| getOwners                    |                               |                       |

framework - Factory 클래스는 Template Method 패턴이 사용되고 있다.

create 메소드에서 createProduct 후 registerProduct 순서로 구현되고 있다.

```java
public abstract class Factory {
    public final Product create(String owner){
        Product p = createProduct(owner);
        registerProduce(p);
        return p;
    }
    protected abstract Product createProduct(String owner);
    protected abstract void registerProduct(Product product);
}

public class IDCardFactory extends Factory {
    private List owners = new ArrayList();
    protected Product createProduct(String owner){
        return new IDCard(owner);
    }
    protected void registerProduct(Product product){
        owners.add(((IDCard)product).getOwner());
    }
}

public class Main {
    public static void main(String[] args){
        Factory factory = new IDCardFactory();
        Product card1 = factory.create("홍길동");
        Product card2 = factory.create("욱짱양");
    	card1.use();
        card2.use();
    }
}
```

##### Factory Method 부연설명

framework를 통해 전혀 다른 제품, 공장을 만들 수 있다.

framework의 내용을 수정할 필요는 없다.



factory는 공장을 의미한다.

인스턴스를 만드는 공장을 Template Method 패턴으로 구성한 것이 Factory Method 패턴이다.


Factory Method 패턴에서는 인스턴스 만드는 방법을 상위 클래스에서 결정하지만 구체적인 이름까지는 결정하지 않는다.



구체적인 내용은 모두 하위 클래스에서 수행한다.

따라서 인스턴스 생성을 위한 골격(framework)과 실제의 인스턴스 생성의 클래스를 분리해서 생각할 수 있다.


---
<u>Java 언어로 배우는 디자인 패턴 입문 참고</u>