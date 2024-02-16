### Java


<details>
  <summary>JVM의 구조와 Java의 실행방식을 설명해주세요.</summary>
  <p>자바 가상 머신의 약자를 따서 줄여 부르는 용어로 JVM의 역할은 자바 애플리케이션을 클래스 로더를 통해 읽어 자바 API와 함께 실행하는 것입니다. 메모리 관리(GC)을 수행하며 스택기반의 가상머신입니다.</p>
  <p>JVM의 구조는 Class Loader, Execution engine, Runtime Data Area, JNI, Native Method Library로 이루어져 있습니다.</p>
  <ul>
    <li>클래스 로더: JVM내로 클래스를 로드하고, 링크를 통해 배치하는 작업을 수행하는 모듈</li>
    <li>실행 엔진: 바이트 코드를 실행시키는 역할</li>
    <ul>
      <li>인터프리터: 바이트 코드를 한줄 씩 실행합니다.</li>
      <li>JIT 컴파일러: 인터피르터 효율을 높이기 위한 컴파일러로 인터프리터가 반복되는 코드를 발견하면 JIT 컴파일러가 반복되는 코드를 네이티브 코드로 바꿔줍니다. 그 다음부터 인터프리터는 네이티브 코드로 컴파일된 코드를 바로 사용합니다.</li>
      <li>GC(Garbage Collector): 가비지 컬렉터로 힙 영역에서 사용되지 않는 객체들을 제거하는 작업을 의미합니다.</li>
    </ul>
    <li>Runtime Data Areas: 프로그램 실행 중에 사용되는 다양한 영역입니다.</li>
    <ul>
      <li>PC Register: Thread가 시작될 때 생성되며 현재 수행 중인 JVM 명령의 주소를 갖고 있습니다.</li>
      <li>Stack Area: 지역 변수, 파라미터 등이 생성되는 영역. 실제 객체는 Heap에 할당되고 해당 레퍼런스만 Stack에 저장됩니다.</li>
      <li>Heap Area: 동적으로 생성된 오브젝트와 배열이 저장되는 곳으로 GC의 대상 영역입니다.</li>
      <li>Method Area: 클래스 멤버 변수, 메소드 정보, Type 정보, Constant Pool, static, final 변수 등이 생성됩니다. 상수 풀(Constant Pool)은 모든 Symbolic Reference를 포함하고 있습니다.</li>
    </ul>
    <li>JNI(Java Native Interface): 자바 애플리케이션에서 C, C++, 어셈블리어로 작성된 함수를 사용할 수 있는 방법을 제공해줍니다. Native 키워드를 사용하여 메서드를 호출합니다. 대표적인 메서드는 Thread의 currentThread()입니다.</li>
    <li>Native Method Library: C, C++로 작성된 라이브러리 입니다.</li>
  </ul>
  <p>Java의 실행방식
    <ul>
    <li>자바 컴파일러(javac)가 자바 소스코드(.java)를 읽어 자바 바이트코드(.class)로 변환시킵니다.</li>
    <li>Class Loader를 통해 class 파일들을 JVM으로 로딩합니다.</li>
    <li>로딩된 class파일들은 Execution engine을 통해 해석됩니다.</li>
    <li>해석된 바이트코드는 Runtime Data Areas 에 배치되어 실질적인 수행이 이루어집니다.</li>
    </ul>
  </p>
</details>

