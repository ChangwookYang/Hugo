---
title: "Array 정렬"
date: 2020-09-14T23:00:00+09:00
---

### Array 내림차순 정렬



Primitive type Array 내림차순 정렬을 어떻게 할지 알아보았다.

stream을 쓴것도 있길래 좀 더 클래식한 코드가 없는지 찾아보다가

temp 변수로 swap을 사용한 소스를 보게되자 stream에 더 익숙해져야되겠다는 생각을 했다.

참고 : [Java 오름차순정렬, 내림차순정렬](https://defacto-standard.tistory.com/18)



##### Wraaper 클래스

```java
// Wrapper 클래스
Integer[] arr = {3, 2, 4, 5, 1};

// 매우 쉽다!
Arrays.sort(arr); // 오름차순
Arrays.sort(arr, Comparator.reverseOrder());	// 내림차순
Arrays.sort(arr, Collections.reverseOrder());	// 내림차순
```

##### Primitive Type
```java
// primitive type array
int[] arr = {3, 2, 4, 5, 1};

// Arrays.sort(T[] a, Comparator<? super T> c) 사용
Integer[] integerArr = Arrays.stream(arr).boxed().toArray(Integer[]::new);
Arrays.sort(integerArr, Comparator.reverseOrder());

// Collections.sort(List<T> list, Comparator<? super T> c) 사용
List<Integer> integerArr1 = Arrays.stream(arr)
    								.boxed()
                                    .sorted(Comparator.reverseOrder())
                                    .collect(Collectors.toList());

List<Integer> integerArr2 = Arrays.stream(arr)
    								.boxed()
    								.collect(Collectors.toList());
Collections.sort(integerArr2, Comparator.reverseOrder());

```

치트키 같은 코드들이다.



---

### 참고

Primitive 자료형, Wraaper 클래스 비교! (참고 : [[Java] Integer와 int의 차이](https://includestdio.tistory.com/1))



##### 1.  Primitive 자료형 - Wrapper 클래스 관계

|          | 설명                    |
| ----------- | ------------------------------------------------------------ |
| **int**     | **primitive 자료형 (long ,float, double...)**                |
|             | 산술 연산이 가능하다                                         |
|             | null 로 초기화 불가능하다.                                   |
| **Integer** | **Wrapper 클래스(객체)**                                     |
|             | Unboxing하지 않으면  산술연산이 불가능하지만 null 값을 처리할 수 있다. |
|             | null 값 처리가 용이하여 SQL과 연동하여 사용할 수 있다.       |
|             | DB에서 자료형이 정수형이지만 null 값이 필요한 경우 VO에서 Integer를 사용할 수 있다. |



##### 2. int와 Integer 간의 변환

   * Boxing : Primitive 자료형 -> Wrapper 클래스
   * Unboxing : Wrapper 클래스 -> Primitive 자료형

   ```java
   // to int i from Integer ii
   int i = ii.intValue();
   
   // to Integer ii from int i
   Integer ii = new Integer(i);
   ```

   

##### 3. Auto boxing / unboxing

자바의 대부분의 경우에는 자동으로 boxing / unboxing을 해준다.

```java
int i = 1;
Integer integer = i;
int i2 = integer;
```  
  



##### 4. int와 Integer의 사이즈를 비교하는 재미난 실험  
  
* Integer 및 int 배열을 1,000,000 개 생성
* 결과
  * Integer : 19,986,824 byte
  * int : 3,998,536 byte
  * Integer가 약 5배 크다.

* 요약
  * Integer가 16 byte
  * Integer를 참조하는데 4 byte
  * Integer의 사이즈 = 16 +4 = 20 byte
  * int의 사이즈 = 4 byte
  * **5배 차이**