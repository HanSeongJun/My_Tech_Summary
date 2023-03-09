## Ch4. 스프링과 문제 해결 - 트랜잭션
### 애플리케이션의 구조
대부분의 애플리케이션은 **프레젠테이션 계층(UI 처리, 웹 요청과 응답 등), 서비스 계층(비즈니스 로직), 데이터 접근 계층(실제 DB 접속 코드)** 등 3가지 계층으로 나누어져 있다.

**변경을 최소화 하기 위해 서비스 계층은 특정 기술에 의존하지 않는 것이 중요**하다.

ex) 트랜잭션 반복 문제, JDBC 코드 반복 문제, SQLException 의존 문제 등... 

또한, 특정 기술에 의존하게 되면 향후 JDBC -> JPA 등으로 기술을 변경할 때 서비스 계층의 코드를 모두 변경해야 한다는 단점이 있다.

### 트랜잭션 추상화
서비스 계층에 순수 비즈니스 로직만 남기기 위해 스프링에서는 트랜잭션 추상화라는 기술을 제공한다. 

**`PlatformTransactionManager`** : **스프링이 제공하는 추상화 인터페이스**, 앞으로 **트랜잭션 매니저**라고 줄여 부르겠다.

### 트랜잭션 매니저
트랜잭션 매니저는 다음 2가지 기능을 제공한다.
* 트랜잭션 추상화
* 리소스 동기화

### 리소스 동기화
트랜잭션의 시작부터 끝까지 같은 데이터베이스 커넥션을 유지시켜주는 기능. 이 기능을 **같은 커넥션을 동기화(맞추어 사용)** 한다고 한다.

**`DataSourceUtils`** : **스프링에서 리소스 동기화를 위한 메소드를 지원하는 클래스**. 이 클래스가 트랜잭션 동기화를 직접 지원하는 것이 아니다. 

`DataSourceUtils.getConnection(DataSource dataSource)` : 트랜잭션 동기화 매니저가 관리하는 커넥션(DataSoruce)에서 커넥션을 얻는다.

`DataSourceUtils.releaseConnection(Connection con, DataSource dataSource)` : 트랜잭션을 사용하기 위해 동기화된 커넥션(DataSource)을 닫지 않고 그대로 유지시켜준다.

### 트랜잭션 템플릿
**`TransactionTemplate`** : 트랜잭션 코드에서 커밋, 롤백 등 트랜잭션 관련 코드들을 제공해주는 클래스, 사용하려면 `PlatformTransactionManager` 을 주입받아야 한다. 
```java
public class TransactionTemplate {
    private PlatformTransactionManager transactionManager;
    
    // 응답 값이 있을 때 사용
    public <T> T execute(TransactionCallback<T> action){..}
    // 응답 값이 없을 때 사용
    void executeWithoutResult(Consumer<TransactionStatus> action){..}
}
```

```java
public void accountTransfer(String fromId, String toId, int money) throws SQLException {
    // 트랜잭션 시작 후
    txTemplate.executeWithoutResult((status) -> {
        try { // 비즈니스 로직 시작
            bizLogic(fromId, toId, money);
        } catch (SQLException e) {
            throw new IllegalStateException(e);
        }
   });
}
```

### 트랜잭션 AOP
스프링 AOP를 통해 프록시를 사용해서 **프록시가 대신 트랜잭션 처리 로직을 실행**시켜 줄 수 있다. 트랜잭션이 필요한 곳에 **`@Transactional`** 어노테이션만 붙이면 된다.
```java
@Transactional
public void accountTransfer(String fromId, String toId, int money) throws SQLException {
    bizLogic(fromId, toId, money);
}
```






