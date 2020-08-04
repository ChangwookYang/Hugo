---
title: "String & StringBuilder"
date: 2020-08-04T23:50:00+09:00
---

### String, StringBuffer, StringBuilder 차이 및 장단점

  --- 
  
#### Java에서 문자열을 다루는 대표적인 클래스로 String, StringBuffer, StringBuilder가 있다.

연산이 많지 않을때는 어떤 클래스를 사용하여도 문제없으나  
연산횟수가 많아지거나 멀티쓰레드, Race Condition 등 상황이 발생하면  
상황에 맞는 적절한 클래스 사용이 필요하다.  
(Race condition이란, 두 개 이상의 스레드가 공유된 자원에 접근하는 상황)

---
#### String vs StringBuffer/StringBuilder

String과 StringBuffer/StringBuilder의 가장 큰 차이점은 String은 **불변**(immutable)의 속성을 갖는 것이다.

```java
String str = "hello";	// String str = new String("hello");
str = str + " world";	// [hello world]
```

위의 예제에서 "hello" 값을 가지고 있던   
String의 참조변수 str이 가리키는 곳에 저장되니 hello에 world 문자열을 더해  
"hello world"로 변경한 것으로 *착각*할 수 있다.

하지만 기존에 "hello" 값이 들어가 있던 String 클래스의 참조변수 str이   
"hello world"라는 값을 가지고 있는 **새로운 메모리영역**을 가리키게 변경된다.  
처음 선언했던 "hello"로 값이 할당되어 있던 메모리 영역은 Garbage로 남아있다가 GC(Gabage Collection)에 의해 사라지게 된다.  
String 클래스는 불변하기 때문에 **문자열을 수정하는 시점에 새로운 인스턴스가 생성**된 것이다.

![image](https://user-images.githubusercontent.com/66955409/89311436-f3238180-d6b0-11ea-9571-04364241e9a0.png)

이처럼 String은 불변성을 가지기 때문에 변하지 않는  
**문자열을 자주 읽어들이는 경우 String을 사용하면 좋은 성능**을 기대할 수 있다.

그러나 문자열 추가, 수정, 삭제 등의 연산이 빈번히 발생하는 알고리즘에 String 클래스를 사용하면  
힙메모리에 많은 임시 가비지가 생성되어 힙메모리 부족으로 어플리케이션 성능에 치명적인 영향을 미친다.


---
이를 해결하기 위해 Java에서 **가변(mutable)성을 갖는 StringBuffer / StringBuilder 클래스**를 도입하였다.  
    
String과는 반대로 StringBuffer/StringBuilder는 가변성을 가지기 때문에 append(), delete()등의 API를 이용하여
**동일 객체 내에서 문자열을 변경하는 것이 가능**하다.  
따라서 문자열 추가, 수정, 삭제가 빈번하게 발생할 경우면 String 클래스가 아닌 StringBuffer/StringBuilder를 사용해야한다.

```Java
StringBuffer sb = new StringBuffer("hello");
sb.append("world");
```

![image](https://user-images.githubusercontent.com/66955409/89311496-0898ab80-d6b1-11ea-9bbf-dfb4a6d1b05d.png)

--- 
#### StringBuffer vs StringBuilder

동일한 API를 가지고 있는 StringBuffer, StringBuilder의 차이점은 무엇일까?

가장 큰 차이점은 동기화의 유무로써 **StringBuffer는 동기화 키워드를 지원**하여 멀티쓰레드 환경에서 안전하다는 점이다.(thread-safe)  
String도 불변성을 가지기 때문에 멀티쓰레드 환경에서의 안정성(thread-safe)을 가지고 있다.



반대로 StringBuilder는 동기화를 지원하지않기 때문에 멀티쓰레드 환경에서 사용하는 것은 적합하지 않지만
동기화를 고려하지 않는 만큼 **단일쓰레드에서의 성능은 StringBuffer보다 뛰어나다**.




---

### 정리

컴파일러에서 분석할 때 최적화에 따라 다른 성능이 나올수도 있지만 일반적인 경우는 아래와 같다.

String : 문자열 연산이 적고 멀티쓰레드 환경일 경우

StringBuffer : 문자열 연산이 많고 멀티쓰레드 환경일 경우

StringBuilder : 문자열 연산이 많고 단일쓰레드이거나 동기화를 고려하지 않아도 되는 경우

|                  | String        | StringBuffer | StringBuilder |
| ---------------- | ------------- | ------------ | ------------- |
| **Storage**      | String pool   | Heap         | Heap          |
| **Modifiable**   | No (immutable) | Yes (mutable) | Yes (mutable)  |
| **Thread safe**  | Yes           | Yes          | No            |
| **Synchronized** | Yes           | Yes          | No            |
| **Performance**  | Fast          | Slow         | Fast          |


[참고링크](https://ifuwanna.tistory.com/221)
