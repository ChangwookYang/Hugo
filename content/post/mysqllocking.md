---
title: "MySQL Locking"
date: 2021-02-24T22:00:00+09:00
---

웹 프로그래밍은 기본적으로 다중 사용자 환경이다.

동일한 자원에 여러 사용자가 동시에 접근하는 상황이 발생할 수 있다.

쇼핑몰에서 물건을 구매하는 상황에서, 두 명의 사용자가 하나밖에 없는 제품을 구매하려한다.

서버로 요청이 넘어가면 다음 세단계를 거쳐 구매를 처리한다고 하자

1. 다시 한번 재고를 확인한다.
2. 재고를 감소시킨다.
3. 구매 정보를 입력한다.

> 재고를 확인하고 구매할 수 있다고 확인 후,
>
> 재고를 감소하기 전에 다른 요청을 처리하는 쓰레드 또는 프로세스가
>
> 재고를 확인하게 되면 둘 다 구매할 수 있다고 판단하게 되고 구매 처리가 되어버린다.



#### MySQL 경우, 어떻게 처리하는가?

MySQL은 테이블이 MyISAM인 경우와 InnoDB인 경우 서로 다른 방법을 사용할 수 있다.



#### 1. Table-Level Locking

테이블이 MyIASAM인 경우에는 테이블 수준의 잠금기능을 사용할 수 있다.

이 방법은 테이블 전체를 잠그기 때문에 다중 사용자 환경에서는 사용하지 않는게 좋다.

테이블에 10개 제품이 있으면 하나를 구매하는 동안에도 다른 9개를 구매하지 못하게 된다.

테이블의 잠금은 **READ lock과 WRITE lock**이 있다.

- READ lock을 걸면 UNLOCK 하기 전까지 다른 세션에서 읽을 수는 있지만

  입력, 수정, 삭제는 할 수 없고 대기하게 된다.

  ```sql
  mysql> LOCK TABLES tb_stock READ;
  ...
  mysql> UNLOCK TABLES;
  ```

- WRITE lock을 걸면 UNLOCK 하기 전까지 다른 세션에서는 읽을 수도 없고 대기하게 된다.

  ```sql
  mysql> LOCK TABLES tb_stock WRITE;
  ...
  mysql> UNLOCK TABLES;
  ```

  

#### 2. Row-Level Locking

MySQL 테이블이 InnoDB라면 행 수준의 잠금을 사용할 수 있다.

행수준의 잠금은 하나의 행을 읽고, 수정할 때 다른 행들에 대한 액세스는 허용될 수 있는 잠금이다.

* 트랜잭션을 시작하고 FOR UPDATE 문을 사용하면 조회 된 행에 대해서는 commit 또는 rollback으로

  트랜잭션이 종료될 때까지 조회, 입력, 수정, 삭제가 모두 블록된다.

* 주의해야할 것은 MySQL이 조회된 행을 기억하는 방법이 인덱스 범위를 기억하는 것이므로

  조건 필드에 인덱스가 없으면 전체 테이블에 lock이 걸린다.

  ```sql
  mysql> START TRANSACTION;
  mysql> SELECT * FROM tb_stock WHERE id=1 FOR UPDATE;
  ...
  mysql> COMMIT;
  ```

* 공유 모드로 잠그면 다른 세션에서 읽기는 가능하고 수정, 삭제는 못하도록 할 수 있다.

  ```sql
  mysql> START TRANSACTION;
  mysql> SELECT * FROM tb_stock WHERE id=1 LOCK IN SHARE MODE;
  ...
  mysql> COMMIT;
  ```

* Row-Level Lock을 사용할 때는 데드락(Dead-Lock)이 발생하지 않도록 주의해야한다.

  하나의 Row를 잠그고, 잠긴 다른 Row가 풀릴 때까지 서로 기다리는 코드를 작성하게 되면

  서로 록을 해제할때까지 무한히 대기하는 데드락이 발생할 수 있다.



#### 3. Optimistic Lock (낙관적 잠금)

낙관적 잠금은 실제로 Lock을 얻어서 잠그는 것이 아니고 단일 쿼리는 원자적이므로

하나의 쿼리가 실행되는 동안에는 끼어들 수 없다는 것을 이용하여 동시성을 제어하는 방법이다.

> 업데이트와 체크를 동시에 한 쿼리로 수행하는 것

1. 테이블에 정수형 필드를 하나 두고 조회 시 그 값을 읽어온다.

   ```sql
   mysql> SELECT quantity, opt_flag FROM tb_stock WHERE id = 1;
   ```
   

2. 업데이트 시 이 값을 조건절에 걸어 업데이트한다.

   ```sql
   mysql> UPDATE tb_stock 
   		  SET quantity=quantity-1, opt_flag = opt_flag+1
   		WHERE opt_flag = '조회 시 opt_flag값'
   		  AND id = 1;
   ```

3. 업데이트 쿼리의 affected row가 0이면 조회 당시의 opt_flag가 변경되었음을 의미

   내가 조회하고 업데이트하기 전에 누군가 먼저 업데이트를 했다는 것을 뜻하므로 실패로 처리한다.

   

위의 쇼핑몰에서 구매하는 예를 사용하여 현실적인 예는 다음과 같다.

```sql
UPDATE tb_stock
SET quantity = quantity-1
WHERE quantity-1 >= 0
  AND id = 1;
```

웹프로그래밍의 경우, 세션이 물리적으로 유지되지 않으므로 조회 당시에 물건을 구매할 수 있다고 보여도

구매 버튼을 누르면 다른 사람이 먼저 구매해 버려서 구매할 수 없는 상황이 발생할 수 있다.

이런 상황을 받아들일 수 있는 응용에서는 낙관적인 잠금이 빠른 성능을 유지하면서도 동시성을 제어할 수 있는

좋은 방법이 된다.