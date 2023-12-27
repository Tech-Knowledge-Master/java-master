# GC 는 무엇이고 왜 필요한가
- JVM의 runtime data area의 heap 영역 중 참조되지 않는 객체를 데몬쓰레드를 통해 제거하는 것
  - 데몬쓰레드 
    - 일반 쓰레드의 작업을 돕는 보조적인 역할 수행
    - 일반 쓰레드가 모두 종료되면 데몬 쓰레드도 자동 종료
    - GC, 자동저장, 화면 자동갱신 등에 사용됨
    - 무한 루프와 조건문을 이용해서 실행 후 대기하다가 특정 조건이 만족되면 작업을 수행하고 다시 대기하도록 작성

- 대부분의 GC의 목표 : STW 시간(pause time, GC 실행을 위해 애플리케이션이 멈추는 것) 최소화 
- GC 수행 시 발생하는 STW 가 의도치 않은 장애의 원인이 될 수 있음 
  -> 그래서 GC 튜닝 (힙 영역을 조정하는 것) 필요 (JVM 메모리는 절대로 조정해선 안됨)
- 두가지 전제에 기반하여 설계됨
  1. 대부분의 객체는 금방 접근 불가한(unreachable) 상태가 된다.
  2. 오래된 객체가 젊은 객체를 참조하는 일은 아주 적게 존재한다. 


  
# GC의 동작방식 
- heap 영역을 young, old 로 구분 (GC 마다 다르지만 초기 방식에 해당)
- Minor GC
  - Young generation (Eden, Survivor0, Survivor1 으로 구성)
  - 동작과정
    1. 객체가 새롭게 생성되면 eden 영역에 메모리가 할당됨
    2. eden 영역이 가득차면 minor GC 동작  
       eden 영역의 객체 중 살아있는 객체는 survivor0 영역으로 copy, eden 영역의 메모리는 비워진다.
    3. 다시 eden 영역이 가득차면 minor GC 동작  
       eden 영역과 survivor0 영역의 객체 중 살아있는 객체는 survivor1 영역으로 copy, eden 영역은 비워진다.
    4. eden 영역이 가득찰 때마다 survivor0,1 을 오가며 minor GC가 반복.  
       survivor 영역을 이동하는 이유는 메모리 파편화를 방지하기 위해서임.  
       살아남은 횟수는 age bit에 기록되고, tenuring threshold를 넘기거나 survivor 영역의 메모리가 부족해지는 경우 old 영역으로 이동(promotion)

- Major GC (Full GC)
  - STW 발생 
  - Old generation
  - GC 튜닝 : Old 영역에서 발생하는 STW 시간을 단축시키는 것
  - Major GC를 수행할 때 어디에 초점을 맞추느냐에 따라 두가지 방식으로 구분
    - throughput collector
      - gc 수행 시 모든 리소스를 투입해서 gc를 빨리 끝내자. STW 시간을 단축시키는 것이 목적
      - serial gc, parallel gc, parallel old gc
    - low pause(latency) collector
      - STW 발생을 분산시켜 체감 pause time을 줄이자. 
      - gc 수행과 동시에 작업 수행 가능 
      - cms, g1
  
  - Serial GC
    - mark-sweep-compaction 알고리즘 사용
    - 싱글 쓰레드 사용 
    - 현재는 거의 사용 X 
  - Parallel GC
    - Serial Gc와 동작과정 동일하지만, Minor GC를 멀티쓰레드로 수행
    - Java 8 버전의 디폴트 GC
  - Parallel Old GC (Parallel Compacting GC)
    - Parallel GC의 개선된 버전
    - Old 영역에서도 멀티쓰레드로 GC tngod
    - mark-summary-compaction 알고리즘 사용
      1. mark
         old 영역의 region을 구분하고, region 별로 살아있는 객체를 체크  
         이때 살아있는 객체의 크기, 위치정보 등 region 별 통계 정보를 만듦  
      2. summary  
         하나의 쓰레드가 GC를 수행하고, 나머지 쓰레드는 애플리케이션 수행  
         region 별 통계정보로 살아있는 객체의 밀도가 높다고 판단되는 region이 어디까지인지 dense prefix를 정함  
         오랜기간 참조된 객체는 앞으로도 사용할 확률이 높다는 가정 하에 dense prefix를 기준으로 compaction 범위를 줄인다.
      3. compaction  
         heap을 일시정지 상태로 만들고 쓰레드들이 각 region을 할당받아 compaction 수행  
         compaction 영역을 destination과 source로 나누어, 살아있는 객체는 destination으로 이동시키고 참조되지 않는 객체는 제거  

- CMS GC (Concurrent Mark Sweep)
    - GC 작업의 대부분을 애플리케이션의 실행과 동시에 수행  
      -> STW를 나눠서 실행하여 체감 pause time을 줄일 수 있음
    - compaction 작업 X -> fragmentation 발생  
      -> 연속적인 메모리 할당이 어려울 정도로 단편하된 경우에 compaction 진행함(full gc). 이때 pause time 오래 걸릴 수 있음   
    - 여러 cpu 코어를 사용하여 동시에 gc 작업을 수행하기에 cpu 리소스를 많이 소비 
    - Java 9 ~ deprecated, Java 14 ~ 사용 중지 

- G1 GC (Garbage first)
  - CMS 를 대체하기 위해 만들어짐
  - 하드웨어의 발전으로 큰 메모리에서 좋은 성능을 내는 것에 초점을 두어 CMS에 비해 pause time 개선  
    -> 대용량 힙 메모리를 사용하는 애플리케이션에 적합
  - 물리적 generation 구분을 없애고, 전체 heap을 1~32mb 단위의 regions로 재편 (약 2048개의 region으로 나눌 수 있음)
  - 각 region은 상태에 따라 역할이 동적으로 부여됨
    - eden : 새로 생긴 객체들이 할당되는 영역  
    - survivor : 살아있는 객체들이 할당되는 영역
    - old : 오래 살아있는 객체들이 할당되는 영역 
    - humongous : region 크기의 50%가 넘는 큰 객체를 저장하기 위한 영역, gc가 최적으로 동작하지 않음  
    - available/unused : 아직 사용되지 않는 영역
  - 동작 과정 : region 별로 순차적으로 작업 진행, 가비지로만 가득찬 region이 발견되자마자 gc 를 진행
    - young GC (STW 발생)
      - pause time 을 줄이기 위해 멀티쓰레드로 작업
      - eden, survivor 영역에서 수행됨
      - GC 후 살아있는 객체를 다른 region 으로 이동시키고, 비워진 region을 available region으로 변경
    - old GC
      - 자세한 동작 과정 생략
      - 메모리 파편화 발생 

- Shenandoah GC
  - OpenJDK 12 에 처음 도입, jdk 8, 11 지원
  - 전체 힙 영역을 대상으로 하는 큰 GC를 적은 횟수로 수행하는 것보다 작은 GC(young 영역)를 여러번 수행하는 전략과 Concurrent GC를 통해  
    CPU를 더 사용하면서 pause time 을 줄이고 보다 일관된 STW 제공

- ZGC
  - jdk 11+ 지원
  - ZPage (region 개념) : 동적으로 생성/삭제되며, 2MB의 배수 형태로 관리

