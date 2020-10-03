---
title: "Static 변수, 클래스"
date: 2020-10-03T15:45:00+09:00
---



Java에서 Static 키워드를 사용한다는 것은 메모리에 한번 할당되어

프로그램이 종료될 때 해제되는 것을 의미한다.



![static메모리](https://t1.daumcdn.net/cfile/tistory/99AAAC405CEC82C032)

일반적으로 우리가 만든 Class는 Static 영역에 생성되고,  
new 연산을 통해 생성한 객체는 Heap 영역에 생성된다.

객체의 생성시에 할당된 Heap 영역의 메모리는 Garbage Collector를 통해 수시로 관리를 받는다.

Static 키워드를 통해 Static 영역에 할당된 메모리는 모든 객체가 공유하는 메모리라는 장점을 갖지만,

Garbage Collector의 관리 영역 밖에 존재하므로 Static을 자주 사용하면  
프로그램 종료시까지 메모리가 할당된 채로 존재하므로  
자주 사용하게 되면 시스템의 퍼포먼스에 악영향을 주게 된다.



##### Static 변수의 특징

>* Static 변수는 클래스 변수이다.
>
>* 객체를 생성하지 않고도 Static 자원에 접근이 가능하다.

Static 변수와 Static 메소드는 Static 메모리 영역에 존재하므로 객체가 생성되기 이전에 이미 할당되어있다.

따라서 객체 생성없이 바로 접근할 수 있다.

[참고링크](https://mangkyu.tistory.com/47)



---

#### Static 정적

자바에서 static 이란 단어가 나오면 **공유**라는 단어를 기억하자

메모리에 하나만 만들어지고 그것을 나누어 사용하기 때문이다!



지역변수와 전역변수의 특징을 모두 가지고 있고

접근하려면 반드시 static 중괄호 블럭 안에 들어와 있어야 한다.



#### Static 변수

>* Static 변수는 인스턴스끼리 값을 공유한다.
>* Static 변수와 메서드는 인스턴스 생성없이 메모리에 스스로 올라간다.

##### 1. Static 변수는 인스턴스끼리 값을 공유한다.

```java
class StaticCar {
    static int var = 1;
    public StaticCar() {
        sysout(var++);
    }
}

public class StaticTest {
    public static void main(String[] args){
        StaticCar sc1 = new StaticCar(); // 결과 : 1
        StaticCar sc2 = new StaticCar(); // 결과 : 2
        StaticCar sc3 = new StaticCar(); // 결과 : 3
    }
}
```

<u>static 변수의 선언문은 처음 한번만 실행되고 이후부터는 무시된다.</u>

즉, 인스턴스끼리 값을 공유하게 된다.


---
##### 2. Static 변수와 메서드는 인스턴스 생성없이 메모리에 스스로 올라간다.

```java
class Var {
    static int num = 10;
    public int methodA() {
        return num += 2;
    }
}

public class StaticVar {
    public static void main(String[] args){
        int result = Var.num + 5;
        sysout(result); // 15
    }
}
```

static 변수와 메서드는 new 생성없이 메모리에 스스로 올라간다.

생성이 없고, 주소값 역시 없다.



java에서 . 앞에는 무조건 주소값이 온다. (car.money)

그러나 static 변수는 . 앞에 static이 선언된 class가 온다. (클래스이름.변수)

<u>static 변수를 '클래스 변수'라고 부르기도 한다.(클래스명을 . 앞에 붙이기 때문)</u>



---
#### Static으로 만드는 class

100% static으로 된 클래스를 만든다면?

* 속도가 엄청나게 빨라진다.

* 코드가 간결하다(new 인스턴스 생성코드가 없어진다.)

* <u>가비지 컬렉션 대상이 아니므로 남용 시 메모리 자원이 부족해 질 수 있다.</u>

  (공유하므로 누가 사용할지 모른다. 따라서 지울 수 없다.)

* static 메서드 사용 시 메서드 내부에는 인스턴스 변수와  인스턴스 메서드를 호출할 수 없다.

  (static 메서드 내에서는 static 변수와 static 메서드만 가능하다.)

[참고링크](https://ggmouse.tistory.com/329)