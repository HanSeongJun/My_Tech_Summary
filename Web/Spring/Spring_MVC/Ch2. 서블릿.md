## Ch2. 서블릿
**`@ServletComponentScan`** : 스프링에서 지원하는 서블릿 자동 등록 어노테이션

**`@WebServlet`** : 속성 값을 통해 서블릿(name)과 매핑될 URL(urlPatterns)을 지정

**`response.getWriter()`** : writer 객체를 얻고 해당 객체를 통해 write나 print를 통해 response body message를 생성하여 응답

### HTTPServletRequest
서블릿은 개발자가 HTTP 요청 메시지를 편리하게 사용할 수 있도록 개발자 대신에 HTTP 요청 메시지를 파싱한다. 그리고 그 결과를 HttpServletRequest 객체에 담아서 제공한다.
* 파싱 : 웹페이지에서 원하는 데이터를 추출하여 가공하기 쉬운 상태로 바꾸는 것

또한 임시 저장소 기능과
* 저장 : `request.setAttribute(name, value)`
* 조회 : `request.getAttribute(name)`

세션 관리 기능을 제공한다.
* `request.getSession(create: true)`

### HTTP 요청 데이터 전송 방식
1. GET - 쿼리 파라미터
2. POST - HTML Form
3. HTTP 메시지 마디에 데이터를 직접 담아서 요청

### 요청 데이터로 넘어온 파라미터를 조회하는 방법
```java
String username = request.getParameter("username"); //단일 파라미터 조회 
Enumeration<String> parameterNames = request.getParameterNames(); //파라미터 이름들 모두 조회
Map<String, String[]> parameterMap = request.getParameterMap(); //파라미터를 Map 으로 조회
String[] usernames = request.getParameterValues("username"); //복수 파라미터 조회
```

**`ServletInputStream`** : POST 형식으로 전송된 데이터의 메시지 바디를 읽을 수 있는 자바에서 제공하는 인터페이스

**`StreamUtils`** : 스프링 프레임워크 내에 내장되어 있는 Input Stream, Output Stream을 다루기 위한 클래스이다. 
* 데이터를 입력 받을 때 - InputStream
* 데이터를 출력 할 때 - OutputStream

**`StreamUtils.copyToString()`** : InputStream을 문자열로 반환해준다.

**`ObjectMapper`** : JSON 변환 라이브러리

### HttpServletResponse
HTTP 응답 메시지 생성, 헤더 생성, 바디 생성을 해주는 클래스

**`@AfterEach`** : 테스트 메소드 실행 후에 무조건 실행, 주로 테스트 데이터 초기화에 사용

**`@BeforeEach`** : 테스트 메서드 실행 전에 무조건 실행, 주로 테스트 실행 전 필요한 작업들에 사용