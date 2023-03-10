## Ch3. MVC 패턴
### MVC(Model View Controller) 패턴
* **컨트롤러** : HTTP 요청을 받아서 파라미터를 검증하고, 비즈니스 로직을 수행한다. 그리고 뷰에 전달할 결과 데이터를 조회해서 모델에 담는다. 
* **모델** : 뷰에 출력할 데이터를 담아둔다. 뷰가 필요한 데이터를 모두 모델에 담아서 전달해주는 덕분에 뷰는 비즈니스 로직이나 데이터 접근을 몰라도 되고, 화면을 렌더링 하는 일에 집중할 수 있다.
* **뷰** : 모델에 담겨있는 데이터를 사용해서 화면을 그리는 일에 집중한다. HTML 생성을 맡는다.

**`RequestDispatcher`** : 클라이언트로부터 최초로 들어온 요청을 Servlet/JSP 내에서 원하는 자원으로 요청을 넘기는 역할을 수행하거나, 특정 자원에 처리를 요청하고 처리 결과를 얻어오는 기능을 수행하는 클래스

**`dispatcher.forward()`** :  다른 서블릿이나 JSP로 이동할 수 있는 기능