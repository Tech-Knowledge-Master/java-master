## Mutable 객체와 Immutable 객체의 차이점에 대해 설명해주세요.

---

### 1. 두 객체의 차이는 무엇인가요?
1. 가변 객체 (mutable)
   - 객체 생성 후 상태(객체 내의 데이터) 변경이 가능
   - `StringBuilder`, `ArrayList`, `HashMap` 등 요소의 추가,삭제,수정이 용이한 대부분의 컬렉션 클래스
   - 

2. 불변 객체 (immutable)
   - 스레드 안전성
   - Integer, String, Double, Float 등의 `Wrapper 클래스`
   - `LocalDate`, `LocalTime` 등 Java 8에서 도입된 날짜와 시간을 다루는 클래스

``
핵심 차이는 '불변성'과 '변경 가능성'
``


### 2. 각각 어떻게 사용해야 좋은지 알고 있나요?
1. 가변 객체
   - Domain model ▶ 모델은 사용자의 입력이나 애플리케이션의 로직에 따라 상태가 변경될 수 있도록 설계되어야 한다
   - JPA Entities ▶ JPA를 사용해 DB를 다룰 때, db의 레코드가 애플리케이션 로직에 따라 변경될 수 있으므로
   - Form Backing Objects ▶ 사용자 입력 수집에 사용되는 객체로, 사용자 입력에 의해 내부 상태가 변경되기 때문에 mutable

2. 불변 객체
   - 애플리케이션의 설정과 환경 속성 ▶ 애플리케이션 실행 중 변경되지 않아야 한다
   - DTO ▶ 계층 간 데이터 전송에 사용되며, 일단 생성되면 변경되지 않는 데이터를 포함하는 경우가 많기 때문 
   - 멀티스레드 환경에서의 Entities ▶ 특히, 읽기 전용 조회에서는 Immutable 객체를 사용하여 데이터의 안전성을 보장


### 3. 가변과 불변 객체는 어떻게 생성하나요?
1. 불변
   - 일반적으로 클래스를 `final` 선언, 내부 필드를 `private final` 선언
   - 생성자 주입(Constructor Injection)을 사용하여 필요한 의존성을 받고, 변경할 수 없게 한다

2. 가변
   - 세터 주입(Setter Injection) 또는 필드 주입(Field Injection)을 사용
   