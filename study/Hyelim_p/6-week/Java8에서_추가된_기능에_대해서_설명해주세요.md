# Java8에서 추가된 기능에 대해서 설명해주세요.

## Lambda 표현식
함수형 프로그래밍을 구성하기 위한 함수식이며, 간단히 말해 자바의 메소드를 간결한 함수 식으로 표현한 것이다.
람다식도 결국 객체이다. 즉, 인터페이스를 익명 클래스로 구현한 익명 구현 객체를 짧게 표현한 것이다.

```java
// (1) 람다식을 사용한 Runnable 인터페이스의 익명 구현 객체
Runnable runnable = () -> System.out.println("Hello, lambda!");

// (2) 익명 클래스를 사용한 기존 방식
Runnable runnableAnonymous = new Runnable() {
    @Override
    public void run() {
        System.out.println("Hello, anonymous class!");
    }
};
```
## 함수형 인터페이스
딱 하나의 추상 메소드가 선언된 인터페이스
람다식은 이 함수형 인터페이스 안에 정의된 하나의 추상 메소드 선언을 짧게 표현한 것이다.
```java
interface IAdd {
    int add(int x, int y);

    final boolean isNumber = true; // final 상수
    default void print() {}; // 디폴트 메서드
    static void print2() {}; // static 메서드
}
```
### @FunctionalInterface
두 개 이상의 추상 메서드가 선언되지 않도록 컴파일러가 체크해주는 기능이다.
인터페이스 선언 시 @FunctionalInterface 어노테이션을 붙여준다면, 두 개 이상의 메소드 선언 시 컴파일 오류를 발생시켜준다.
```java
@FunctionalInterface
public interface MyFunctional {
    public void method();
    public void otherMethod(); // 컴파일 오류 발생
}
```
## Stream
연속된 정보를 처리하기 위해 사용하는 클래스
## Default Method
인터페이스에 새로운 메서드를 구현하는 경우를 생각해보면 인터페이스를 구현한 구현체가 수십개라면? 변경할 것이 너무 많다. 
인터페이스를 구현하는 모든 클래스에는 새로 추가된 메서드를 구현해야한다. 
디폴트 메서드는 기존의 구현을 고치지 않고도 이미 공개된 인터페이스를 변경하기 위해 등장하였다.

기존의 코드를 최대한 변경하지 않으면서 설계된 인터페이스에 새로운 확장을 가능하게 만들기 위해 등장한 기술이다 (OCP 원칙과 관련)

## Optional
- null 또는 값을 감싸서 Runtime Exception 인 NPE(NullPointerException) 로부터 부담을 줄이기 위해 등장한 Wrapper 클래스이다.
- Optional은 값을 Wrapping하고 다시 풀고, null 일 경우에는 대체하는 함수를 호출하는 등의 오버헤드가 있으므로 잘못 사용하면 시스템 성능이 저하된다. 
그렇기 때문에 메소드의 반환 값이 절대 null이 아니라면 Optional을 사용하지 않는 것이 좋다. 
즉, Optional은 메소드의 결과가 null이 될 수 있으며, null에 의해 오류가 발생할 가능성이 매우 높을 때 반환값으로만 사용되어야 한다.

## 날짜 관련 클래스 추가
java.time 패키지에 날짜와 시간을 나타내는 여러 API 추가됨

자세한 내용은 <a href="https://scshim.tistory.com/251">링크</a> 를 참고하자
## 병렬 배열 정렬
## String Joiner
Java 8에서 새롭게 추가된 StringJoiner를 이용해 접두사, 접미사를 추가할 수 있다.