<details>
  <summary>GC가 무엇인지, 필요한 이유는 무엇인지, 동작방식에 대해 설명해주세요.</summary>
  </br>
  <p>GC는 힙 영역에서 사용하지 않는 객체들을 제거하는 작업을 총칭합니다. 이 객체를 제거하는 작업이 필요한 이유는 자바는 개발자가 메모리를 직접 해제해줄 수 없는 언어이기 때문입니다. 따라서 객체를 사용하고 제거하는 기능이 필요하게 됩니다.</p>
  <p>GC의 동작방식은 가장 간단한 Serial GC 방식으로 설명합니다. 좀 더 진보된 GC는 G1 GC, ZGC가 있으며 여기선 다루지 않습니다.</p>
  <p>GC는 Minor GC, Major GC로 구분할 수 있습니다. Minor GC는 young 영역에서, Major GC는 old 영역에서 일어난다고 정의합니다. (Major GC, Full GC는 명확히 정의된 문서가 없습니다.) GC를 수행할 때는 GC를 수행하는 스레드 이외의 스레드는 모두 정지합니다. 이를 Stop-the-world라고 합니다.</p>
  <p>Minor GC는 Eden 영역이 가득 참에서 부터 시작됩니다. Eden 영역에서 참조가 남아있는 객체를 mark하고 survivor 영역으로 복사합니다. 그리고 Eden 영역을 비웁니다. Survivor 영역도 가득차면 같은 방식으로 다른 Survivor 영역에 복사하고 비웁니다. 이를 반복하다 보면 계속 해서 살아남는 객체는 old 영역으로 이동하게 됩니다.</p>
  <p>Major GC는 old 영역에서 일어납니다. 위와 반대로 삭제되어야 하는 객체를 mark합니다. 그리고 지웁(sweep)니다. 메모리는 단편화 된 상태이므로 이를 한 군데에 모아주는 것을 Compaction이라 하며 compact라고 합니다. 그래서 Mark-Sweep-Compact 알고리즘이라고 합니다.</p>
  <p>이것이 중요한 이유는 GC 수행시 시스템이 멈추기 때문에 의도치 않은 장애의 원인이 될 수 있습니다. 따라서 이를 위해 힙 영역을 조정하는 것을 GC 튜닝이라고 하고 JVM 메모리는 절대 마음대로 조정해선 안됩니다.</p>
</details>

<details>
  <summary>컬렉션 프레임워크에 대해서 설명해주세요.</summary>
  </br>
  <p>Java Collection은 널리 알려져 있는 자료구조를 바탕으로 객체, 데이터들을 효율적으로 관리 할 수 있는 자료구조들이 있는 라이브러리를 컬렉션 프레임워크라고 합니다.</p>
  <p>List, Set은 Collection 인터페이스을 상속받지만, Map 인터페이스는 구조상의 차이라 별도로 정의합니다.</p>
</details>

<details>
  <summary>제네릭에 대해서 설명해주세요.</summary>
  </br>
  <p>제네릭은 자바의 타입 안정성을 맡고 있습니다. 컴파일 과정에서 타입체크를 해주는 기능으로 객체의 타입을 컴파일 시에 체크하기 때문에 객체의 타입 안정성을 높이고 형변환의 번거로움을 줄여줍니다.</p>
</details>

<details>
  <summary>애노테이션에 대해서 설명해주세요.</summary>
  </br>
  <p>애노테이션은 인터페이스를 기반으로 한 문법으로 주석처럼 코드에 달아 클래스에 특별한 의미를 부여하거나 기능을 주입할 수 있습니다. built-in annotation은 상속받아서 메소드를 오버라이드 할 때 나타나는 @Override 애노테이션이 그 대표적인 예입니다.</p>
  <p>메타 애너테이션은 애노테이션을 선언할 때 사용하는 애노테이션입니다.</p>
  <ul>
    <li>@Retention: 애노테이션 유지 범위를 지정합니다. (소스, 클래스, 런타임)</li>
    <li>@Inherit: 애노테이션을 하위 클래스까지 전달여부를 지정합니다. 이 애노테이션이 있으면 하위 클래스까지 상속이 가능합니다.</li>
    <li>@Target: 해당 애노테이션을 어디에 사용할 지 결정합니다. (타입, 필드, 메서드, 파라미터, 생성자, 로컬변수, 애노테이션 타입)</li>
  </ul>
</details>

