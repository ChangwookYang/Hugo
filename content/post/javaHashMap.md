---
title: "Java HashMap의 동작 원리"
date: 2021-02-20T23:30:00+09:00
---



### Java HashMap은 어떻게 동작하는가?

HashMap은 Java Collection Framework에 속한 구현체 클래스이다.

이글에서는 어떤 방식으로 HashMap 구현체의 성능을 향상시켰는지 소개한다.

구체적인 내용은 Amortized Constant Time 을 위하여 어떻게 해시 충돌 가능성을 줄이고 있는가이다.



#### HashMap 과 Hash Table

HashMap 과 HashTable은 Java의 API 이름이다.

HashTable 또한 Map 인터페이스를 구현하고 있기 때문에 HashMap과 HashTable이 제공하는 기능은 같다.

다만 HashMap은 보조 해시 함수(Additional Hash Function)을 사용하기 때문에

보조 해시 함수를 사용하지 않는 HashTable에 비하여 해시충돌이 덜 발생할 수 있어 성능상 이점이 있다.

HashMap과 HashTable을 정의한다면

> 키에 대한 해시 값을 사용하여 값을 저장하고 조회하며,
>
> 키-값 쌍의 개수에 따라 동적으로 크기가 증가하는 associate array 이다.
>
> associate array를 지칭하는 다른 용어의 대표적으로 Map, Dictionary, Symbol Table 등이 있다.

```java
// HashTable
public class 8ccce55530bc3477c678dd9921b60f3e.gifHashtable<K,V> extends Dictionary<K,V> 
    implements Map<K,V>, Cloneable, java.io.Serializable {

// HashMap  
public class 928b3cc3fe40d69cd06cbe7f5f3767f8.gifHashMap<K,V> extends AbstractMap<K,V> 
    implements Map<K,V>, Cloneable, Serializable {
```

associatibe array를 지칭하기 위하여 HashTable에서는 Dictiaonary라는 이름을 사용하고

HashMap에서는 Map이라는 용어를 사용하고 있다.

map은 원래 수학 함수에서의 대응 관계를 지칭하는 용어로, 경우에 따라서는 함수 자체를 의미하기도한다.

즉 HashMap이란 이름에서 알 수 있듯이, 

HashMap은 키 집합인 정의역과 값 집합인 공역의 대응에 해시 함수를 이용한다.

![image](https://d2.naver.com/content/images/2015/06/helloworld-831311-1.png)



#### 해시 분포와 해시 충돌

동일하지 않은 어떤 객체 X와 Y가 있을 때, 즉 X.equals(Y)가 '거짓'일 때

X.hashCode() != Y.hashCode()가 같지 않다면, 이 때 사용하는 해시함수는 완전한 해시함수라 한다.



Boolean같이 서로 구별되는 객체의 종류가 적거나, Integer, Long, Double같은 Number객체는

객체가 나타내려는 값 자체를 해시 값으로 사용할 수 있기 때문에 완전한 해시함수 대상으로 삼을수 있다.

하지만 String이나 POJO에 대하여 완전한 해시 함수를 제작하는 것은 사실상 불가능하다.



적은 연산만으로 빠르게 동작할 수 있는 완전한 해시함수가 있다고 하더라도

그것을 HashMap에서 사용할 수 있는 것은 아니다.

HashMap은 기본적으로 각 객체의 hashCode() 메서드가 반환하는 값을 사용하는데 결과 자료형은 int다.

32비트 자료형으로는 완전한 자료 해시함수를 만들 수 없다. 

논리적으로 생성 가능한 객체의 수가 2^32보다 많을 수 있기 떄문이며,

또한 모든 HashMap 객체에서 O(1)을 보장하기 위해 랜덤 접근이 가능하게 하려면

원소가 2^32인 배열을 모든 HashMap이 가지고 있어야 하기 때문이다.



따라서 HashMap을 비롯한 많은 해시 함수를 이용하는 associative array 구현체에서는

메모리를 절약하기 위하여 실제 해시 함수의 표현 점수 범위보다 작은 M개의 원소가 있는 배열만을 사용한다.

따라서 다음과 같이 객체에 대한 해시 코드의 나머지 값을 해시 버킷 인덱스 값으로 사용한다.

```java
int index = X.hashCode() % M;
```



이 코드와 같은 방식을 사용하면, 서로 다른 해시코드를 가지는 서로 다른 객체가 1/M의 확률로 해서

같은 해시 버킷을 사용하게 된다.

이는 해시 함수가 얼마나 해시 충돌을 잘 회피하도록 구현되었느냐에 상관 없이

발생할 수 있는 또다른 종류의 해시 충도리다.

이렇게 해시 충돌이 발생하더라도 키-값 쌍 데이터를 잘 저장하고 조회할 수 있게 하는 방식에는

대표적으로 두가지가 있는데, 하나는 Open Addressing이고, 다른 하나는 Separate Chaining 이다.

이 둘 외에도 해시 충돌을 해결하는 다양한 자료구조가 있지만, 거의 이 둘을 응용한 것이다.

![image](https://d2.naver.com/content/images/2015/06/helloworld-831311-4.png)



Open Addressing은 데이터를 입력하려는 해시 버킷이 이미 사용 중인 경우

다른 해시 버킷에 해당 데이터를 삽입하는 방식이다.

데이터를 저장/조회할 해시 버킷을 찾을 때에는 Linear Probing, Quadratic Probing 등의 방법을 사용한다.

Spatrate Chaining에서 각 배열의 인자는 인덱스가 같은 해시 버킷을 연결한

링크드 리스트의 첫 부분(head)이다.

둘 모두 WorstCase O(M)이다. 하지만 Open Addressing은 연속된 공간에 데이터를 저장하여

Separate Chaining에 비하여  캐시 효율이 높다. 따라서 데이터 개수가 충분히 적다면

Open Addressing이 Separate Chaining보다 더 성능이 좋다.

하지만 배열의 크기(M)가 커질수록 캐시 효율이라는 Open Addressing의 장점이 사라진다.

L1, L2 캐시 적중률 (hit ratio)이 낮아지기 때문이다.



Java HashMap 에서 사용하는 방식은 Separate Chaining 이다.

Open Addressing은 데이터를 삭제할 때 처리가 효율적이기 어려운데.

HashMap에서 remove() 메서드는 매우 빈번하게 호출될 수 있기 때문이다.

게다가 HashMap에 저장된 키-값 쌍 개수가 일정 개수 이상으로 많아지면,

일반적으로 Open Addressing 보다는 Separate Chaining이 빠르다.

Open Addressing의 경우 해시 버킷을 채운 밀도가 높아질수록 Worst Case 발생 빈도가 더 높아진다.

반면 Separate Chaining 방식의 경우 해시 충돌이 잘 발생하지 않도록 조정할 수 있다면

WorstCase가 발생하는 것을 줄일 수 있다.(보조 해시 함수)









참고 : https://d2.naver.com/helloworld/831311

