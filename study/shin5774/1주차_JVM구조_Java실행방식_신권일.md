# JVM의 구조와 Java의 실행방식을 설명해주세요.

## Java Development Kit(JDK)
- 정의: 자바 개발키트의 약자, 개발자들이 자바로 개발하는데 사용되는 SDK 키트
- 구성: Java Runtime Environment(JRE) + Java Development Tools
    - Java Development Tools
        - javac: 자바 컴파일러, 자바소스를 바이트코드로 컴파일
        - java: 자바 인터프리터: 바이트코드를 해석하고 실행
        - jar: 자바 클래스 파일을 압축한 자바 아카이브 파일(.jar)을 생성하고 관리하는 압축 프로그램
        - javadoc: 자바 소스로부터 HTML 형식의 API 도큐먼트 생성
        - 이외에도 여러가지 존재

## Java Runtime Environment(JRE)
- 정의: 자바 실행 환경의 약자
- 구성: Java Virtula Machine(JVM) + Libraries


## Java Virtual Machine(JVM)
- 정의: 자바 가상 환경의 약자, 자바를 돌리는 프로그램
- 장점: OS의 제약없이 자바 프로그램을 돌릴수 있도록 함.
- 단점: C언어에 비해 속도가 느림
    - 자바 프로그램과 OS 사이에 JVM이 있어 서로 상호작용 하도록 도와줌

- 자바 프로그램의 대략적인 동작 과정 : .java -> [compiler] -> .class(바이트 코드) -> [JVM] -> OS

