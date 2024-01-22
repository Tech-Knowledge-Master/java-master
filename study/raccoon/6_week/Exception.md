# Exception
자바의 예외는 크게 **Error**, **Checked Exception**, **Unchecked Exception**으로 나눌 수 있습니다.

![javaInError.png](exceptionImages%2FjavaInError.png)

## Error
에러는 시스템에 비정상인 상황이 발생했을 때 발생합니다.

이런 에러는 개발자가 처리할 수 있는 방법이 많지 않습니다.

## Exception
예외는 프로그램 실행 중에 개발자의 실수로 예기치 않는 상황이 발생했을 때 입니다.

예외에는 크게 Uncheck, Check로 구별할 수 있습니다.

### Check Exception
RuntimeException의 하위 클래스가 아니면서 Exception의 하위 클래스를 의미합니다.

즉 반드시 예외 처리를 해야 하는 특징(try / catch ro throw)을 가지고 있습니다.

### Uncheck Exception
언체크 예외는 RuntimeException의 하위 클래스를 의미합니다.

말 그대로 Runtime에 발생할 수 있는 예외를 의미합니다.

예를 들어 다음과 같은 예외가 발생합니다.
- ArrayIndexOutOfBoundsException
- NullPointerException

이런 에러는 개발자의 단순 실수로 나오기 때문에 try / catch를 강요하지 ㅇ낳습니다.

### RollBack 여부
|                 | Checked Exception                                                                                | Unchecked Exception                                                                                                                           |
|-----------------|--------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| 처리 여부           | 반드시 예외 처리를 해야함                                                                                   | 명시적인 처리를 강제하지 않음                                                                                                                              |
| 확인 시점           | 컴파일 단계                                                                                           | 실행 단계                                                                                                                                         |
| 예외 발생 시 트랜잭션 처리 | Roll-Back 하지 않음                                                                                  | Roll-Back 함                                                                                                                                   |
| 대표 예외           | Exception의 상속받는 하위 클래스 중 Runtime Exception을 제외한 모든 예외 <br> - IOExcepton <br/>- SQLException<br/> | RuntimeException 하위 예외 <br/> - NullPointerException <br/> - IllegalArgumentException <br/> - IndexOutOfBoundException <br/> - SystemException |

**여기서 Roll-Back은 SQL에서 AllOrNothing을 말하는 것으로, 모든 작동이 실행되지 않는다면 중간이 정지된 데이터를 날려버리는 방식을 말한다.**

### 참고 자료
- https://devlog-wjdrbs96.tistory.com/351
- https://cocodo.tistory.com/10
- https://velog.io/@eastperson/Java%EC%9D%98-Checked-Exception%EC%9D%80-%EC%8B%A4%EC%88%98%EB%8B%A4-83omm70j
- 