# ✎ Interface
자식 클래스가 여러 부모 클래스를 상속받을 수 있지만 메서드 출저의 모호성 등 많은 오류가 존재할 수 있다.

따라서 자바에서는 인터페이스를 도입하여 다중 상속을 지원하고 있다.

인터페이스(interface)란 다른 클래스를 작성할 때 기본이 되는 틀을 제공하면서, 다른 클래스 사이의 중간 매개 역할까지 담당하는 일종의 추상 클래스를 의미한다.

따라서 인터페이스는 인스턴스를 생성할 수 없다.(구현 코드가 존재하지 않기 때문에)

## Interface의 장점
1. 대규모 프로젝트 개발 시 일관되고 정형화된 개발을 위한 표준화가 가능하다.
2. 클래스의 작성과 인터페이스의 구현을 동시에 진행할 수 있으므로, 개발 시간을 단축할 수 있다.
3. 클래스와 클래스 간의 관계를 인터페이스로 연결하면, 클래스마다 독립적인 프로그래밍이 가능하다.


# ✎ Abstract Class
자바에서는 하나 이상의 추상 메소드를 포함하는 클래스를 가리켜 추상 클래스라고 한다.

따라서 추상클래스는 구현체가 존재하지 않으므로 당연히(?) 인스턴스를 생성할 수 없다.
하지만 자식 클래스가 구현 코드를 작성하면, 자식 클래스는 인스턴스를 생성할 수 있게 되는 것이다.

![Interface and Abstract Class.png](image%2FinterfaceAndAbstractClass%2FInterface%20and%20Abstract%20Class.png)

## Abstract Method
추상 메소드란 자식 클래스에서 반드시 오버라이딩해야만 사용할 수 있는 메소드를 의미한다.

자바에서 추상 메소드를 선언하여 사용하는 목적은 추상 메소드가 포함된 클래스를 상속받는 자식 클래스가 반드시 추상 메소드를 구현하도록 하기 위함이다.

따라서 추상 메소드는 선언부만 존재하며, 구현부는 자식 클래스에서 오버라이딩을 해서 구현한다.

### Abstract Method의 사용 목적
자바에서 추상 메소드를 선언하여 사용하는 목적은 추상 메소드가 포함된 클래스를 상속받는 자식 클래스가 반드시 추상 메소드를 구현하도록 하기 위함이다.

만약 일반 메소드로 구현한다면 사용자에 따라 해당 메소드를 구현할 수도 있고, 안 할 수도 있다.

하지만 추상 메소드가 포함된 추상 클래스를 상속받은 모든 자식 클래스는 추상 메소드를 구현해야만 인스턴스를 생성할 수 있으므로, 반드시 구현하게 된다.

# ✎ Interface와 Abstract Class의 차이점
추상 클래스는 is kind of 관계로 관련성이 높은 명확한 계층 구조의 추상화에 사용되며, 인터페이스는 is able to 관계로 관련성이 낮더라도 기능에 따른 추상화가 필요하거나 다중 상속(구현)이 필요할 때 사용된다.

|         | 추상클래스	           | 인터페이스                              |
|---------|------------------|------------------------------------|
| 다중상속    | 	불가능             | 가능                                 |
| 추상 메서드	 | 0개 이상	           | 전부                                 |
| 일반 메서드	 | 가능               | 불가능. 다만 Java8부터는 디폴트, 정적 메서드 구현 가능 |
| 필드	     | 일반 변수, 상수 모두 가능	 | 상수(static final)만 가능               |
| 상속 키워드	 | extends          | implements                         |
| 접근 제어자	 | 제한 없음	           | public                             |

---
### 참고 자료
- https://tcpschool.com/java/java_polymorphism_interface
- https://tcpschool.com/java/java_polymorphism_abstract
- https://code-lab1.tistory.com/287#google_vignette