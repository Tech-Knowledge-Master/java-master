# GC가 무엇인지, 필요한 이유는 무엇인지, 동작방식에 대해 설명해주세요.

## GC(Garbage Collection)
- GC(Garbage Collection): 메모리 관리 방법중 하나로 Java에서는 JVM의 Heap 영역에서 동적으로 할당된 메모리중 필요 없어진 메모리 객체를 주기적으로 제거하는 프로세스.
    - 사용 목적: 불필요한 메모리의 소모를 막기위해서
    - 문제점
        1. 메모리 해제 시기 파악이 어려워 제어가 힘듬.
        2. GC동작중에는 다른 동작이 멈춰서 오버헤드가 발생함(STW,Stop The World)
    - 청소 방식
        - Mark-Sweep: 기초적인 청소 방식
            1. Mark: GC의 Rootspace부터 그래프 순회로 연결된 객체를 찾아내서 마킹
                -GC의 Rootspace: JVM에서는 stack,method stack,method area를 의미함.
                -마킹을 통해 객체는 Reachable(객체 참조됨)과 Unrechable(참조 안됨,GC대상)로 나뉜다.
            2. Sweep: 참조되지 않은 객체를 Heap에서 제거
            3. Compact(Optional): 분산된 객체를 Heap의 시작주소로 모아서 압축함
    - JVM Heap Area의 GC 구분
        - Minor GC: Young Generation에서의 GC
            - Young Generation은 크기가 작아서 시간이 적게 걸림.
            - GC 수행후 남은 GC들은 age값이 증가함.
            - age값이 기준치를 넘는 객체는 Old Generation으로 넘어간다
        - Major GC: Old Generation에서의 GC
            - 크기가 커서 수행 시간이 김 -> STW 문제가 발생
    - 알고리즘 종류
        1. Serial GC: CPU 코어가 1개일때 사용하는 가장 단순한 GC
            - 싱글 쓰레드로 GC 수행 -> STW 시간이 김.
        2. Parallel GC: Java 8의 디폴트 GC, Minor GC를 멀티쓰레드로 수행.
            - Serial GC에 비해 STW 시간이 감소함.
        3. Parallel Old GC: Parallel GC의 개선판.
            - 두 GC를 멀티쓰레드로 수행.
            - 새로운 청소방식인 `Mark-Summary-Compact` 방식을 사용함
                - Mark-Summary-Compact
                    1. Mark: old 영역을 region 단위로 나눠서 자주 참조되는 객체를 식별
                    2. Summary: region별 통계정보를 통해 생존한 객체들의 밀도가 높은 부분이 어디까지인지 dense prefix를 정함.
                    3. Compact: compact영역을 destination과 source로 나눠서 생존 객체를 destination으로 보내고 나머지를 제거.
        4. CMS(Concurrent Mark Sweep) GC: 어플리케이션의 쓰레드와 GC 쓰레드를 동시에 실행해 STW 시간을 줄이기 위해 고안된 알고리즘
            - GC 과정이 매우 복잡해짐
                1. Initial Mark: GC의 root가 참조하는 객체를 마킹함,STW 발생
                2. Concurrent Mark: 참조된 객체를 따라가면서 지속적으로 마킹함.
                3. Remark: Concurrent mark에서 변경점이 없는지 다시 마킹,STW 발생
                4. Concurrent Sweep: unreachable 객체 제거
            - 메모리 파편화 문제가 발생함.
                - 별도의 Compaction 작업이 없기때문
            - Java 14 이후부터는 사용이 중지됨.
        5. G1(Garbage First) GC: CMS GC를 대체하기 위해 나온 알고리즘
            - Java 9+의 디폴트 GC
            - 4GB 이상의 Heap,STW시간이 0.5초가 필요한 상황에 사용이 권장.
                - Heap의 크기가 작으면 미사용을 권장함.
            - Heap을 나누는 방식에 `Region`이라는 개념을 도입함.
                - Region: Heap 영역을 분할해서 동적으로 역할을 부여하는 개념
                    - 기존 Young/Old 개념 사용 및 개념 추가
                        1. Humonogous: region의 크기의 50%를 초과하는 객체가 있는 region
                        2. Available/Unused: 사용되지 않은 region
            - Region별로 GC를 진행하기에 GC 빈도를 줄이고 소요시간이 감소됨.
            - Cycle
                - Young-only: Minor GC만 수행되는 단계
                    - 설정한 Old Generation의 비율을 초과하면 Major GC를 수행함.
                - Space Reclamation(공간 회수): Mixed GC가 수행됨
            - GC의 종류
                1. Minor GC: Young에 있는 유효객체를 다른 region(old,survivor,available/unused)로 옮김.
                2. Major GC: Old에 있는 유효객체를 다른 region(old,survivor,available/unused)로 옮김.
                    - 단계
                        1. Initial Mark: Old region에 존재하는 객체들이 참조하는 Survivor Region을 마킹함 ,STW 발생
                        2. Root Region Scan: Initial mark에서 스캔된 survivor region에 대해 scan 작업을 한다.
                            - Old Region을 참조하고 있는 객체에 대해 마킹
                        3. Concurrent Marking: 모든 Old Generation 내에 생존해 있는 객체를 마킹함.
                            - region 내의 객체가 모두 garbage인 region을 체크함.
                        4. Remark: 앞서 체크된 region을 회수하고 Concurrent Mark 작업을 마무리한다,STW 발생
                            - SATB(Snapshot-At-The-Beginning) 기법을 사용함.
                                - 최초 Object 그래프를 스냅샷해서 마킹 전후를 비교해 발생 가능한 문제를 보완함.
                        5. Cleanup & Copy: 유효 객체를 다른 region으로 옮기면서(Evacuation) compact 작업을 수행함.,STW 발생
                            - 유효객체의 비율이 낮은 region 순으로 순차적으로 수거함.
                            - 살아있는 객체가 아주 적은 Old 영역에 대해 GC pause를 표시하고 Young GC때 수집되도록 한다.
                3. Mixed GC: Young,Old 모두 Evacuation을 수행하는 과정
                    -STW를 줄이기위해 여러번 수행함(기본 8번 수행)
            - 문제점
                1. Minor/Major GC를 수행해도 공간이 모자라다면 Full GC를 수행하는데 이때 Single Thread로 동작하기에 시간이 발생함.
                2. Heap 공간이 작은 application에서는 Full GC가 자주 발생함.
                3. Humonogous 영역은 최적화가 되어 있지 않아서 해당 영역이 많으면 성능이 떨어짐.
        6. Shenandoah GC: 레드헷에서 개발한 GC로 CMS의 단편화, G1이 가진 pause 문제를 해결함.
            - Java 12에 release 됨.
            - 강력한 Concurrency와 가벼운 GC로직으로 heap 사이즈에 영향을 받지 않고 일정한 pause시간이 소요됨.
            - G1과 다르게 region을 generation별로 나누지 않음.
            - 작동 방식
                1. Initial Marking: concurrent marking을 시작하고 root set을 검사한다.
                    - pause 발생 (1st pause)
                2. Concurrent Marking: heap 전반에 걸쳐서 reachable object를 추적함.
                3. Final Marking: root set을 재탐색해 Concurrent Marking을 완료함.
                    - 이동 가능한 region을 파악해 Evacuation 초기화 및 일부 root를 이동시킴.
                    - pause 발생 (2nd pause)
                4. Concurrent Cleanup: garbage 영역을 비움.
                5. Concurrent Evacuation: collection set에서 벗어난 object를 다른 region으로 복사.
                    - collection set: GC가 수행될 Region이 저장되어 있는 자료구조.
                6. Init Update Refs: 전 단계를 마무리하고 다음단계를 준비하는 과정.
                    - Update Reference 단계 초기화 및 evacutaion 종료 -> 다음 단계 준비
                    - pause 발생 (3rd pause)
                7. Concurrent Update References: concurrent evacutaion된 object 참조를 갱신함.
                8. Concurrent Cleanup: 참조가 없는 region을 회수함.
        7. Z GC: 대량의 메모리를 low-latency로 처리하기 위해 만들어진 GC
            - Java 15에 release
            - Region과 유사한 `ZPage`라는 개념을 도입함.
                - Zpage : 2MB의 배수로 동적으로 만들어진 영역
                    - Small(2MB),Medium(32MB),Large(2*N MB)로 구분이됨.
            - 특징: Heap 크기가 커져도 STW 시간이 10ms를 넘지 않는다.
            - 작동 방식
                - 10 phase로 구성되고 크게 coloring과 relocation으로 구분함.
                - Coloring(phase 1~5)
                    - phase 1: 각 스레드의 로컬 변수를 스캔하고 이를 GC root로 하고 GC root set을 생성.
                        - STW 시간이 짧음. -> 스레드별 로컬 변수가 적어서
                        - 기존 ZPage를 검사하고 cycle후 살아남을 객체를 넣을 새 Zpage를 생성.
                    - phase 2~4(Concurrent Mark): Mark stack 내부 객체를 대상으로 Marking/Remapping을 진행함.
                        - GC root에서 접근가능한 객체에 coloring(=marking)과 remapping을 실행함.
                        - coloring 결과로 garbage가 하나라도 있는 Zpage의 집합을 relocation set이라고 한다.
                    - phase 5:soft reference, week reference, phantom reference에 대한 처리를 진행.
                        - STW 발생
                - Relocation(phase 6~10)
                    - phase 6: 이전 cycle에서 식별한 relocation set 초기화
                    - phase 8: garbage 해제 및 살아있는 객체를 새 Zpage에 재할당(relocation).
                    - phase 9,10: 살아있는 객체를 새 Zpage로 relocation.
            - 사용 개념
                - Colored Pointers: GC 처리시에 활용되는 4bit의 flag
                    - 가상 메모리 48bit에서 42bit로 줄이고 42~45bit를 Colored pointer로 지정함.
                    - 42부터 순서대로 Marked 0,Marked 1,Remapped,Finalized라고 불림.
                    - 마스킹을 통해 GC 처리에 활용
                - Load Barriers: heap으로부터 참조가 일어날 때마다 실행되는 코드
                    - Colored Pointer를 보고 bad color(부적합)를 확인하고 상황에 따라 slow path를 실행함.
                        - slow path: 객체가 정상적이지 않은 경우에 실행됨.
                            - 상황에 따라 Relocation,Remark,Remapping이 수행됨.
                                - Remapping: 살아있는 객체를 새 Zpage에 옮기고 그 주소를 바라볼수 있도록 forward table을 할당하고 관리하는것
