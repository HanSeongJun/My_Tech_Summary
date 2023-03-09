## Ch1. JDBC
### JDBC
자바에서 데이터베이스에 접속할 수 있도록 하는 자바 API. 애플리케이션을 개발할 때 중요한 데이터들을 따로 데이터베이스에 보관하기 위해 사용하는 기술.

### JDBC 드라이버
각 DB마다 커넥션을 연결하는 방법, SQL을 전달하는 방법, 그리고 결과를 응답받는 방법이 각각 다르기 때문에 JDBC는 개발자가 사용하는 DB에 맞는 구현체를 제공한다. 이것을 **JDBC 드라이버**라고 한다.

### 애플리케이션과 DB의 연결 흐름
1. **커넥션 연결** : TCP/IP를 사용해 커넥션을 연결한다.
2. **SQL 전달** : 애플리케이션 서버는 DB가 이해할 수 있는 SQL을 연결된 커넥션을 통해 DB에 전달한다.
3. **결과 응답** : DB는 전달된 SQL을 수행하고 그 결과를 응답한다. 애플리케이션 서버는 응답 결과를 활용한다.

### SQL Mapper
객체와 관계형 데이터베이스의 데이터를 개발자가 작성한 SQL로 매핑시켜주는 프레임워크

### ORM 
객체와 관계형 데이터베이스의 데이터를 자동으로 매핑시켜주는 프레임워크

### DB 연결하기
```java
Connection connection = DriverManager.getConnection(URL, USERNAME, PASSWORD);
```
**`Connection`** : 특정 데이터베이스와의 연결을 의미한다. SQL문이 실행되고, 연결 내에서 SQL응답 결과가 반환된다.

**`DriverManager`** : JDBC 드라이버들을 관리해주는 객체이다. getConnection()을 사용하면 사용하고 있는 DB에 적합한 드라이버를 찾아주고, 드라이버가 제공하는 커넥션을 반환해준다.

### JDBC가 제공하는 클래스와 인터페이스
**`Connection`** : 자바에서 DB를 연결해주는 객체
```java
con = getConnection(); // 커넥션 생성
```

**`Statement`** : DB로 데이터 전송 

**`PreparedStatement`** : Statement에 값을 받을 수 있는 기능 추가
```java
pstmt = con.prepareStatement(sql); // DB에 SQL 데이터를 전달
pstmt.executeUpdate(); // 데이터를 변경할 때 사용
```

**`ResultSet`** : 쿼리의 결과값을 저장
```java
// PrepparedStatementd에 있는 값을 꺼내서 쿼리의 결과값을 저장한다.
rs = pstmt.executeQuery(); // 데이터를 꺼낼 때 사용
```
