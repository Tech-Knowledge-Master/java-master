# Checked Exception과 Unchecked Exception에 대해 설명해주세요. 스프링 트랜잭션 추상화에서 rollback 대상은 무엇일까요?
컴파일러가 에러처리를 확인하지 않는 RuntimeException 클래스들은 unchecked 예외라고 부르고 
예외처리를 확인하는 Exception 클래스들은 checked 예외이다.

```
              ---> Throwable <--- 
              |    (checked)     |
              |                  |
              |                  |
      ---> Exception           Error
      |    (checked)        (unchecked)
      |
RuntimeException
  (unchecked)

```
### Checked exceptions
- Checked Exception 은 자바 컴파일러가 처리해야하는 예외이다.
- throw 키워드를 사용해서 선언적으로 예외를 던지거나 try-catch 형태로 예외를 직접 처리해야 한다는 의미
- Exception 클래스를 상속하는 클래스들 - IOException, ServletException 등
- 클라이언트가 예외를 직접 처리하고 예외를 복구할 것으로 예측할 수 있을 때 사용하기 적합한 Exception 타입

![img_1.png](img_1.png)

예외를 처리하지 않으면 위처럼 컴파일러가 에러를 보여준다.
### Unchecked exceptions / Runtime exceptions
- UnCheckedException 은 자바 컴파일러가 처리할 필요가 없는 예외이다. 컴파일러가 신경 쓰지 않기 때문에 별도의 예외처리를 해주지 않아도 된다.
- NullPointerException, IllegalArgumentException, and SecurityException.
### Errors
- 라이브러리 비 호환성, 무한 재귀, 메모리 누수와 같은 심각하고 일반적으로 복구할 수 없는 상태를 나타낸다.
- OutOfMemoryError, StackOverflowError 


### throws 키워드
throws 는 메소드 선언부에서 사용되며 해당 메서드에서 발생할 수 있는 예외를 명시적으로 선언한다.
이런 맥락에서 throw 키워드 뒤에 오는 예외는 주로 Checked Exception 이다.
이 메소드를 호출하는 쪽에서 예외를 명시적으로 처리해야한다.
> "throw"는 예외를 발생시키는 데 사용되고, "throws"는 메서드에서 발생할 수 있는 예외를 선언하는 데 사용

### try-catch-finally
try 블록 안에서 예외가 발생할 수 있는 코드를 감싸고, 예외가 발생하면 catch 블록이 실행되어 예외를 처리한다.
```java
public class Example {
    public static void main(String[] args) {
        try {
            // 예외가 발생할 수 있는 코드
            int result = divide(10, 0);
            System.out.println("Result: " + result);
        } catch (ArithmeticException e) {
            // 예외 처리
            System.out.println("Cannot divide by zero.");
        } finally {
            // 항상 실행되는 코드
            System.out.println("Finally block: This will be executed no matter what.");
        }
    }

    public static int divide(int numerator, int denominator) {
        return numerator / denominator;
    }
}

```

### 스프링 트랜잭션 추상화에서 rollback 의 대상
기본적으로 CheckedException 은 예외가 발생하면 트랜잭션 roll-back 하지 않고 예외를 던져준다. 하지만
Unchecked Exception 은 예외 발생 시 트랜잭션 roll-back 한다는 점에서 큰 차이가 있다.


---

https://sup2is.github.io/2021/03/04/java-exceptions-and-spring-transactional.html
https://www.baeldung.com/java-exceptions#handling-exceptions