<details>
  <summary>오버라이딩과 오버로딩이 무엇이며 어떤 차이가 있을까요?</summary>
  </br>
  <p>의외로 굉장히 많은 답을 들을 수 있는 질문입니다.</p>
  <p>오버라이딩은 상위 클래스의 메소드를 재정의 하는 것을 의미합니다. 또, 런타임 다형성이기도 합니다.</p>
  <p>오버로딩은 같은 클래스 내에서 동일한 메소드 이름을 가지지만, 매개변수의 타입, 개수가 다르게 구현할 수 있는 것을 의미하며 컴파일 타임 다형성이기도 합니다. 따라서 오버라이딩 될 수 있습니다.</p>
  <p>추가로 `@Override`를 써야하는 이유를 꼭 생각해보세요. 이 애노테이션은 컴파일 타임에 오버라이딩에 대한 안정성을 부여해주기 때문에 반드시 써주는 것이 좋습니다.</p>
</details>

<details>
  <summary>인터페이스와 추상클래스의 차이점에 대해 설명해주세요.</summary>
  </br>
  <p>추상클래스는 객체의 추상적인 상위 개념으로 공통된 개념을 표현할 때 사용합니다. 단일 상속만 가능합니다. 추상클래스를 상속하는 집합간에는 연관관계가 있습니다.</p>
  <p>인터페이스는 구현 객체가 같은 동작을 한다는 것을 보장하기 위해 사용합니다. 다중 상속이 가능합니다. 인터페이스를 구현하는 집합간에는 관계가 없을 수 있습니다.</p>
</details>

<details>
  <summary>클래스는 무엇이고 객체는 무엇인가요?</summary>
  </br>
  <p>클래스는 객체를 정의하는 틀 또는 설계도와 같은 의미로 사용됩니다.</p>
  <p>객체는 식별 가능한 개체 또는 사물입니다. 객체는 구별 가능한 식별자, 특징적인 행동, 변경 가능한 상태를 가집니다. 인스턴스들을 통칭하는 용도로 사용합니다.</p>
</details>

<details>
  <summary>정적(static)이란 무엇인가요?</summary>
  </br>
  <p>static은 클래스 멤버라고 하며, 클래스 로더가 클래스를 로딩해서 메소드 메모리 영역에 적재할 때 클래스별로 관리됩니다.</p>
  <p>static 키워드를 통해 생성된 정적멤버들은 PermGen 또는 Metaspace에 저장되며 저장된 메모리는 모든 객체가 공유하며 하나의 멤버를 어디서든지 참조할 수 있는 장점이 있습니다.</p>
  <p>그러나, GC의 관리 영역 밖에 존재하기 때문에 프로그램 종료시까지 메모리가 할당된 채로 존재합니다. 너무 남발하게 되면 시스템 성능에 악영향을 줄 수 있습니다.</p>
</details>

<details>
  <summary>자바의 원시타입들은 무엇이 있으며 각각 몇 바이트를 차지하나요?</summary>
  </br>
  <p>실제 면접에서 들었던 질문입니다. 들었을 때 굉장히 당황했던 기억이 나네요.</p>
  <p>boolean(1), char(unsigned 2), byte(1), short(2), int(4), long(8), float(4), double(8)</p>
  <p>사실 JVM에 의존적이기 때문에 정확한 크기라기 보다는 대략적인 크기입니다.</p>
</details>

<details>
  <summary>접근 제어자의 종류와 이에 대해 설명해주세요.</summary>
  </br>
  <p>private, default, protected, public이 있습니다. private은 해당 클래스 내에서만 접근 가능하고, default는 해당 패키지, protected는 상속한 클래스, public은 전체 영역에서 접근 가능합니다.</p>
  <p>접근 제어자를 사용하는 이유는 외부에 보여주고 싶은 정보들을 선택적으로 제공하기 위함이고, 캡슐화와 통하는 면이 있습니다.</p>
</details>

