---
title: "String & StringBuilder"
date: 2020-08-04T23:50:00+09:00
---

### String, StringBuffer, StringBuilder 차이 및 장단점

[참고링크](https://ifuwanna.tistory.com/221)

Java에서 문자열을 다루는 대표적인 클래스로 String, StringBuffer, StringBuilder가 있다.

연산이 많지 않을때는 위에 나열된 어떤 클래스를 사용하여도 문제없으나
연산횟수가 많아지거나 멀티쓰레드, Race Condition 등 상황이 발생하면 상황에 맞는 적절한 클래스 사용이 필요하다.



#### String vs StringBuffer/StringBuilder

String과 StringBuffer/StringBuilder 클래스의 가장 큰 차이점은 String은 **불변(immutable)**의 속성을 갖는다는 점이다.

```java
String str = "hello";	// String str = new String("hello");
str = str + " world";	// [hello world]
```

위의 예제에서 "hello" 값을 가지고 있던 String 클래스의 참조변수 str이 가리키는 곳에 저장도니 hello에 world 문자열을 더해
"hello world"로 변경한 것으로 착각할 수 있다.

하지만 기존에 "hello" 값이 들어가 있던 String 클래스의 참조변수 str이 "hello world"라는 값을 가지고 있는 **새로운 메모리영역**을 가리키게 변경된다.
처음 선언했던 "hello"로 값이 할당되어 있던 메모리 영역은 Garbage로 남아있다가 GC(Gabage Collection)에 의해 사라지게 된다.

String 클래스는 불변하기 때문에 문자열을 수정하는 시점에 새로운 인스턴스가 생성된 것이다.

이처럼 String은 불변성을 가지기 때문에 변하지 않는 문자열을 자주 읽어들이는 경우 String을 사용하면 좋은 성능을 기대할 수 있다.

그러나 문자열 추가, 수정, 삭제 등의 연산이 빈번히 발생하는 알고리즘에 String 클래스를 사용하면
힙메모리에 많은 임시 가비지가 생성되어 힙메모리 부족으로 어플리케이션 성능에 치명적인 영향을 끼치게 된다.