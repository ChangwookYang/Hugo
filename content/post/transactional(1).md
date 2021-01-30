---
title: "@Transactional(1)"
date: 2021-01-30T12:30:00+09:00
---

##### Spring에서 JPA 기술을 쓸 때 빼놓을 수 없는 기능 중 하나

* transaction begin, commit을 자동 수행해준다.

* 예외 발생 시, rollback 처리를 자동 수행해준다.



> 예제코드

```java
public class BooksImpl implements Books {
	public void addBooks(List<String> bookNames){
        bookNames.forEach(bookName -> this.addBook(bookName));
    }	    
    
    @Transactional
    public void addBook(String bookName){
        Book book = new Book(bookName);
        bookRepository.save(book);
        book.setFlag(true);
    }
}
```

위의 코드에서 문제는 addBooks 메서드 내부에서 호출하는 addBook 메서드의 

@Transactional 어노테이션이 적용되지 않는 것!

그렇기에 해당 코드를 실행하더라도 Database에 저장된 Book 정보에 Flag 컬럼에 정상적응로 업데이트 안된다.

bookRepository의 save메서드는 자체적으로 @Transactional 어노테이션이 붙어있어, 

정상적으로 코드가 수행되어 DB 인서트를 수행한다.

하지만 update의 역할을 하는 book 객체의변경감지가 동작하지 않아, Flag 컬럼에 업데이트가 되지 않는다.

왜 동작 안했는지는 Spring의 @Transactional 기능의 제공방식에 대해 살펴봐야한다.

---



### Spring @Transactional 기능제공 방식

JPA의 객체 변경 감지는 transaction이 commit이 될 때, 작동한다.

그렇기에 Spring은 @Transactional 어노테이션을 선언한 메서드가 실행되기전,

transaction begin 코드가 삽입되며 메서드가 실행된 후,

transaction commit 코드를 삽입하여 객체 변경감지를 수행하게 유도한다.

##### Spring 코드의 삽입방법 2가지

1. 바이트 코드 생성 (CGLIB 사용)
2. 프록시 객체 사용

2가지 방법 중 Spring은 기본적으로 프록시 객체 사용이 선택된다.

그렇기에 interface가 반드시 필요하다.

SpringBoot는 기본적으로 바이트 코드 생성이 선택되어 굳이 interface가 필요없다.

개발환경이 SpringBoot라면 Books 인터페이스 없이 봐도 무방하다.



원리는, 프록시 객체로 우리가 만든 메서드를 한번 감싸서, 메서드 위 아래로 코드를 삽입해준다.

```java
public class BooksProxy {
    private final Books books;
    private final TransactionManager manager = TransactionManager.getInstance();
    
    public BooksProxy(Books books){
        this.books = books;
    }
    
    public void addBook(String bookName){
        try {
            manager.begin();
            books.addBook(bookName);
            manager.commit();
        } catch(Exception e) {
            manager.rollback();
        }
    }
}
```

그렇기에, 우리는 BooksImpl 자료형을 사용할 때, 스프링이 제공하는 BooksProxy 객체를 사용하게 되며,

BooksProxy 객체가 제공하는 addBook 메서드를 사용해야만 transaction 처리가 수행되는 것이다.



### @Transactional이 수행되지 않은 경우

```java
public class BooksImpl implements Books {
  public void addBooks(List<String> bookNames) {
    bookNames.forEach(bookName -> this.addBook(bookName));
  }
  
  @Transactional
  public void addBook(String bookName) {
    Book book = new Book(bookName);
    bookRepository.save(book);
    book.setFlag(true);
  }
}
```



BooksProxy가 addBooks 메서드를 수행하면 아래와 같은 순서로 작동된다.

>  BooksProxy::addBooks -> BooksImpl::Book

즉, BooksImpl 내부의 코드 addBook이 수행되기 때문에

해당 메서드는 프록시로 감싸진 메서드가 아니라는 점에서

 @Transactional 어노테이션이 수행되지 않는다.



### 해결방법

@Transactional 메서드를 내부적으로 사용하지 않는 것이 근본적인 해결책이다.

굳이 사용하려면 의존성 주입을 이용하여 Proxy 인스턴스를 자체적으로 가져와 사용한다.

```java
@Service
public class BooksImpl implements Books {
    @Autowired
    private Books self;
    
    public void addBooks(List<String> bookNames){
     	bookNames.forEach(bookName -> self.addBook(bookName));
        // this가 아닌 self 변수로
    }
    
    @Transactional
    public void addBook(String bookName){
        BOok book = new Book(bookName);
        bookRepository.save(book);
        book.setFlag(true);
    }
}
```

해당 코드는 Books 인터페이스를 이용하여 BooksProxy 인스턴스를 주입할 수 있도록 유도한다.

그 후, 자기 자신 즉 this가 가지고 있는 순수한 addBook 메서드가 아니라

proxy로 감싸진 addBook 메서드를 통해 @Transactional 어노테이션 기능을 사용할 수 있게된다.



### 정리하자면

@Transactional 어노테이션 메서드는 클래스 내부적으로 사용하지 말고,

프록시 객체를 활용하여 밖에서 사용하자











참고 : https://mommoo.tistory.com/92