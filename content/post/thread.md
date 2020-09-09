---
title: "스레드(Thread)"
date: 2020-09-02T02:02:00+09:00
---

### 스레드(Thread)


- **멀티 태스킹**

  파일을 인쇄하며 동시 문서를 편집하는 것처럼 컴퓨터 CPU 하나로 병렬 작업 가능하다.

  운영체제가 CPU 시간을 쪼개서 각 작업들에 할당하여 작업들이 동시에 수행되는 것처럼 보인다.

    
- **멀티 스레딩**

  멀티 태스킹과 같은 병렬 작업을 하나의 애플리케이션으로 가져온 것이다.

  하나의 애플리케이션 안에서 여러 작업을 동시에 한다.

  (음악 재생 애플리케이션은 mp3파일을 다운받으며 동시에 압축을 풀어 음악재생)

  자바는 멀티 스레딩을 프로그래머들에게 언어 수준에서 제공한다.  
  (자바 런타임 시스템에 의하여 동시 실행)

- **프로세스와 스레드**

  프로세스, 스레드의 가장 근본적인 차이점은 프로세스는 자신만의 데이터를 가지는데 반하여

  스레드들은 동일한 데이터를 공유한다. 따라서 같은 변수, 데이터의 **동기화**에 신경써야한다.

--- 

- **스레드 생성과 실행**

  ```java
  Thread t = new Thread(); // 스레드 객체 생성
  t.start();	// 스레드 시작(작업내용은 Thread클래스 안의 run()에 구현한다.)
  ```

  스레드 생성 작업에는 두가지 방법이 있다.
	
  **1) Thread 클래스를 상속하는 방법**
    
  ```java
  class MyThread extends Thread {
    public void run(){
      ...
    }
  }

  Thread t = new MyThread();
  t.start();
  ```

  **2) Runnable 인터페이스를 구현하는 방법**

  ```java
  class MyRunnable implements Runnable {
      public void run() {
          ...
      }
  }

  Thread t = new Thread(new MyRunnable());
  t.start();
  ```

  두 방법 중에서 Runnable 인터페이스를 구현하는 방법이 더 일반적이다.

  Thread를 상속받을 경우 다른 클래스로부터 상속받을 수 없어 Runnable 구현이 더 유연하다.

--- 
* **스레드 스케줄링**

  스케줄링에 의하여 하나의 CPU를 여러 스레드가 나눠 쓸 수 있다.

  CPU를 스레드가 나누어 쓰기 위해서 어떤 순서로 스레드를 수행할지 결정하는 스케줄링이 필요하다.

  자바 런타임 시스템은 **우선 순위(priority) 스케줄링**을 이용한다.

  스레드는 생성되면 Thread클래스에 정의된 MIN_PRIORITY(1)와 MAX_PRIORITY(10) 사이에서 우선순위를 배정받는다.

  자신을 생성한 스레드로부터 우선 순위를 상속받는다.

  * 멀티 스레드 우선순위 정하기

    * 우선순위 방식

      우선 순위가 높은 스레드가 실행상태를 더 많이 가지도록 스케줄링하며 setPriority 메소드로 우선순위 설정

    * 순환 할당 방식

      시간 할당량을 정하여 하나의 스레드를 정해진 만큼 실행 (JVM에 의해 결정)

  * 스케줄링 관련 메소드

    * sleep() :  다른 스레드와 보조를 맞추는 용도, 스레드가 수면 상태인 동안 인터럽트되면 InterruptedException 발생

    * 인터럽트(interrupt) : 하나의 스레드가 실행되는 작업을 중지시킨다.
  
      ```java
      for(int i = 0; i < 10; i++){
          try {
              Thread.sleep(1000);
          } catch (InterruptedException e){
              // 인터럽트를 받은 것으로 단순 리턴
              return;
          }
      }
      ```
      
    * 조인(Joins)
    
      하나의 스레드가 다른 스레드의 종료를 기다리게 하는 메소드.
    
      t가 현재 실행중인 스레드 객체이면 다음은 t가 종료될 때까지 기다린다.
    
      > t.join();
      
    * yield() : CPU를 다른 스레드에게 양보하는 메소드
    
      ​			 동일한 우선순위를 가지고 있는 다른 스레드를 실행시킬 때 사용한다.



