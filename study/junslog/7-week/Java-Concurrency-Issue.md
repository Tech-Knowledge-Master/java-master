# [ Java Study -  자바의 동시성 이슈 ]

### Thread - Safe

- 멀티 스레드 프로그래밍과 같은 ‘공유 자원’에 대해 동시 접근하는 상황에서
스레드 간의 실행 순서에 따라 공유 자원의 값이 달라질 수 있는 Race Condition이 발생하는데, 이에 대한 문제 해결이 필요하다.
- 이렇게 여러 테스크가 동시에 처리되도록 구현하는 것을 동시성 프로그래밍,
데이터 충돌과 같은 데이터 무결성 이슈를 해결하고, 충돌을 피하는 방법을 동시성 보장이라 한다.
- Java 에서는 관련 키워드로 `**volatile**`, `**synchronized**`, `**atomic**`
이렇게 세가지 키워드를 정리해볼 수 있다.

### 컴퓨터 구조 - CPU / CPU Cache Memory / RAM의 관계

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/53670f0d-7c0e-4c81-8f66-7aec59a42d5a/b1059a1f-faae-4b4d-91a6-50873c6d9750/Untitled.png)

동시성 프로그래밍에서는

- **CPU와 Memory 사이에 존재하는 CPU Cache Memory**
- **멀티 코어 환경의 병렬성**

이 두가지 특징 때문에

다수의 스레드가 공유 자원에 접근 시 **Critical Section Problem**이 생길 수 있다.

- 메모리 가시성 문제
- 동기화 문제 ( 공유 자원에 대한 연산의 수행 순서와 원자성을 보장해줘야하는 이슈 )

가 생길 수 있다. 

### 가시성 문제

- 여러 개의 스레드가 사용됨에 따라,
CPU Cache Memory와 RAM의 데이터가 서로 일치하지 않아 생기는 문제이다.
- 한 스레드가 변경된 값을 cache memory에서 ram에 데이터를 저장하기 전, 
다른 스레드에서 Ram에서 해당 값을 읽어 변경되기 이전의 값을 처리하게 되는 상황을 
가시성이 보장되지 않는다 라고 말한다.
- 공유 자원을 변경한 스레드가 언제 CPU Cache에서 RAM으로 쓰기 연산을 할 지 보장이 안되고,
또 해당 스레드가 RAM에 그 값을 쓰기 연산 했다 해서, 다른 스레드가 이미 이전에 가져온 공유 자원의 값을 CPU Cache Memory에서 참조하고, 언제 다시 RAM에서 가져올지 보장이 안되기에
어느 시점 까지 or 계속 잘못된 값을 가지고 연산을 할 수 있다. 이것이 메모리 가시성 문제이다.
- volatile 키워드를 통해 가시성이 보장되어야 하는
어떤 변수를 Cache가 아닌 Memory로부터 직접 읽고 쓰게 함으로써, 
가시성을 보장할 수 있다.
- 여러 스레드가 공유 자원에서 읽기 연산만을 할 때는 동시성을 보장할 수 있다.
- 여러 스레드가 공유 자원에 쓰기 연산을 할 경우, 가시성을 보장했다 해서 동시성이 보장되지는 않는다.

### 원자성 문제

- High-Level Instruction 과 이것이 컴파일러에 의해 기계어로 변경되어 Machine Instruction 수준으로 내려갈 때, 이것이 1:1 매핑이 되지 않기에 발생하는 문제
- Context-Switching에 의해 공유 데이터에 대한 동시 접근 이슈가 생길 수 있다. ( Race Condition )
- `**synchronized**` 또는 `**atomic**` 키워드를 사용하여 해결할 수 있다.

### Synchronized

- 해당 블럭으로 감싸진 객체 ( 공유 자원 )에 하나의 스레드만 들어가는 것을 보장한다.
**( Monitor Lock 이용 )**
- synchronized 블럭을 들어가기 전에,
CPU Cache Memory와 Main Memory를 동기화 해주는 과정이 있다.
- 즉, synchronized 블럭을 사용하면 **동시성 문제**를 비롯한
메모리 **가시성 문제**도 해결할 수 있다.

### Atomic

- 어떤 변수에 대한 연산의 원자성을 보장해준다.
- CAS ( Compare And Swap ) 알고리즘으로, CPU Cache Memory에서 잘못된 값을 참조하는 문제를 해결해준다.
- volatile 키워드로 메모리 가시성을 보장하고, CAS 알고리즘을 통해 기존에 가져왔던 공유자원의 값과 현재 스레드가 들고왔던 공유자원이 값이 다른 경우 연산을 Rollback 함으로써 연산의 원자성을 보장한다.
- volatile + CAS 의 조합을 통해, synchronized의 단점인 Lock에 의한 병렬성을 저해한다는 요소를 보완하여 연산의 원자성을 보장할 수 있다.
- Java의 Concurrent Package의 클래스들은 CAS 알고리즘을 통해 원자성을 보장한다.

[https://ecsimsw.tistory.com/entry/자바의-동기화-방식-메모리-가시성이란-synchronized-volatile-atomic](https://ecsimsw.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%9D%98-%EB%8F%99%EA%B8%B0%ED%99%94-%EB%B0%A9%EC%8B%9D-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EA%B0%80%EC%8B%9C%EC%84%B1%EC%9D%B4%EB%9E%80-synchronized-volatile-atomic)

https://badcandy.github.io/2019/01/14/concurrency-02/