<details>
  <summary>객체지향에 대해서 설명해주세요.</summary>
  </br>
  <p>객체지향을 정의하면, 의존성 관리입니다.</p>
  <p>객체지향으로 의존성을 관리함으로써 변경 영향을 최소화하고 독립적인 배포가 가능해지며 독립적인 개발이 가능해집니다. 따라서 객체지향에서 가장 중요한 것은 DIP(Dependency Inversion Principle)를 통한 고수준 정책(High Level Policy)와 저수준 구현 세부사항(Low Level Details)의 분리라고 할 수 있습니다.</p>
</details>

<details>
  <summary>SOLID(객체지향 5대원칙)에 대해서 설명해주세요.</summary>
  </br>
  <p>SRP(단일책임원칙)은 한 클래스의 하나의 책임만 가져야 합니다.</p>
  <p>OCP(개방-폐쇄 원칙)은 확장에는 열려 있으나 변경에는 닫혀 있어야 하며, 다형성을 활용해야 합니다.</p>
  <p>LSP(리스코프 치환 원칙)은 프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야하는 원칙으로 상위 타입을 상속해서 재정의 했을 때 프로그램이 깨지지 않아야 합니다.</p>
  <p>ISP(인터페이스 분리 원칙)은 클라이언트는 자신이 사용하지 않는 메서드에 의존 관계를 맺으면 안되는 원칙입니다. 특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 더 낫습니다. 즉, 비대한 인터페이스보단 더 작고 구체적인 인터페이스로 분리해야합니다.</p>
  <p>DIP(의존관계 역전 원칙)은 추상적인 것은 자신보다 구체적인 것에 의존하지 않고, 변화하기 쉬운 것에 의존해서는 안된다는 원칙입니다. 구체적으론 구현 클래스에 의존하지 말고, 인터페이스에 의존해야 하는 원칙입니다.</p>
</details>

<details>
  <summary>동일성(identity)와 동등성(equality)에 대해 설명해주세요. (equals(), ==)</summary>
  </br>
  <p>동일성은 객체의 주소를 비교하는 것이고, 동등성은 객체의 같음을 비교하는 것입니다.</p>
  <p>기본적으로 자바에서는 Object 클래스에 정의된 equals() 메소드가 동일성 비교를 합니다. 따라서, 개발자는 원한다면 equals() 메소드를 오버라이딩해서 동등성의 판단 기준을 정의해주면 됩니다.</p>
</details>

<details>
  <summary>원시타입과 참조타입의 차이에 대해 설명해주세요.</summary>
  </br>
  <p>원시타입은 Java에서 단 8개 밖에 존재하지 않는 타입입니다. 나머지는 모두 참조타입이라고 볼 수 있고, Object 클래스이거나 이를 상속하는 클래스들로 이루어져 있습니다.</p>
  <p>원시타입은 항상 값이 존재해야 합니다. 반면, Object 타입은 null 포인터를 가질 수 있습니다. 그리고 멤버변수가 초기화될 때, 원시타입은 기본값을 가지지만, 참조타입은 null 포인터를 가지는 차이도 있습니다.</p>
</details>

<details>
  <summary>String, StringBuilder, StringBuffer 각각의 차이에 대해 설명해주세요.</summary>
  </br>
  <p>String은 불변입니다. StringBuilder와 StringBuffer는 이런 String의 특징때문에 사용하는 가변타입이라고 볼 수 있습니다.</p>
  <p>StringBuilder와 StringBuffer는 Thread-safe 여부의 차이가 있습니다. StringBuilder는 Thread-safe하지 않습니다. 따라서 Multi-Thread 환경에서 사용할 때는 StringBuffer를 사용합니다.</p>
</details>

<details>
  <summary>Checked Exception과 Unchecked Exception에 대해 설명해주세요. 스프링 트랜잭션 추상화에서 rollback 대상은 무엇일까요?</summary>
  </br>
  <p>둘의 차이는 RuntimeException을 상속하는가의 여부에 따라 다릅니다. RuntimeException을 상속하면 UncheckedException이 됩니다. 스프링 트랜잭션 추상화에서 rollback 대상은 바로 UncheckedException입니다.</p>
  <p>이 둘을 잘 알기 위해서는 토비의 스프링을 보시는 것을 추천합니다.</p>