* **스레드의 상태**

  * Runnable 상태 : 쓰레드가 실행되기 위한 준비 단계	

    ​		CPU를 점유하고 있지 않으며, Running 상태를 위해 대기하고 있는 상태이다.

    ​		start() 메소드를 호출하면  run() 메소드에 설정된 스레드가 Runnable 상태로 진입한다.

   * Running 상태 : 스케줄러에 의해 선택된 스레드가 실행되는 단계

     ​		CPU를 점유하여 실행하고 있는 상태이며 run() 메소드는 JVM만이 호출 가능하다.

     ​		Runnable 상태에 있는 여러 스레드 중 우선순위를 가진 스레드가 결정되면 JVM이 자동으로 run()을 호출하여

     ​		스레드가 Running 상태로 진입한다.

   * Blocked 상태 : 쓰레드가 작업을 완수하지 못하고 잠시 작업을 멈추는 단계

     ​		CPU의 점유권을 상실하여 이후에 Runnable 상태로 전환한다.

     ​		wait()메소드에 의해 Block 상태가 된 스레드는 notify() 메소드가 호출되면 Runnable 상태로 된다.

     ​		sleep() 메소드에 의해 Block 상태가 된 스레드는 지정 시간이 지나면 Runnable 상태로 된다.

   * Dead 상태 : Running 상태에서 스레드가 모두 실행되고 난 후 완료 상태

![img](https://user-images.githubusercontent.com/66955409/91877041-6e7c5100-ecb8-11ea-9df2-6dea89169540.png)


--- 
* **동기화**

  동기화란 공유된 자원 중에서 동시에 사용하면 안되는 자원을 보호하는 도구

  하나의 스레드가 작업이 끝날때까지 다음 스레드가 기다려야하는 데 이러한 영역을 임계 영역(ciritical section)이라 한다.

  동기화로 발생할 수 있는 문제는 스레드 간섭(Thread interference), 메모리 일치 오류(memory consistency error)가 있다.
  * 스레드 간섭

    서로 다른 스레드에서 실행되는 두 연산이 동일 데이터에 적용되며 겹치는 것을 의미

    데이터들이 섞이고 값이 이상하게 변경된다.

  * 메모리 불일치 오류

    서로 다른 스레드가 동일한 데이터의 값을 서로 다르게 볼 때 발생할 수 있다.

    counter 변수를 스레드 A, B가 공유하는데

    counter++; 를 스레드 A가 증가시켰으나 B에서 증가된 값을 가질 것이라는 보장이 없다.
    
    

* **동기화 선언 방법**

  * 메소드 동기화
  
    동기화된 메소드를 만들기 위해 synchronized 키워드를 메소드 선언에 붙일 수 있다.
  
    synchronized 키워드가 있으면 하나의 스레드가 공유 메소드를 실행하는 동안 다르 스레드는 실행할 수 없다.
  
    ```java
    public synchronized void 메소드 () {
     	...   
    }
    ```
  
  * 구간 동기화
  
    ```java
    public void 메소드() {
        ...
        synchronized(this){
            // 동기화 내용
        }
    }
    ```
  
    
  
* **ReentrantLock** 

  자바 5.0에서 추가된 클래스로 동기화에서 시작과 끝점을 수동적으로 설정이 가능한 명시적인 동기화이다.

  ```java
  import java.util.concurrent.locks.ReentrantLock;
  
  class SomeClass {
      private final ReentrantLock locker = new ReentrantLock();
      public void SomeMethod() {
          locker.lock(); // 쓰레드에 락을 건다.(동기화의 시작)
          try {
              // 동기화 내용
          } catch (Exception e) {
              // 예외 처리
          } finally {
              locker.unlock(); // 쓰레드의 락을 푼다.(동기화의 끝지점)
          }
      }
  }
  ```

  synchronized와의 차이점은 ReentrantLock이 훨씬 세부적으로 사용 가능하다는 점이다.

  가장 큰 차이점은 synchronized는 암묵적이고 ReentrantLock은 명시적이다.