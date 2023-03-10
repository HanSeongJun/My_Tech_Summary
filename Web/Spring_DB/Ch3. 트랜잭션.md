## Ch3. 트랜잭션(Transaction)
### 트랜잭션
**데이터베이스의 상태를 변환**시키는 하나의 논리적 기능을 수행하기 위한 **작업의 단위** 또는 한꺼번에 모두 **수행되어야 할 일련의 연산**들을 의미
> **상태를 변환** : SQL 질의어를 통해 DB에 접근하는 것
> - SELECT
> - INSERT
> - DELETE
> - UPDATE

> **작업의 단위** : 많은 SQL 명령문들을 사람이 정하는 기준에 따라 정하는 것
> 
> EX) 사용자 A가 사용자 B에게 만원을 송금한다. 
> -> 이 때 DB의 작업은 다음과 같이 이루어진다.
> 1. 사용자 A의 계좌에서 만원을 차감 : UPDATE문을 사용해 사용자 A의 잔고를 변경 
> 2. 사용자 B의 계좌에 만원을 추가 : UPDATE문을 사용해 사용자 A의 잔고를 변경
>
> -> 현재 작업 단위 : 출금 UPDATE + 입금 UPDATE
> - 이를 통틀어 하나의 트랜잭션이라고 한다.
> - 위 두 쿼리문 모두 성공적으로 완료되어야만 "하나의 작업(트랜잭션)"이 완료되는 것이다. 이를 **Commit** 이라 부른다.
> - 작업 단위에 속하는 쿼리 중 하나라도 실패하면 모든 쿼리문을 취소하고 이전 상태로 돌려놓아야한다. 이를 **Rollback** 이라 부른다.

### 트랜잭션의 특징
DB의 트랜잭션이 안전하게 수행되기 위해서는 **ACID** **조건**을 충족해야 한다. 

### ACID
Atomicity(원자성), Consistency(일관성), Isolation(고립성), Durability(지속성)의 약자로서, 데이터베이스의 트랜잭션이 안전하게 수행되기 위한 4가지 필수적인 성질을 말한다.

###  Atomicity(원자성)
트랜잭션이 데이터베이스에 모두 반영되던지, 아니면 전혀 반영되지 않아야 하며 작업이 부분적으로 실행되거나 중단되지 않는 것을 보장하는 것

### Consistency(일관성)
트랜잭션이 완료된 결괏값이 일관적인 DB 상태를 유지하는 것

### Isolation(고립성)
하나의 트랜잭션 수행시 다른 트랜잭션의 작업이 끼어들지 못하도록 보장하는 것

### Durability(지속성)
트랜잭션이 정상적으로 종료된 다음에는 영구적으로 데이터베이스에 작업의 결과가 저장되어야 함

### 트랜잭션 격리 수준 (Isolation level)
특정 트랜잭션이 다른 트랜잭션에 변경한 데이터를 볼 수 있도록 허용할지 말지를 결정하는 것
- **Level 0.** **READ UNCOMMITED(커밋되지 않은 읽기)** : 트랜잭션의 변경내용이 COMMIT이나 ROLLBACK과 상관없이 다른 트랜잭션에서 보여진다.
- **Level 1.** **READ COMMITTED(커밋된 읽기)** : 트랜잭션의 변경 내용이 COMMIT 되어야만 다른 트랜잭션에서 조회할 수 있다.
- **Level 2. REPEATABLE READ(반복 가능한 읽기)** : 트랜잭션이 시작되기 전에 커밋된 내용에 대해서만 조회할 수 있는 격리수준이다.
- **Level 3. SERIALIZABLE(직렬화 가능)** : 트랜잭션에서 읽고 쓰는 내용에 대해서 다른 트랜잭션에서는 절대 접근할 수 없다.
  
### DB Lock
데이터베이스는 데이터를 영속적으로 저장하고 있는 시스템이다. 이런 시스템은 같은 자원(데이터)에 대해서 동시에 접근하는 경우가 생길 수 밖에 없는데, 이럴 경우 데이터가 오염 될 수 있다. 그렇게 되지 않도록 데이터의 일관성과 무결성을 유지해야할 필요가 있다.

예를 들어 수강신청 시스템에서 1명만이 정원으로 남게되었다. 여기서 2사람이 거의 동시에 버튼을 눌렀다. 이런 상황에서 성공은 1명만 되어야 하고, 이런 상황에 사용하는 공통적인 방법이 **Lock**이다.

### 트랜잭션 적용
트랜잭션은 비즈니스 로직이 있는 서비스 계층에서 시작해야 한다. 왜냐하면 **비즈니스 로직이 잘못되어 문제가 생기면 문제가 되는 부분을 롤백**시켜야 하기 때문이다. 따라서 **트랜잭션 시작 -> 로직 수행 -> 트랜잭션 종료** 과정동안 커넥션을 유지시켜야 한다.

**파라미터를 통해 커넥션을 전달하면 같은 커넥션이 사용되도록 유지할 수 있다.**

```java
findById(Connection con, String memberId)
update(Connection con, String memberId, int money)
```

중요한 점은 커넥션을 닫으면 안된다는 것이다. 서비스 로직이 실행되고, 트랜잭션이 종료 되는 시점에 닫아야 한다.
```java
public class Repository {
    ...
    public void update( ... ) {
        // 커넥션 유지가 필요한 메소드
        finally {
        //connection을 닫으면 안된다.
        JdbcUtils.closeStatement(pstmt);
        }
    }
}
```

```java
public class Service {
    public void accountTransfer( ... ) {
        try {
            // 트랜잭션 시작
            bizLogic( ... );
            // 성공시 커밋 
        } catch {
            // 실패시 롤백
        } finally {
            // 비즈니스 로직과 트랜잭션이 끝나는 이 시점에
            // 커넥션을 닫아주어야 한다.
            release(con);
        }
    }
}
```