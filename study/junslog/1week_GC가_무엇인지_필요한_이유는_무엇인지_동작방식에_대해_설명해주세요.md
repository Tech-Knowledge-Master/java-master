# GC란 무엇인가
---

GC(Garbage Collection)은 자바 애플리케이션에서 사용하지 않는 메모리를
자동으로 수거하는 기능을 말한다.

GC를 이해하기 위해서는 자바의 메모리 구조를 이해해야 한다.
일반적으로, 애플리케이션에서 사용되는 객체는 오래 유지되는 객체보다 잠시 사용되는 경우가 많다.

자바에서는 크게 두 영역으로 메모리를 구분함.
- Young 영역과 Old 영역
- Young 영역은 생성된지 얼마되지 않은 객체들을 저장하는 장소
- Old 영역은 생성된지 오래된 객체를 저장하는 장소

[ New / Young 영역 ]
- 이 영역은 자바 객체가 생성되자마자 저장되고,
생긴지 얼마 안된 객체가 저장되는 곳이다.

- 자바 객체가 생성되면 이 영역에서 저장되다가
시간이 지남에 따라 우선 순위가 낮아지면 Old 영역으로 옮겨진다.

[ Old 영역 ]
- New/Young 영역에서 저장되었던 객체 중에
오래된 객체가 이동되어서 저장되는 영역이다.

[ Perm 영역 ]
- Class, Method 등의 코드가 저장되는 영역으로,
JVM에 의해서 사용된다.

---

## JVM이 메모리를 관리하는 방식

### Minor GC

New/Young 영역을 Minor GC라고 부른다.
New/Young 영역은 Eden, Survivor 영역으로 나뉜다.

Eden 영역은 자바 객체가 생성되자마자 저장되는 곳이다.
이렇게 생성된 객체는 Minor GC가 발생할 때 Survivor 영역으로 이동한다.

Survivor 영역은 Survivor1, Survivor2 영역으로 나뉘는다,
Minor GC가 발생하면 Eden과 Survivor1에 활성 객체를 Survivor2로 복사한다.

활성이 아닌 객체는 자연스럽게 Survivor1에 남아있게 되고,
Survivor1과 Eden 영역을 클리어 한다.
( 결과적으로 활성 객체만 Survivor2로 이동 )

다음번 Minor GC가 발생하면, 같은 원리로 Eden과 Survivor2 영역에서 활성 객체를
Survivor1으로 이동시키게 된다. 계속 이런 방식을 반복하면서 Minor GC를 수행한다.

Minor GC를 수행하다가, Survivor 영역에서 오래된 객체를 Old 영역으로 옮기게 된다.

이러한 방식의 GC 알고리즘을 Copy & Scavenge ( Scavenge : 찾아다니다 )라고 한다.
이 방식은 속도가 매우 빠르고, 작은 크기의 메모리를 Collecting 하는데 매우 효과적이다.

Minor GC의 경우 자주 일어나기에, GC에 걸리는 시간이 짧은 알고리즘이 적합하다.

---
### Major GC ( Old Generation GC, Full GC )

- Old 영역의 가비지 컬렉션을 Full GC라 부른다.
- Full GC에 사용되는 알고리즘을 Mark & Compact 라고 한다.
- Mark & Compact 알고리즘은 객체들의 참조를 확인하면서
참조가 연결되지 않은 객체를 표시한다.
  - 이 작업이 끝나면, 사용되지 않은 객체를 모두 표시하고 이 표시한 객체를 삭제한다.
- Full GC는 속도가 매우 느리며,
Full GC가 일어나는 도중에는 순간적으로 자바 애플리케이션이 멈춰버리기 때문에
Full GC가 일어나는 정도와 시간은 애플리케이션의 성능과 안정성에 아주 큰 영향을 미친다.

---
## GC가 중요한 이유
- 가비지 컬렉션 중 하나인 마이너 GC의 경우,
빠르게 끝나기 때문에 큰 문제가 되지 않지만,
Full GC의 경우 자바 애플리케이션이 멈춰버리기 때문에 문제가 될 수 있다.

- Full GC로 인해, 애플리케이션이 멈추는 동안
사용자의 요청이 큐에 들어있다가 순간적으로 요청이 한번에 들어오기 때문에
과부하에 의한 여러 장애를 만들 수도 있다.

- 따라서 원활한 서비스를 위해서는 GC가 어떻게 일어나게 하느냐가
시스템의 안정성과 성능에 큰 변수로 작용할 수 있다.

---
## 다양한 GC 알고리즘
- Copy & Scavenge ( Minor GC ), Mark & Compact ( Full GC )의 방식 말고도
다양한 GC 방법을 제공하고 있다.

예를 들면 다음과 같다.
- Default Collector
- Parallel GC for young generator
- Concurrent GC for old generator
- Increment GC ( Train GC )

### Default Collector
- Minor GC로 Copy & Scavenge,
Major GC로 Mark & Compact를 사용한다.

### Parallel GC
- 자바는 멀티 스레드 환경을 지원하지만,
하나의 CPU에서는 동시에 하나의 스레드만 수행 가능하다.

- 예전에는 하나의 CPU에서만 GC를 수행했지만,
후에 하나의 CPU에서 동시에 여러 개의 스레드를 실행할 수 있는 하이퍼스레딩 기술이나
여러 CPU를 동시에 장착한 하드웨어의 보급으로, 하나의 하드웨어에서 동시에 여러 개의 스레드를 수행할 수 있게 되었다.

- Parallel GC에는 크게 두 가지 종류의 옵션을 가지고 있는데,
Low-pause 방식과 Throughput 방식이 있다.

- Low-pause 방식은 GC가 일어날 때 애플리케이션이 멈추는 현상을 최소화하는데 역점을 두었다.

- Throughput 방식의 Parallel GC는
Minor GC가 발생하였을 때, 되도록이면 신속하게 수행하도록 Throughput에 중심을 두었다.

---

### Concurrent GC

- Full GC는 시간이 길고 애플리케이션이 순간 멈춰버리므로, 애플리케이션이 멈추는 방식을 최소화하는 GC 방식이다.

- Full GC에 소요되는 작업을 애플리케이션을 멈추고 진행하는 것이 아니라,
일부는 애플리케이션이 돌아가는 단계에서 수행하고, 최소한의 작업만을 애플리케이션이 멈췄을 때 수행하는 방식이다.

### Incremental GC ( Train GC )

- Train GC라고도 불리는 GC 방식의 의도는
Full GC에 의해서 애플리케이션이 멈추는 시간을 줄이기 위한 것이다.

- 작동은 Minor GC가 일어날 때마다 Old 영역을 조금씩 GC해서
Full GC가 발생하는 횟수나 시간을 줄이는 방식이다.

- Incremental GC는 많은 자원을 소모하고
Minor GC를 자주 일으켜서, 그리고
Incremental GC를 사용한다고 해서 Full GC가 없어지거나
그 횟수가 획기적으로 줄어드는 것은 아니다.
오히려 느려지는 경우가 많아서, 반드시 테스트를 거치고 사용해야한다.