</details>

<details>
  <summary>Java8에서 추가된 기능에 대해서 설명해주세요.</summary>
  </br>
  <p>자신이 사용한 경험을 말해주면 더 효과적일 것 같습니다.</p>
  <p>Java8에서는 Lambda식, Stream API, Optional, 날짜 시간 API, StringJoiner 등이 추가되었습니다.</p>
  <p>lambda는 함수형 프로그래밍을 지원하기 위한 기능이고, Stream API는 고차함수를 지원합니다. Optional은 Null-safety를 제공하며, Stream과 사용법이 유사합니다. 날짜 시간 API는 Joda-time등의 라이브러리에서 영향을 받아 괜찮은 API가 되었으며, StringJoiner는 문자열을 간단하게 구분자로 합칠 수 있는 기능을 제공합니다.</p>
</details>

<details>
  <summary>try-with-resource에 대해서 설명해주세요.</summary>
  </br>
  <p>try-with-resources는 자바 버전7에 도입된 문법입니다.</p>
  <p>자바 7 버전 이전에서 하나 이상의 리소스(java.lang.AutoCloseable을 구현한 객체 혹은 java.io.Closeable를 구현한 객체)를 사용할 경우 개발자가 임의로 finally 문에서 ~~.close()를 사용하여 자원 해제를 시켜줘야 했습니다.</p>
  <p>만약 개발자가 사용한 자원을 finally 문에서 해제시켜주지 않고 누락시켰다면 자원이 해제되지 않은 채로 프로그램이 오작동하게 되고, finally 문에서 자원을 해제 시켜주더라도 자원 해제를 위한 중복 코드가 발생하기 때문에 소스 코드의 가독성을 해치는 단점이 있었습니다.</p>
  <p>이를 해결하기 위해 try() 안에 사용할 리소스 객체를 명시적으로 선언하여 사용하면, try 블록 안에서 로직이 정상적으로 완료되었는지, 갑작스럽게 완료되었는지 여부와 관계 없이 JVM에서 자동으로 자원을 반납해주는 기능을 하도록 도입하였습니다.</p>
  <p>추가로, 자바 9 버전에서는 try() 문 안에 명시적으로 객체 선언을 하기 보다는 try 문 바깥에서 객체 선언을 하고 생성된 인스턴스의 변수를 넣어줄 수 있도록 바뀌었습니다.</p>
  <p>
  Java 7 : try(BufferedReader br = new BufferedReader()) <br>
  Java 9 : try(br)
  </p>
</details>

<details>
  <summary>강한 결합과 느슨한 결합이 무엇인지 설명해주세요.</summary>
  </br>
  <p>결합도는 의존성의 정도를 나타내며 다른 모듈에 대해 얼마나 많은 정보를 알고 있는지에 대한 척도입니다.</p>
  <p>어떤 모듈이 다른 모듈에 너무 자세한 부분(구현 세부사항)까지 알고 있을 경우에 강한 결합도를 가진다고 합니다.</p>
  <p>어떤 모듈이 다른 모듈에 대해 필요한 정보(인터페이스로 추상화된 고수준 정책)만 알고 있다면 두 모듈은 낮은 결합도를 가진다고 합니다.</p>
  <p>객체지향 관점에서 결합도는 객체 또는 클래스가 협력에 필요한 적절한 수준의 관계만을 유지하고 있는지를 나타냅니다. 이러한 관점에서 강한 결합도는 반드시 지양해야 하며, 개발자는 적절한 결합도를 유지할 수 있도록 고민하고 설계해야 합니다.</p>
</details>

