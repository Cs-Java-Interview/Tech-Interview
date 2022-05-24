# Transaction's Isolation Level

## 정의
여러 트랜잭션이 동시에 처리될 때
특정 트랜잭션이 `다른 트랜잭션에서 변경하거나 조회하는 데이터를 볼 수 있게 허용할지 말지`를 결정하는 것

## 격리수준에 따른 세 가지 부정합의 문제
|   |DIRTY READ |NON-REPEATABLE READ |PHANTOM READ |
|---|---|--- |---|
|READ UNCOMMITTED |O |O |O            |
|READ COMMITTED   |X |O |O            |
|REPEATABLE READ  |X |X |O (InnoDB X) |
|SERIALIZABLE     |X |X |X            |

### DIRTY READ
- 어떤 트랜잭션에서 처리한 작업이 완료되지 않았는데도 다른 트랜잭션에서 볼 수 있는 현상
- READ UNCOMMITTED 격리수준에서 허용된다.
- 롤백되기 전에 확인한 데이터가 롤백 이후 없어질 수 있기 때문에, 데이터가 나타났다가 사라졌다 하는 현상을 초래한다.

### NON_REPEATABLE READ
- `REPEATABLE READ`
  - 하나의 트랜잭션 내에서 똑같은 SELECT 쿼리를 실행했을 때 항상 같은 결과를 보장해야 한다.
- REPEATABLE READ 정합성에 어긋나는 경우 발생
- 즉 하나의 트랜잭션 내에서 똑같은 SELECT 쿼리를 실행했을 때 다른 결과를 가져오는 현상

### PHANTOM READ
- 다른 트랜잭션에서 수행된 작업에 의해 레코드가 보였다 안 보였다 하는 현상

## 특징
`READ UNCOMMITTED` > `READ COMMITTED` > `REPEATABLE READ` > `SERIALIZABLE`
- 뒤로 갈수록 격리(고립)정도가 높아지며, 동시 처리 성능은 낮아진다.
- 격리 수준이 높아질수록 MySQL 서버의 성능이 많이 떨어진다고 생각할 수 있지만, 
- `SERIALIZABLE` 수준이 아니라면 크게 성능의 개선이나 저하는 발생하지 않는다.

## 종류별 격리수준
### READ UNCOMMITTED
- #### 문제 : `DIRTY READ` 발생
- RDBMS 표준에서는 트랜잭션의 격리 수준으로 인정하지 않는 경우가 많다.
- 일반적인 데이터베이스에서는 거의 사용하지 않는다.

### READ COMMITTED
- Oracle DBMS 에서 기본으로 사용되는 격리수준이다.
- DIRTY READ 발생 X
  - 어떤 트랜잭션에서 데이터를 변경했더라도 다른 트랜잭션에서는 `commit` 완료된 데이터만 조회할 수 있다.
- 예시
  - 사용자가 데이터를 변경하면 그 값은 테이블에 즉시 기록되고, 변경한 데이터의 이전 값은 언두 영역으로 백업된다.
  - 커밋 전에 이 데이터가 조회될 경우 테이블이 아니라 언두 영역에 백업된 레코드에서 가져온다.
  - 최종적으로 커밋이 수행되고 나면 그 때부터는 변경된 값으로 조회된다.
- #### 문제 : `NON-REPEATABLE READ` 라는 부정합 발생

### REPEATABLE READ
- MySQL InnoDB 스토리지 엔진에서 기본으로 사용되는 격리수준이다.
- NON-REPEATABLE READ 발생 X
- MVCC(Multi Version Concurrency Control)
  - InnoDB 스토리지 엔진에서 레코드 값을 변경하는 방식
  - 트랜잭션이 rollback 될 가능성을 대비해 변경 전 레코드를 언두(Undo) 공간에 백업해두고 실제 레코드 값을 변경

#### `READ COMMITTED` 와의 차이점
- MVCC를 이용해 commit 전 데이터를 보여주는 READ COMMITTED 격리수준과의 차이점은
- `백업 레코드의 버전` 가운데 몇 번째 이전 버전까지 찾아 들어가느냐
- 언두 영역에 `백업되는 모든 레코드`는 `변경을 발생시킨 트랜잭션의 번호를 포함`하고 있다.
- 언두 영역에 백업된 데이터는 InnoDB 스토리지 엔진이 불필요하다고 판단하는 시점에 주기적으로 삭제한다.
- REPEATABLE READ 격리수준에서는 MVCC 보장을 위해
  - 실행 중인 트랜잭션 가운데 가장 오래된 트랜잭션 번호보다 트랜잭션 번호가 앞선 언두 영역의 데이터는 삭제할 수 없다.
- 트랜잭션 안에서 실행되는 모든 SELECT 쿼리는 트랜잭션 번호가 `자신의 번호보다 작은 트랜잭션 번호에서 변경한 것만` 보게 된다.

### SERIALIZABLE
- `PHANTOM READ` 발생 X
- 가장 단순하면서 가장 엄격한 격리수준이다.
- 동시 처리 성능이 다른 트랜잭션 격리수준보다 떨어지므로 동시성이 중요한 데이터베이스에서는 거의 사용되지 않는다.
- InnoDB 테이블에서 기본적으로 순수한 SELECT 작업은 Non-locking consistent read(잠금이 필요없는 일관된 읽기)로 실행된다.
- 트랜잭션의 격리 수준이 SERIALIZABLE 로 설정되면 읽기 작업도 공유 잠금(읽기 잠금)을 얻어야만 한다.
- 따라서 읽기 작업 중임에도 다른 트랜잭션은 레코드를 변경하지 못하게 되는 것이다.
- 즉 한 트랜잭션에서 읽고 쓰는 레코드를 다른 트랜잭션에서는 절대! 접근할 수 없게 된다.