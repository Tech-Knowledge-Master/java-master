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

![image](https://github.com/Tech-Knowledge-Master/java-master/assets/62535887/c53f47bf-be1d-4c4e-8570-d667e04e0ceb)

별도의 예외 처리를 하지 않아도 컴파일이 가능하다.
### Errors
- 라이브러리 비 호환성, 무한 재귀, 메모리 누수와 같은 심각하고 일반적으로 복구할 수 없는 상태를 나타낸다.
- OutOfMemoryError, StackOverflowError 


### throws 키워드
throws 는 메소드 선언부에서 사용되며 해당 메서드에서 발생할 수 있는 예외를 명시적으로 선언한다.
이 메소드를 호출하는 쪽에서 예외를 명시적으로 처리해야한다.
> "throw"는 예외를 발생시키는 데 사용되고, "throws"는 메서드에서 발생할 수 있는 예외를 선언하는 데 사용, 호출하는 쪽에서 예외 별도 처리 필요

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

### 자바 예외 권장 사항
throws 를 사용해 상위 메소드로 반복적인 예외를 던지는 것은 좋지 않다.

1-1. throws 를 사용해 상위메소드로 예외 처리를 위임(권장 ❌)
```Java
public class ObjectMapperUtil {

  private final ObjectMapper objectMapper = new ObjectMapper();

  // 예외처리를 throws를 통해서 위임하고 있습니다.
  public String writeValueAsString(Object object) throws JsonProcessingException {
    return objectMapper.writeValueAsString(object);
  }

  // 예외처리를 throws를 통해서 위임하고 있습니다.
  public <T> T readValue(String json, Class<T> clazz) throws IOException {
    return objectMapper.readValue(json, clazz);
  }
}
```

1-2. 메소드를 사용하는 곳에서 try-catch 로 처리하거나 throw 로 다시 예외를 발생시켜야한다. (권장 ❌)
   
![image](https://github.com/Tech-Knowledge-Master/java-master/assets/62535887/c7947340-be44-43dc-9d7f-4c84a13a227f)

2-1. 따라서 아래와 같은 방법으로 예외 처리를 구체화 시켜야한다.checkedException 을 uncheckedException 으로 구체화시키자 (권장 ⭕️)

![image](https://github.com/Tech-Knowledge-Master/java-master/assets/62535887/df185972-f9ed-4389-9d3d-2d15b4cb62e2)

2-2. Checked Exception을 Unckecekd Exception으로 던지고 있기 때문에 메소드를 호출하는 곳에서는 별도의 예외 처리가 필요가 없어진다.(권장 ⭕️)

![image](https://github.com/Tech-Knowledge-Master/java-master/assets/62535887/d86f7e46-3720-4c03-a8f1-341b42600bf0)


---

https://sup2is.github.io/2021/03/04/java-exceptions-and-spring-transactional.html
https://www.baeldung.com/java-exceptions#handling-exceptions
