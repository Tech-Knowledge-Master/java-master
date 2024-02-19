# 자바에서 null을 안전하게 다루는 방법에 대해 설명해주세요.

## null 정의
* 객체가 없거나 상태가 정의되지 않은 상태

## null 을 안전하게 다르는 방법
## 1) 자바 기본 장치
* 단정문(Assertion)
```
// 인자로 boolean으로 평가되는 표현식 또는 값을 받아서 참이면 그냥 지나가고, 거짓이면 AssertionError 예외가 발생
assert expression1;
//표현식1이 거짓인 경우 두번째 표현식의 값이 예외 메세지로 보여지게 됨
assert expression1: expression2;
```
  * 부울식을 포함하고 있는 문장으로서, 프로그래머는 그 문장이 실행될 경우 불리언 식이 참이라고 단언할 수 있다.
  * 개발자가 본인의 코드에서 가정한 사실이 올바른지 검사할 수 있게 해주는 기능
  * 예외 처리가 아니라 검사라는 기능을 제공 - 프로그램이 올바르게 실행되도록 해주는 도구이자 프로그램의 안정성을 높여줄 수 있다.
  * 부울시이 거짓이면 AssertionError 발생
  * 일반적인 컴파일 상황에서는 실행되지 않으므로 -ea 와 같은 옵션을 별도로 줘야함
  * 아래와 같은 상황에서는 사용하면 안된다.
    * public 메서드의 파라미터를 검사하는 경우 - Assertion 으로 검사하기 보다 IllegalArgumentException 을 발생시키자

* Java.util.Objects
* Java.util.Optional
    * Java 8 에서는 Optional<T> 클래스를 사용해서 npe(null point exception) 가 발생하지 않도록 해준다.
    * Optional<T>: null 이 올 수 있는 값을 감싸는 wrapper 클래스, 참조하더라도 npe 가 발생하지 않도록 도와준다.

## null 사용 시 주의점
1. API 에 null 을 최대한 자제해라
   * null 로 지나치게 유연한 메서드를 만들지 말고 명시적인 메서드를 만들어라
   * API에 null 을 받아 분기처리 하지말고 애초에 null 이 있을 때 메서드와, 없을 때 메서드를 나눠서 만들어라
   * null을 반환하지 말고 예외를 던져라
   * 빈 반환값은 빈 컬렉션이나 null 객체를 사용해라

2. 초기화를 명확히 해라
3. null 을 지역적으로 제한하자. 클래스와 메서드를 최대한 작게 만들어 null 의 위험을 줄이자
4. 사전조건과 사후 조건을 확인해라 -> Spring Assert 클래스
