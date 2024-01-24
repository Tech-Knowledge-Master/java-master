# GC란 무엇인가
자바애플리케이션에서 사용하지 않는 메모리를 자동으로 수거하는 기능을 말한다. JVM 의 실행엔진에 포함된다.

메모리를 직접 할당하고 해제하는 C, C++ 과는 달리 자바에서는 GC 를 이용하기 때문에 개발자들이 메모리 관리에 신경을 
쓰지않아도 된다.

## Heap
JVM 의 힙 영역은 동적으로 객체가 저장되는 공간으로 가비지 컬렉터의 대상이 되는 공간이다.
1. 대부분의 객체는 금방 접근 불가능한 상태가 된다.
2. 오래된 객체에서 새로운 객체로의 참조는 아주 적게 존재한다.

객체는 대부분 일회성이며, 메모리에 오래 남아있는 경우는 드물다. 이 특성을 활용해 효율적인 메모리 관리를 위해
Heap 영역은 물리적으로 young(eden, survivor0,survivor1) 과 old 영역으로 나뉜다.

### young 영역
* 새롭게 생성된 객체가 할당되는 영역
* 대부분의 객체가 금방 Unreachable 상태가 되기에 많은 객체가 young 영역에 생성되었다 사라진다.
* 이 영역에 대한 가비지 컬렉션을 Minor GC 라고 부른다.

### old 영역
* young영역에서 reachable 상태를 유지하며 살아남은 객체가 복사되는 지역
* young 영역의 수명이 짧은 객체들은 큰 공간을 필요로하지 않으며 큰 객체들은 바로 Old 영역에 할당되기에 young 영역보다 크게 할당되며, 영역의 크기가 큰 만큼 가비지 컬렉션은 적게 발생한다.
* major gc or full gc 라 부른다

## minor GC 동작방식
1. 새로 생성된 객체가 Eden 영역에 할당된다.
2. 객체가 계속 생성되어 Eden 영역이 꽉차게 되고 Minor GC가 실행된다. 
   1. Eden 영역에서 사용되지 않는 객체의 메모리가 해제된다.
   2. Eden 영역에서 살아남은 객체는 1개의 Survivor 영역으로 이동된다.
3. 1~2번의 과정이 반복되다가 Survivor 영역이 가득 차게 되면 Survivor 영역의 살아남은 객체를 다른 Survivor 영역으로 이동시킨다.(1개의 Survivor 영역은 반드시 빈 상태가 된다.)
4. 이러한 과정을 반복하여 계속해서 살아남은 객체는 Old 영역으로 이동(Promotion)된다.

## major GC 동작방식
Major GC는 객체들이 계속 Promotion되어 Old 영역의 메모리가 부족해지면 발생하게 된다.

### 공통적인 알고리즘

> 가비지 컬렉션에서의 루트 스페이스는 Heap 메모리 영역을 참조하는 Method area, Stack, Native method stack 이 된다.

1. Serial GC
    
    GC 를 처리하는 스레드가 1개이다.
    Minor GC 는 mark-sweep 를 사용하고 major GC 는 mark-sweep-compact 를 사용한다.
    cpu 코어가 1개일때 사용하기 위한 가장 단순한 GC 이다.
2. Parallel GC
    
    Serial GC 와 기본적인 알고리즘은 같지만 Young영역의 Minor GC 를 머티스레드로 수행한다.
    Serial GC 에 비해 STW 시간이 짧다
3. Parallel Old GC
    
    Parallel GC 에서 OLD 영역에서도 멀티스레드 GC 로 수행한다.
    Mark-Summary-Compact 방식을 사용한다.
4. CMS GC(Concurrent Mark Sweep)
    
    애플리케이션 스레드와 GC 스레드가 동시에 실행된다.
    GC 대상을 파악하는 과정이 복잡해 다른 GC 대비 CPU 사용량이 높다
5. G1 GC
    
    물리적으로 메모리 공간을 나누지 않고 Region 이라는 개념을 도입해 Heap 을 균등하게 여러개 지역으로 나누고, 각 지역을 역할과 함께
   논리적으로, 동적으로 구분해 객체를 할당한다. 가비지가 가득 찬 영역을 빠르게 회수하여 빈 공간을 확보하므로 GC 의 빈도가 적다.
6. Shenandoah GC
7. ZGC
