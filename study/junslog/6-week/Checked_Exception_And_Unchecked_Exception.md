### Checked Exception과 Unchecked Exception에 대해 설명해주세요. 스프링 트랜잭션 추상화에서 Rollback 대상은 무엇인가요?

```
Checked Exception 과 Unchecked Exception은
자바의 예외 처리 계층 중 Exception 클래스의 하위 클래스들입니다.
이 중 RuntimeException을 상속받은 클래스들을 Unchecked Exception이라 합니다.
그 외의 예외는 컴파일 단계에서 무조건 처리를 해주어야 하는 Checked Exception 입니다.
스프링에서는 트랜잭션 내에서 Unchecked Exception이 발생 시 RollBack을 합니다.
```

### 꼬리질문
- 자바의 프로그램 오류 발생 시 예외가 발생하는데요. 아예 처리가 불가능한 오류같은 경우를 뭐라고 이야기하나요?
그리고 그에 대한 예시를 들어주세요.

```
메모리 부족이나 스택 오버플로우 같은 JVM 시스템적인 문제같은 경우는
프로그래머가 처리할 수 없는 영역의 오류입니다. 이를 Error 라고 합니다.
예시로는, `OutOfMemoryError` 나, `StackOverflowError` 를 들 수 있습니다.
```

- 예외 처리를 계층 구조로 만들었는데, 이렇게 만든 이유가 무엇일까요?

```
상속을 통해 계층 구조를 확립함으로써,
예외처리 시, 다형성을 활용할 수 있어 개발이 용이해지는 장점이 있습니다.
```

[인터페이스 상속이 아닌 구현 상속의 문제점](https://hoons-dev.tistory.com/106)

자바가 실행 시 프로그램 오류를 나타내는 방식은
크게 3가지로 나눌 수 있습니다.

1) 예외 ( Exception )
   - 체크 예외 ( Checked Exception )
   - 언체크 예외 ( Unchecked Exception )
2) 에러 ( Error )

#### 자바의 예외 계층구조
![자바_예외_계층.png](img%2F%EC%9E%90%EB%B0%94_%EC%98%88%EC%99%B8_%EA%B3%84%EC%B8%B5.png)

Object 클래스를 상속받은 Throwable 클래스를 기준으로
Error, Exception 클래스로 나뉘어짐.

자바가 실행될 때 ( Runtime )
발생할 수 있는 프로그램 오류를
에러( Error )와 예외( Exception )로 구분합니다.

### [ Error ]
- 시스템에 비정상적인 상황이 발생했을 경우 발생한다.

- 메모리 부족 ( OOM; OutOfMemoryError ), 스택오버플로우(StackOverflowError)와 같이 복구할 수 없는 것을 말한다.

- 개발자가 예측하기 쉽지 않고, 처리할 수 있는 방법도 없음.

### [ Exception ]
- 프로그램 실행 중, 개발자의 실수로 예기치 않은 상황이 발생했을 때

- 배열의 범위 초과 ( ArrayIndexOutOfBoundsException )
- 값이 Null인 참조변수를 참조한 경우 ( NullPointerException )
- 존재하지 않는 파일의 이름을 입력 ( FileNotFoundException )
등등..

예외는
- 체크 예외 ( Checked Exception )
- 언체크 예외 ( Unchecked Exception )
으로 나뉨.

자바 예외 클래스의 계층 구조를 보았을 때,
RuntimeException의 하위 클래스들이 UncheckedException 이라 하고,
RuntimeException의 하위 클래스가 아닌 Exception 클래스의 하위 클래스들을 Checked Exception이라고 함.

---

## 체크 예외 ( Checked Exception )
- 체크 예외는 RuntimeException의 하위 클래스가 아니면서,
Exception 클래스의 하위 클래스들입니다. 체크 예외의 특징은 반드시 컴파일 타임에 에러를 처리해야하는 특징을 가지고 있습니다.
  ( try/catch or throw )

- FileNotFoundException, ClassNotFoundException 등의 예외가 있습니다.


## 언체크 예외 ( Unchecked Exception )
- RuntimeException의 하위 클래스들을 의미합니다.

- 체크 예외와 달리 에러 처리를 강제하지 않습니다.

- 실행 중 발생할 수 있는 예외를 의미합니다.

- ArrayIndexOutOfBoundsException, NullPointerException 등을 의미합니다.

-> 컴파일러가 예외처리를 학인하지 않은 RuntimeException 클래스들은 Unchecked 예외라 부르고,
예외처리를 확인하는 Exception 클래스들은 Checked 예외라고 부릅니다.


## 체크 예외와 언체크 예외의 Rollback 여부

- Checked Exception
-> 반드시 예외를 컴파일 단계에서 처리해야 합니다.
-> 예외 발생 시 트랜잭션을 Rollback 하지 않고, commit 까지 완료됩니다.
-> 대표적인 예외로는 IOException, SQLException이 있습니다.

- Unchecked Exception
-> 명시적인 처리를 강제하지 않습니다.
-> 런타임에 발생 여부를 확인할 수 있습니다.
-> 예외 발생 시 트랜잭션을 Rollback 합니다.
-> 대표적인 예외로는 NullPointerException(NPE), IllegalArgumentException, IndexOutOfBoundException, SystemException