- 내부 구조
```
program File(.java)
	|
    |←Java Compiler(javac)
    |
Java Byte Code(.class)
    ↓
---JVM--------------------------------
|  Class loader <-> Execution Engine |
|           ↑           ↑            |
|           ↓           ↓            |
|         Runtime Data Areas         |
--------------------------------------
                     ↑
             Native Method Inteface ← Native Method Library
```
- 각 구조 설명
    - Class Loader: 로드된 바이트 코드들을 엮어서 Runtime Data Area에 배치함.
        - 실행 단계
            1. Loading: 클래스 파일을 JVM 메모리에 로드하는 단계
                - 메모리에 Method Area에 로드함.
                - `동적 로딩`를 하기에 static 변수 및 미 사용 클래스는 로드하지 않음.
                - 메서드,변수,클래스,인터페이스,열거형을 구분하여 저장.
                - 완료후 클래스 타입의 객체를 생성하여 Heap 영역에 저장.
            2. Linking: 클래스 파일을 검증하는 단계
                1. Verifying: 클래스가 자바 언어 명세 및 JVM 명세대로 구성되어 있는지 검증함.
                    - 실패시 ` java.lang.VerifyError` 발생.
                2. Preparing: 클래스가 필요로 하는 메모리를 할당함.(데이터 구조를 준비함.)
                    - 메모리 할당 및 static field가 기본값으로 초기화.
                3. Resolving: 클래스의 상수 풀 내 모든 심볼릭 레퍼런스를 다이렉트 레퍼런스로 변경.
                    - 심볼릭 레퍼런스: 메모리 번지가 아닌 이름에 의한 참조
                    - 다이렉트 레퍼런스: 메모리 번지에 의한 참조
            3. Initializing: 확보된 메모리 영역의 클래스의 static 변수를 명시값으로 할당.
        - 종류
            - Bootstrap Class Loader: JVM 시작시 가장 먼저 실행되는 클래스로더
                - 자바 클래스를 로드할수 있는 자바 자체의 클래스로더와 최소한의 자바 클래스를 로드함.
            - Extension Class Loader: 확장 자바 클래스들을 로드함.
                - Bootstrap Class Loader를 부모로 가짐.
            - Application Class Loader: Classpath에 있는 클래스파일 도는 jar에 속한 클래스를 로드함
                - 우리가 만든 .class 확장자 파일을 로드함.
                - System Class Loader라고도 함.
            - 각 클래스 로더의 동작 과정
                1. Method Area에 클래스가 로드 되어있는지 확인
                2. 없다면 시스템 클래스 로더에 클래스로드 요청
                3. 시스템 클래스로더 -> 확장 클래스 로더 -> 부트스트랩 클래스 로더로 위임함.
                4. 위에서부터 차례대로 클래스 존재 확인.
                5. 전부 없다면 `ClassNotFoundException` 발생.
    - Runtime Data Area: 메모리 영역, 자바 애플리케이션을 실행할때 사용되는 데이터들을 적재하는 곳
        - 구성
            1. Method Area: JVM이 시작시 생성되는 공간으로 클래스의 바이트코드가 로드되는 곳
                - Static Area,Class Area로도 불림
                - 클래스/인터페이스의 메타데이터가 저장됨.
                - 저장 목록
                    1. Type Information: 클래스와 인터페이스의 정보
                        - Type명,Type 종류,Type의 제어자, 연관된 인터페이스의 정보
                    2. Field Information: 인스턴스 변수의 정보
                        - Type명,제어자
                    3. Method Information: 메서드의 모든 정보
                        - Method명,Method return Type, method parameter수,각 parameter Type 정보, 필요한 메서드의 정보
                    4. class variable: static 키워드로 선언된 변수
                    5. Runtime Constant Pool:Type의 상수 정보를 저장하는 Pool, 각 상수로는 인덱스를 통해 접근이 가능.
                        - Type,Field,Mehtod로 접근하기위한 Reference 정보
                        - 상수 자료형을 저장하고 참조함.
                        - 각 클래스별로 존재함.
            2. Heap Area: 동적으로 할당되는 영역, Reference Type이 저장되는 공간. GC의 대상 공간.
                - 구성
                    1. Young Generation
                        1. Eden: 새로 생성된 객체가 위치. GC 진행후에는 남은 객체는 survivor로 이동.
                        2. Survivor: 두개가 존재하고 GC 후 남은 객체는 빈 Survivor로 이동.
                    2. Old Generation: 생명주기가 긴 객체를 GC대상으로 하는영역, Young에서 살아남은 객체가 이곳으로 옴.
            3. Stack Area: 기본 자료형을 생성할때 저장하는 공간. 임시로 사용되는 변수나 정보가 저장되는 공간.
                - 메서드 호출마다 스택프레임이 생성되고 메서드 수행이 끝나면 프레임 별로 삭제됨
                    - 스택 프레임: 수행되는 메서드의 데이터가 저장되는 공간
                        - 구성
                            1. Local Variable: 메서드의 지역변수를 가지는 배열
                            2. Operand Stack: 메서드 내 계산을 위한 작업공간
                            3. Frame Data
                                - Constant Pool Resolution: Runtime Constant Pool의 Reference
                                - Instruction Pointer: 자신을 호출한 stack Frame의 포인터

                - 스레드 시작시에 할당되기에 사이즈의 변경이 안되고 초과하게되면 `스택오버플로우`가 발생함.
            4. PC(Program Counter) Register: 현재 수행중인 JVM 명령어 주소를 저장하는 공간
                - Java는 OS/CPU에게는 하나의 프로세스이기에 JVM의 리소스를 이용해 명령을 수행해야함.
                - CPU에 직접 Instruction(연산)을 수행하지 않고 Stack에서 Operand(비연산값)을 뽑아내서 별도의 메모리 공간에 저장
                - 다른 언어의 메서드를 수행하는 과정에서는 Undefined 상태가 됨.
            5. Native Method Stacks:실제 실행 가능한 기계어로 작성된 프로그램 or 네이티브 코드(자바 이외의 언어로 작성된 코드)를 실행하기 위한 공간.
                - Java Native Interface: 자바가 다른언어로 작성된 어플리케이션과 상호작용 할수 있는 인터페이스를 제공하는 프로그램.
                - Native Method Library: C,C++로 작성된 라이브러리
        - 특징
            - Method Area와 Heap Area는 모든 스레드가 공유하지만 나머지는 각 스레드별로 생성해서 관리됨.

    - Execution Engine: Data Area에 배치된 바이트코드를 명령어 단위로 읽어서 실행함.
        - 구성
            1. Intepreter: 바이트코드 명령어를 하나씩 읽어서 해석후 수행.
            2. JIT(Just In Time) Compiler: Interpreter의 속도 문제를 해결하기 위해 고안된 컴파일러
                - 소스코드를 한번에 native machine으로 변환후 다시 사용될때 이전에 변환된 값을 사용함.
                - 평소에는 Interpreter 방식을 사용하다 컴파일 임계치를 넘으면 JIT 컴파일 방식을 수행
                    - 컴파일 임계치: 코드 컴파일을 수행할 기준 (method entry counter + back-edge loop counter)
                        - method entry counter: JVM 내에 있는 메서드가 호출된 횟수
                        - back-edge loop counter: 메서드가 루프를 빠져나오기까지 회전환 횟수
            3. Garbage Collection: Heap 메모리에서 더는 사용하지 않는 메모리를 회수하는 역할.
