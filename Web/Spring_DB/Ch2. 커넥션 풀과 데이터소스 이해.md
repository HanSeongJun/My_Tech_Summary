## Ch2. 커넥션풀과 데이터소스 이해
### 커넥션 풀
**커넥션을 미리 생성해두고 사용할 수 있게 하는 방법.** 커넥션 풀에 들어 있는 커넥션은 TCP/IP로 DB와 커넥션이 연결되어 있는 상태이기 때문에 언제든지 즉시 SQL을 DB에 전달할 수 있다.

### DataSource
**커넥션 풀에서 커넥션을 획득하는 방법을 추상화 하는 인터페이스.** 핵심 기능은 커넥션 조회 하나이다. 
```java
public interface DataSource {
    Connection getConnection() throws SQLException;
}

// 한번만 설정 정보를 입력해 놓으면,  
DriverManagerDataSource dataSource = new DriverManagerDataSource(URL, USERNAME, PASSWORD);

// 커넥션 풀에 있는 커넥션을 편리하게 얻어올 수 있다.
Connection con = dataSource.getConnection();
```

### JdbcUtils
스프링이 제공하는 JDBC를 편리하게 다룰 수 있는 편의 메소드
```java
JdbcUtils.closeResultSet(rs);
JdbcUtils.closeStatement(stmt);
JdbcUtils.closeConnection(con);
```