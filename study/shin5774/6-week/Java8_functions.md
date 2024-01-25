# Java8에서 추가된 기능에 대해서 설명해주세요.

## 답변

```
자바 8에는 람다식과 스트림, 인터페이스의 디폴트 메서드,날짜 관련 패키지인 java.time과 StringJoiner등이 추가되었습니다. 
람다식과 스트림을 통해 코드의 가독성과 성능을 향상시켰고 디폴트 메서드를 통해 인터페이스에서 구현체가 있을수 있게되었으며 time 패키지를 통해 기존 시간 관련 클래스의 문제를 해결하였습니다.
```

## 내용 정리

- Lambda Expression 및 Method Reference
    - Lambda Expression

      함수적 프로그래밍의 형태로 재사용이 가능 한 코드 블록

      기존의 anonymous inner class를 이용한 처리방식을 간결하게 처리할수 있다.

        - anonymous inner class(익명 클래스)

          클래스의 이름이 없는 경우로 상속받은 추상클래스 또는 인터페이스를 바로 객체로 생성 및 반환함.(한번만 사용하려고 만든 클래스)

            - 구현 방법
                - new (추상클래스/인터페이스)명(){ //메서드 구현부 작성 }
        - 타겟타입

          Lambda Expression이 할당되는 인터페이스로 abstract method가 반드시 하나만 존재해야 함.

          @FunctionalInterface를 통해 이를 체크할수 있음.

        - 표현식

          ((type) variable) → { //코드 }

- Method Reference

  Lambda Expression 내부에서 다른 함수 하나만 실행하는 경우에 `::` 연산자를 이용해 기존 메서드를 참조함

    - 표현식

      <소유자>::<method>

      ex)Integer::compareTo

- Streams

  Collections에서 Streams API를 사용하여, 이전의 반복문이 아닌 함수형 구현

    - 스트림 등장 이전에는 iterator interface를 사용해 반복을 처리했음.
    - 기존의 `외부 반복자`와 다르게 `내부 반복자`를 사용하여 병렬처리에 적합해짐
        - 외부 반복자

          기존의 for문 방식, 반복문을 통해 직접 컬렉션 요소를 꺼내서 반복처리를 하는것

        - 내부 반복자

          처리할 행동을 컬렉션에 넘겨주어서 내부에서 반복처리를 하는것

    - 특징
        - 지연 처리(Lazy Invocation)

          스트림의 최종작업이 진행되지 않는다면 스트림의 연산은 실행되지 않는다.

        - 처리 순서

          스트림은 요소 하나씩 수직적으로 처리한다.

        - 병렬 처리

          Parallel Stream을 통해 병렬 스트림을 만들수 있다.

- Interface의 Default Method
  
    [인터페이스 설명](../3-week/인터페이스와_추상클래스의_차이점.md)
- Optional Class

  모든 참조타입을 감쌀수 있는 Wrapper Class로 Optional 객체를 통해 NullPointException을 회피할수 있게 된다.

    - 설계의도 (by Brian Goetz,Java Architect)

      null을 반환할때 오류 발생 가능성이 매우 높아 ‘결과없음’을 명확하게 드러내기 위해 메서드의 반환 타입으로 사용하기 위해 설계됨.

    - 문제점
        1. Wrapper 클래스 및 null 회피를 위한 다른 메서드 호출로 인해 오버헤드가 있게됨.

           ⇒ 과도한 사용으로 인해 성능이 저하될수 있다.

           → 결과가 null이나 null에 의해 오류가 발생할 가능성이 매우 높은 경우에만 사용해야한다.

        2. NullPointerException 대신 NoSuchElementException이 발생함.
        3. 코드의 가독성이 안좋아짐.
        4. 새로운 문제가 발생함

           대표적으로 Serialize를 할때마다 Optional은 직렬화를 지원 안하기에 문제가 발생함.

    - 적절한 사용법
        - Optional 변수에 Null을 할당하지 말아라
        - 값이 없는 경우 .orElseX()를 통해 기본값을 반환
        - 단순 값 획득 목적이라면 사용하지 마라
        - 생성자,수정자,메서드 파라미터등으로 Optional을 넘기지 말아라
        - Collection의 경우는 빈 Collection을 사용
        - 반환 타입으로만 사용하라
- Functional Interface

  Lambda Expression의 타겟타입과 동일하게 abstract method가 오직 하나인 인터페이스
  
  - 기본 제공 Functional Interface
      - 기본 제공 Functional Interface
        - Predicate : T → boolean /	boolean test(T t)
        - Consumer : T → void /	void accept(T t)
        - Supplier : () → T / T get()
        - Function<T,R> : T → R	/ R apply(T t)
        - Comparator : (T, T) → int	/ int compare(T o1, T o2)
        - Runnable : () → void / void run()
        - Callable : () → T	/ V call()
      - Supplier vs Callable

        Callable이 병렬처리를 위해 등장한 개념으로 딱히 차이는 없다.
  - Bi interface
    - BiPredicate : (T, U) → boolean / boolean test(T t, U u)
    - BiConsumer : (T, U) → void / void accept(T t, U u)
    - BiFunction : (T, U) → R / R apply(T t, U u)

- 날짜 관련 클래스
  - 등장배경

    기존 Date,Calandar 클래스에서 있던 여러 문제점에 대응하기 위해 나오게 됨

      - Date

        1900년 오프셋,0 시작 인덱스등 모호한 설계와 toString()으로 반환되는 문자열을 활용하기 어려움

      - Calandar

        인덱스는 그대로,쉽게 에러를 일으킴, DateFormat이 스레드에 안전하지 않음.

        윤초같은 특별케이스를 고려하지 못함.

    ⇒ 두 클래스 다 가변 클래스
    
  - java.time 패키지
    
      날짜와 시간을 나타내는 여러 api가 모여있는 패키지
    
      - 구성
          - LocalDate
            
              불변객체, 정적 팩토리 메서드를 사용하여 of를 통해 인스턴스를 만듦.
            
          - LocalTime
            
              LocalDate와 동일, 시간을 나타내는 클래스
            
          - LocalDateTime
            
              LocalDate와 LocalTime을 쌍으로 가지는 복합 클래스. 둘을 동시에 설정할수도 있고 각각을 따로 설정할수도 있다.
            
          - ZoneDateTime
            
              LocalDateTime에 시간대를 추가한 클래스
            
          - ZoneId
            
              타임존을 나타내는 클래스
            
          - DataTimeFormatter
            
              날짜,시간 개체 처리를 도와주는 포맷터
            
          - DayOfWeek(Enum)
            
              요일을 나타내는 열거형.
            
          - Instant
            
              유닉스 에포크시간을 기준으로 특정 지점까지의 시간을 초로 표현하는 클래스로 나노초의 정밀도를 제공한다.
            
              - 유닉스 에포크 시간
                
                  1970년 1월 1일 0시0분0초를 기준.
                
          - Duration
            
              두 시간 객체 사이의 시간을 초와 나노초로 표현해주는 클래스
            
          - Period
            
              두 시간 객체 사이의 시간을 년,월,일로 표현해주는 클래스

- 병렬 배열 정렬

  parallelSort() 메서드를 통해 여러 쓰레드를 사용해 병렬적으로 배열을 정렬할수 있게 되었다.

- StringJoiner

  문자열에 공백이나 구분자를 반복해서 붙여야 하는 경우에 유용하게 사용할수 있는 클래스

    - String.join 과의 차이점

      없음, String.join 내부에서 StringJoiner를 사용

      StringJoiner에서는 StringBuilder를 사용