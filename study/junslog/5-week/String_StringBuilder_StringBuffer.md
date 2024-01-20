## String, StringBuilder, StringBuffer 각각의 차이에 대해 설명해주세요.

String은 불변(Immutable)이지만

StringBuilder는 Mutable 하다.

문자열, 숫자, char 등을 concat할 때는 StringBuffer, StringBuilder를 사용할 수 있다.

단, 복잡한 경우 의미가 있고, 단순한 경우에는 굳이 StringBuffer, StringBuilder를 쓰지 않고 + 연산자를 활용해서 직접 합치면 됨.

String 객체는 한번 생성되면 할당된 메모리 공간이 변하지 않는다.
`+` 연산자나 concat 메서드를 통해 기존에 생성된 String 클래스 객체 문자열에 다른 문자열을 붙여도
기존 문자열에 새로운 문자열을 붙이는 것이 아니라, 새로운 String 객체를 만든다.


StringBuffer와 StringBuilder는 Mutable하다는 점에서 비슷하지만,
둘의 차이는 '동기화' 여부이다.

StringBuffer 클래스는 각 메서드별로 Synchronized Keyword가 존재하여,
멀티스레드 환경에서도 동기화를 지원한다.

반면, StringBuilder는 동기화를 보장하지 않는다.

따라서, 멀티스레드 환경이라면 값 동기화 보장을 위해 StringBuffer를 사용하고

단일스레드 환경이라면 StringBuilder를 사용하는 것이 좋다.