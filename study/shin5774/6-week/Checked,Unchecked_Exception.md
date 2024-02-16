# Checked Exception과 Unchecked Exception에 대해 설명해주세요. 스프링 트랜잭션 추상화에서 rollback 대상은 무엇일까요?

## 답변

```
Checked Exception은 예외처리 구문이 없다면 컴파일이 안되는 Exception을 의미합니다.
UnChecked Exception은 예외처리 구문이 없어도 컴파일이 진행되는 Exception으로 RuntimeException과 이를 상속받는 하위 클래스가 이에 해당됩니다.
스프링 트랜잭션 추상화에서 rollback 대상은 UnChecked Exception 입니다.
```

## 내용 정리

- Checked Exception

  예외 처리 구문(try-catch/throw)가 없다면 컴파일이 진행되지 않는 Exception   

- Unchecked Exception

  예외 처리 코드가 없더라도 컴파일이 진행되는 Exception

  RuntimeException과 이를 상속받는 하위 클래스가 해당한다.

  해당 Exception은 대부분 시스템의 문제보다는 개발자나사용자의 실수로 인해 발생하는 경우가 많다. 따라서 사용자의 입력에서 발생할수 있는 Exception을 제외하고는 Exception handling이 아닌 Debuging을 통해서 해당 케이스를 막아주는것이 성능적으로 좋다.

- 사용자 정의 Exception

  사용자 정의 Exception 또한 Checked와 Unchecked로 나뉘는데 이를 구분하는 기준은 해당 클래스가 RuntimeException이나 하위 클래스를 상속하느냐 여부에 달려있다.

- 스프링 트랜잭션 추상화

  트랜잭션을 사용하는 코드가 데이터 접근 기술마다 다르고 접근 기술을 바꿀때마다 서비스 계층 코드도 바뀌는 문제를 해결하기 위해 추상화시키는 과정.

  ⇒ 기능을 추상화한 인터페이스에 의존시킴.

  → 스프링에서는 PlatformTransactionManager interface가 그것이다.

  스프링 트랜잭션에서는 트랜잭션 메서드 실행중에 발생하는 uncheckException, 또는 error를 default로 rollback대상으로 간주함.