<details>
  <summary>직렬화와 역직렬화에 대해서 설명해주세요.</summary>
  </br>
  <p>직렬화란 자바 시스템 내부에서 사용되는 객체 또는 데이터를 외부의 자바 시스템에서도 사용할 수 있도록 바이트 형태로 데이터 변환하는 기술과 바이트로 변환된 데이터를 다시 변환하는 기술(역직렬화)을 아울러서 이야기 합니다.</p>
  <p>자바 직렬화는 JVM의 메모리에서만 상주되어있는 객체 데이터를 영속화(Persistence)가 필요할 때 사용됩니다. 시스템이 종료되더라도 없어지지 않는 장점을 가지며 영속화된 데이터이기 때문에 네트워크로 전송이 가능합니다.</p>
</details>

<details>
  <summary>자바의 동시성 이슈(공유자원 접근)에 대해 설명해주세요.</summary>
  </br>
  <p></p>
</details>

<details>
  <summary>Mutable 객체와 Immutable 객체의 차이점에 대해 설명해주세요.</summary>
  </br>
  <p>Mutable 객체는 변경 가능 객체이고, Immutable 객체는 불변 객체라고 흔히들 말합니다.</p>
  <p>Mutable 객체는 도메인 개체(도메인 클래스 혹은 엔터티)로 사용됩니다. Mutable 객체의 변경 메서드는 Command method라고도 부르며, 리턴 타입을 void 로 정의합니다. 또한 void 리턴 타입의 어떠한 상태를 변경하는 메서드는 모두 Command method의 상징입니다.</p>
  <p>
  Immutable 객체는 불변객체이며 값 객체, 서비스 객체 등에 사용됩니다. Immutable 객체의 변경 메서드는 변경한 객체의 복사본을 반환해야 합니다.
  </p>
</details>

<details>
  <summary>자바에서 null을 안전하게 다루는 방법에 대해 설명해주세요.</summary>
  </br>
  <p>공개 메서드가 아닌 곳에는 assert를 사용하여 null을 방어할 수 있습니다. 또한 메서드의 인자를 받을 때 Objects.requireNonNull()을 사용하여 방어할 수 있습니다. 그리고 Optional을 사용해 리턴 타입에서 null을 반환하지 않도록 방어할 수 있습니다. 마지막으로 사전 조건과 사후 조건을 명확히 하여 계약에 의한 설계를 실천해야 합니다.</p>
</details>


<details>
  <summary>JDK와 JRE의 차이점을 설명하세요.</summary>
  </br>
  JDK는 Java Development KIT의 약자로 개발하는데 사용되는 도구이며 JRE를 포함하고 있으며 <br/>
  JRE는 Java Runtime Environment의 약자로 자바로 만들어진 프로그램을 실행시키는데 필요한 도구가 <br/> 
  들어있는 차이가 있습니다.<br/>

운영서버와 같은 곳에서는 개발에 필요한 도구가 아닌 프로그램을 실행시키는 도구만 필요하기 때문에<br/>
개발도구가 들어있는 JDK아닌 JRE를 설치합니다.<br/>

----여기는 굳이 말씀 안하셔도 될듯합니다.----<br/>
그러나 최근에 JDK가 많이 가벼워지고 하드웨어도 좋아지고 해서 운영서버에 설치하여도 큰 문제가 없고<br/>
JDK에 로깅, 디버깅, 로그분석등 유용한 도구들도 있고 해서<br/>
굳이 JRE설치하는것 보다 JDK를 설치 해서 개발및 실행환경을 통합적으로 관리하는 경우도 있다고 합니다.<br/>
  </details>

추가) 
Java의 Enum에 대해 설명해주세요.
Generic에 대해 이전에 비해 어떤 이점이 있는지 설명
JVM의 heap 영역 중 young영역에서 survivor부분이 survivor1과 survivor2로 나뉘어져있는 이유?
JNI의 메모리상에서 변환되어서 실행되는 과정
final에 대해 설명(용도 3가지, 왜 쓰는지)

< 싱글톤 패턴 >
싱글톤이 무엇이고 왜 쓰는지?
싱글톤을 쓰면 멀티스레드 환경에서 스레드세이프 한지?
멀티스레드 환경과 단일스레드 환경에서 싱글톤 차이
싱글톤 외에 스레드세이프하게 구현할 수 있는 디자인 패턴
