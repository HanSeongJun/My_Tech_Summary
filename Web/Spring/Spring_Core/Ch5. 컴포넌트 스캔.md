## Ch5. 컴포넌트 스캔
**`@Compenent`** : 직접 작성한 클래스를 스프링에서 객체로 만들어 관리하는 대상임을 명시하는 어노테이션

**`@CompenentScan`** : `@Component`와 `@Service`, `@Repository`, `@Controller`, `@Configuration` 등이  붙은 클래스 Bean들을 찾아서 스프링 컨테이너에 Bean으로 등록해주는 어노테이션

**`@Autowired`** : 의존관계를 자동으로 주입해주는 어노테이션

### 자동 등록 vs 수동 등록
자동 등록보다는 수동 등록이 우선권이 높다.
