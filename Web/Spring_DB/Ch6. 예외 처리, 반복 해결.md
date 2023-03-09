## Ch6. 예외 처리, 반복 해결

### 스프링 예외 추상화
스프링은 데이터 접근 계층에 대한 수십 가지 예외를 정리해서 일관된 예외 계층을 제공한다.  각 예외들은 특정 기술에 종속적이지 않게 설계되어 있기 때문에 서비스 계층에서도 스프링이 제공하는 예외를 사용하면 된다.

* **`DataAccessException`** : 최상위 예외
* **`NoTransient`** : 일시적이지 않다는 뜻이다. 같은 SQL을 그대로 반복해서 실행하면 실패한다. EX) SQL 문법 오류, DB 제약조건 위배 등 
* **`Transient`** : 일시적이라는 뜻이다. 동일한 SQL을 다시 시도했을 때 성공할 가능성이 있다. EX) 락이 풀렸을 경우 등 

### 스프링 예외 변환기
DB에서 발생하는 오류들을 스프링이 정의한 예외들로 자동으로 변환해주는 변환기

```java
SQLExceptionTranslator exTranslator = new
SQLErrorCodeSQLExceptionTranslator(dataSource);
DataAccessException resultEx = exTranslator.translate("select", sql, e);
```
```java
SQLErrorCodeSQLExceptionTranslator exTranslator = new SQLErrorCodeSQLExceptionTranslator(datasource);
// DataIntegrityViolationException
DataAccessException resultEx = exTranslator.translate("select", sql, e);
```

### JDBC 템플릿
**`JdbcTemplate`** : 스프링이 제공하는 JDBC 반복 문제를 해결해주는 템플릿. 아래 코드의 반복을 해결해준다. **트랜잭션을 위한 커넥션 동기화는 물론이고, 예외 발생시 스프링 예외 변환기도 자동으로 실행** 해준다. 
```java
try {
    con = getConnection();
    pstmt = con.prepareStatement(sql);
    pstmt.setString(1, memberId);
    pstmt.executeUpdate();
} catch (SQLException e) {
    throw new MyDbException(e);
} finally {
    close(con, pstmt, null);
}
```
```java
@Override
public Member save(Member member) {
    String sql = "insert into member(member_id, money) values(?, ?)";
    template.update(sql, member.getMemberId(), member.getMoney());

    return member;
}
```

---
## 총 흐름 정리
> **최종 목적 : 서비스 계층에 순수한 서비스 로직만 남기는 것.**

### 1. 애플리케이션에서 데이터베이스를 사용하기 위해 자바가 제공하는 JDBC 기술을 사용
`DriverManager` 를 통해서 DB 커넥션을 얻어왔다. 

### 2. 커넥션을 미리 생성해 저장해두는 커넥션 풀 사용
매 연결마다 커넥션을 얻어오는 것이 비효율적이므로 자바가 제공하는 `DataSource` 를 통해 커넥션 풀에서 커넥션을 꺼내서 사용했다. 커넥션 풀에는 TCP/IP로 DB와 연결되어 있는 커넥션들이 들어있다.

-> 이 과정에서 `JdbcUtils` 을 사용해서 JDBC를 좀 더 편리하게 다루었다.

### 3. 로직을 수행하는 과정 중 문제가 생겨 트랜잭션을 롤백 시켜야 할 필요가 생겼다.
트랜잭션을 롤백 시키려면 "트랜잭션 시작 -> 로직 수행 -> 트랜잭션 종료" 과정동안 커넥션을 유지 시켜야 했다.

-> 파라미터에 커넥션을 전달시켜 커넥션을 위 과정동안 유지시켰다. 

### 4. 트랜잭션 매니저 활용
1~3 과정을 진행하면서 코드에는 트랜잭션 반복 문제, JDBC 코드 반복 문제, SQLException 의존 문제 등 많은 문제점이 생겼다. 

위 문제점들을 해결하기 위해 아래 기능을 가지고 있는
* 트랜잭션 추상화(특정 기술에 의존하지 않도록)
* 리소스 동기화(트랜잭션의 시작부터 끝까지 같은 데이터베이스 커넥션을 유지시켜주는 기능)
  
`PlatformTransactionManager` 라는 트랜잭션 매니저를 사용하였다.

### 5. 커밋, 롤백 등 반복문제가 여전히 남아있었다.
트랜잭션 코드에서 커밋, 롤백 등 트랜잭션 관련 코드들을 제공해주는 `TrasactionTemplate`라는 템플릿를 사용하여 중복 코드를 없앴다.

### 6. 트랜잭션 템플릿에도 의존하게 되었다.

### 7. 스프링 AOP를 통해 프록시를 사용해서 프록시가 대신 트랜잭션 처리 로직을 실행
`@Transactional`을 사용하여 위 모든 과정을 해결하였다. 

### 8. 그러나, SQLException을 아직 의존하는 문제가 있다.
체크 예외 대신 언체크 예외를 사용, 이 언체크 예외를 스프링이 제공하는 예외 변환기를 사용하여 스프링 예외로 변환하여 해결

-> `JdbcTemplate` 을 사용하면 예외 변환기를 자동으로 실행해준다. 

