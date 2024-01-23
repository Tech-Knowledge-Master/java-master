# Java 8
Java 8에서 추가된 기능은 크게 Lambda, Default Method, 함수형 인터페이스, Stream, Optional, 날짜 관련 클래스 추가, 병렬 배열 정렬, String Joiner이 있다.

## Lambda 표현식
Lambda를 Java에 들어온 이유는 Java를 좀 더 현대적인 언어로 사용하고, 함수 언어적 특성을 부여하기 위해서이다.

Lambda는 parameter를 받아 값을 반환하는 짧은 코드 블럭을 의미합니다. 람다 식은 메서드와 유사하지만 이름이 없고, 메서드 본문에서 바로 구현할 수 있다.

# 함수형 프로그래밍
함수형 프로그래밍이란 대입문이 없는 코딩을 하는 것을 말한다.

즉 타입과 값을 부여해서 메서드를 작성하는 것이 아니라 선언된 함수를 조합하여 메서드를 작성하고, 선언된 함수 안에 파라미터를 넣어서 작성하는 코딩 형태를 말한다.

## Default Method
인터페이스에서 실제 코드를 구현할 수 있도록 기능을 제공하는 것을 Default Method라고 합니다.

이런 기능이 추가된 이유는 기존에 존재하던 인터페이스를 보완하는 과정에서 기존에 구현했던 인터페이스 및 구현체 클래스를 수정하고, 추가하게 되면서 OCP를 위반하고 기존 코드와 호환성이 많이 떨어지게 되는 것을 막기 위해 추가되었습니다.

## Functional Interface
함수형 인터페이스는 추상 메서드가 단 하나인 인터페이스를 의미합니다.

함수형 인터페이스라서 람다 표현식을 사용할 수 있으며, default method, static method는 여러개 존재해도 상관이 없습니다.

@FunctionalInterface 어노테이션으로 함수형 인터페이스를 표시한다.

## Optional
NPE(NullPointerException) 예외를 피하기 위한 기능으로 추가되었다.

null이 올 수 있는 값을 감싸는 Wrapper 클래스이다.

즉 '결과 없음'을 확실하게 나타내기 위해 메서드의 반환 타입으로 도입된 것이다.

## String Joiner
여러 문자들을 연결할 때 Delimiter(구분자)를 지정해 줄 수 있는 기능이 추가된 것이다.

또한 Prefix, Suffix를 붙일 수도 있다.

### 참고 자료
- https://gogomalibu.tistory.com/97
- https://mangkyu.tistory.com/111
- https://siyoon210.tistory.com/95
- https://velog.io/@heoseungyeon/%EB%94%94%ED%8F%B4%ED%8A%B8-%EB%A9%94%EC%84%9C%EB%93%9CDefault-Method
- https://bcp0109.tistory.com/313
- https://mangkyu.tistory.com/70
- https://futurecreator.github.io/2018/06/02/java-string-joiner/
- 