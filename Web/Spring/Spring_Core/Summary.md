## 요약
### 강의에서 사용한 어노테이션 
**`@Configuration`** : 클래스가 환경 설정과 관련된 파일이라는 것을 알려준다.

**`@Bean`** : 개발자가 직접 제어가 불가능한 객체들을 스프링이 관리하는 객체(빈)으로 등록하기 위해 사용한다.

**`@Component`** : 해당 클래스가 스프링에서 빈으로 만들어 관리하는 대상임을 나타낸다.

**`@ComponentScan`** : `@Component`와 `@Service`, `@Repository`, `@Controller`, `@Configuration`이 붙은 클래스 빈들을 찾아서 스프링 컨테이너에 빈으로 등록해주는 어노테이션

**`@Autowired`** : 생성자나 Setter와 같은 메소드 없이 의존성을 주입해서 자동으로 객체를 생성해주는 어노테이션

**`@Qualifier`** : 빈에 추가 구분자를 붙여준다.

**`@Primary`** : 빈에게 우선권을 준다.

**`@PostConstruct`** : 체가 생성된 후 별도의 초기화 작업을 위해 실행하는 메소드 위에 선언

**`@PreDestory`** : 스프링 컨테이너에서 객체(빈)를 제거하기 전에 해야할 작업을 하는 메소드 위에  선언

**`@Scope`** : 빈이 존재할 수 있는 범위 설정



### 강의에서 사용한 클래스, 인터페이스와 메소드
**`ApplicationContext`** : 스프링 컨테이너 인터페이스

**`AnnotationConfigApplicationContext`** : `ApplicationContext`의 구현체

**`Assertions`** : 프로그램에 대한 가정을 테스트 할 수 있게 도와주는 클래스

**`BeanDefinition`** : 스프링 빈에 대한 메타정보가 들어있는 인터페이스

**`getBeanDefinitionNames()`** : 스프링에 등록된 모든 빈 이름을 조회

**`getBean()`** : 빈 이름, 타입(=객체의 주소값)으로 빈 객체(인스턴스)를 조회

**`getBeansOfType()`** : 해당 타입의 모든 빈을 조회, 반환형은 Map

**`Provider`** : DL을 제공하는 인터페이스
