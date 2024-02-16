## Java 17에서 추가된 기능에 대해서 설명해주세요.

---

### 1. Stream.toList() 기능
Stream을 List로 변환할 때 Collectors에서 기능을 찾아 사용했던 기존 버전에서
Java17 부터는 Collectors 호출 없이 toList()만으로 변환을 지원


### 2. 레코드 클래스 (Record Data class)
immutable 객체를 생성하는 새로운 유형의 클래스로    
기존 toString, equals, hashCode Method에 대한 구현을 자동 제공


### 3. 봉인 클래스 (Sealed Classes)
클래스와 인터페이스의 `상속을 제한`하는 기능    
즉, 어떤 클래스가 다른 클래스에 의해 상속될 수 있는지 개발자가 명시적으로 선언이 가능

1. 도입 목적
   - 개발자가 의도한 대로 명확한 클래스 계층 구조를 만들기 위해
   - 잘못된 상속을 방지하기 위해

2. 이점
   1. 정교하고 명확한 상속 계층의 표현
      - `public`과 `final` 사이의 옵션을 제공하여, 특정 클래스들에게만 상속을 허용하고 다른 클래스들에게는 상속을 금지
   
   2. 타입 체크와 안정성 강화
      - 어떤 클래스가 상속될 수 있는지 정확히 제어함으로써 예상치 못한 상속을 방지하여 런타임 오류의 가능성🔽

   3. 패턴 매칭과의 통합
      - Sealed Classes로 정의된 타입 계층을 통해, 패턴 매칭을 사용할 때 가능한 모든 케이스를 커버하여 코드의 안전성🔼


### 4. 패턴 매칭 (Pattern Matching for switch)
데이터 구조를 검사하고 분해하는 프로그래밍 방식    
조건문이나 반복문에서 복잡한 데이터 구조를 보다 쉽게 처리하기 위한 목적으로 도입

1. 타입 캐스팅 간소화
   - `instanceof`를 사용하여 객체의 타입을 체크한 후, 명시적으로 타입을 캐스팅 해야하는 이전 방식을 한 줄로 간소화

2. switch문 개선
   - switch문의 결과를 변수에 할당할 수 있는 기능(switch expression)의 도입

```
Object obj = ...

String formatted = switch (obj) {
    case Integer i -> String.format("Integer %d", i);
    case Long l -> String.format("Long %d", l);
    case Double d -> String.format("Double %f", d);
    case String s -> String.format("String %s", s);
    default -> obj.toString();
};
```


### 5. 신규 Mac OS 렌더링 파이프라인 (New macOS Rendering Pipeline)
macOS에서 자바 애플리케이션의 GUI 성능을 개선하기 위한 새로운 렌더링 파이프라인

▶▶ GUI 성능을 개선해야 하는 이유?

1. 애플리케이션의 반응 속도 향상
2. CPU와 GPU의 자원 절약 > 배터리 수명 연장
3. 플랫폼 호환과 지속 가능성 보장
