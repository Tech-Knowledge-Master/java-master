# JVM의 구조와 Java의 실행방식
---

## JVM ( Java Virtual Machine ) 개요
- JVM은 자바 플랫폼의 초석이다.

- Java 애플리케이션은 자바 가상 머신 위에서 실행되며,
그렇기에 Java의 동작 방식을 이해하기 위해서는 JVM에 대한 이해가 선행되어야 한다.


## JDK, JRE, JVM

< 개요 >

- JDK는 JRE를 포함하고, JRE는 JVM을 포함한다.

- JDK = JRE + ( javac, javap, javadoc, java, jar )

- JRE = JVM + ( Class Libs / File for supporting execution of program )

- JVM = Runtime Data Area / Interpreter / JIT Compiler / Garbage Collector

< 상세 >
- javac : 자바 언어를 바이트 코드로 컴파일 해주는 자바 컴파일러(javac)

- javap : 자바 클래스 파일을 해석해주는 역 어셈블리어

- JRE는 자바 실행 환경으로
JVM 및 자바 클래스 라이브러리, 기타 자바 어플리케이션 실행에 필요한 파일을 포함한다.

- JVM은 자바 가상 머신으로,
자바 어플리케이션을 실행하는 가상 머신이다.
실제 컴퓨터로부터 Java 어플리케이션 실행을 위한 메모리를 할당받아
Runtime Data Area를 구성한다.

- JVM은 인터프리터 / JIT 컴파일러를 통해 바이트 코드를 각 운영체제에 맞는 기계어로 해석시켜 실행시킨다.
또한, GC를 통해 어플리케이션의 Heap 메모리를 관리한다.

---
## JVM 명세 ( The Java Virtual Machine Specification )

- JVM이 제공해야하는 기능을 묶어놓은 인터페이스와 같음

- 해당 인터페이스를 따르면, 누구나 JVM을 개발하여 제공할 수 있다.

- JVM 종류 : 오라클의 핫스팟 JVM, IBM JVM ..

## JVM 구조

### Runtime Data Area

- 프로그램 실행 중 다양한 런타임 데이터 영역을 사용한다.

- 런타임 데이터 영역은, 모든 스레드들이 공유하는 영역과
스레드 별 할당되는 영역으로 구분된다.

- 런타임 데이터 영역은
Heap / Method / 그 외 영역으로 구분된다.

- JVM을 시작하면, Heap영역과 Method 영역이 생성되며
해당 영역들은 모든 스레드들이 공유한다.

- 각 스레드가 시작될 때마다,
스레드 마다 PC Register, Stack, Native Method Stack이 생성되고
스레드가 종료될 때 사라진다.

- 모든 스레드들이 실행되고 종료되면, JVM이 종료되면서
Heap 영역과 Method 영역도 사라진다.

### PC Register

- PC Register는 자바 가상 머신이 현재 실행중인 명령어의 주소를 저장한다.

### Stack

- Stack은 Frame이라는 자료구조를 저장한다.