## Ch6. 다양한 의존관계 주입 방법

### 생성자 주입
생성자를 통해 의존관계를 주입받는 방법. 불편, 필수 의존관계에서 사용. 주로 사용하는 방식이다.  대부분의 의존관계 주입은 한 번 일어나면 종료시점시까지 의존관계를 변경할 일이 없기 때문에 주로 생성자 주입을 사용한다. 
```java
@Component
public class A {

    @Autowired
    public A( ... ) {
        // 생성자 주입
    }
}
```

### 수정자 주입
Setter 라는 수정자 메소드를 통해 의존관계를 주입받는 방법. 선택, 변경 가능성이 있는 의존관계에서 사용
```java
@Component
public class A {

    @Autowired
    public void set~( ... ) {

    }
}
```

### 필드 주입
필드에 바로 의존관계를 주입받는 방법.
```java
@Component
public class A {

    @Autowired
    private Object object;
}
```

### 일반 메소드 주입
일반 메소드를 통해 의존관계를 주입받는 방법.
```java
@Component
public class A {

    @Autowired
    public void init( ... ) {

    }
}
```

### 옵션 처리
`@Autowired`만 사용하게 되면 기본값이 true로 되어있기 때문에 자동 주입 대상이 없으면 오류가 발생한다. 스프링에서는 옵션 기능을 제공하여 오류가 발생하는 것을 막아준다.

* `@Autowired(required = false)` : 자동 주입할 대상이 없으면 수정자 메소드 자체가 호출이 안된다.
* `org.springframework.lang.@Nullable` : 자동 주입할 대상이 없으면 null이 입력된다.
* `Optional<>`: 자동 주입할 대상이 없으면 Optional.empty가 입력된다.

### Lombok
반복되는 getter, setter, toString 등의 반복 메소드 작성 코드를 줄여주는 코드 다이어트 라이브러리이다.
* **`@RequiredArgsConstructor`** : 생성자 자동 생성
* **`@Getter`** : 접근자 자동 생성
* **`@Setter`** : 생성자 자동 생성
* **`@ToString`** : `toString()` 메소드 자동 생성  
* **`@Data`** : `@Getter`, `@Setter`, `@RequiredArgsConstructor`, `@ToString`, `@EqualsAndHashCode` 등을 한번에 설정

**`@Qualifier`** : 스프링 빈에 추가 구분자를 붙여준다. 

**`@Primary`** : 같은 빈이 2개 이상일 경우, 이 어노테이션을 붙여주면 `@Autowired` 매칭 시우선권을 가지게 된다. 