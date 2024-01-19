# String, StringBuilder, StringBuffer 각각의 차이에 대해 설명해주세요.

String 은 불변이고 StringBuilder 와 StringBuffer 는 가변자료형이다.

String 은 한번 할당된 공간은 변하지 않는다. 반면 StringBuilder 와 StringBuffer 는 객체의 공간이 부족해지는 경우 버퍼의 크기를 유연하게 늘려주어 가변적이다.

---
(1) String 이 문자열 생성, 수정, 삭제 연산에 불리한 이유는?

String 자료형으로 + 연산이나 concat() 메소드를 수행하면 새로운 String 인스턴스를 생성하게 되어 문자열을 많이 결합하면 공간의 낭비가 발생하고 속도도 느려진다. 

→ 문자열을 매번 업데이트 할때마다 메모리 블럭이 추가되어 후에 GC 의 대상이되어 Minor GC 를 일으며 Major GC 를 일으키는 원인이 된다.

반면 StringBuilder 와 StringBuffer 는 가변성을 가지기에 .append() 등의 API 를 이용해 동일 객체내에서 문자열의 크기를 변경할 수 있다. 즉 자체 메모리 블럭에서 늘이고 줄이는 작업이 가능하다.

문자열의 추가, 삭제, 수정이 빈번할 경우 StringBuilder 와 StringBuffer 를 사용하는 것이 더 효율적이다.

(2) StringBuilder 와 StringBuffer 무조건 String 보다 성능이 좋나?

아니다. StringBuilder 와 StringBuffer 를 생성할 경우 버퍼의 크기를 초기에 설정해줘야하는데 이러한 동작으로 인해 무거운 편에 속한다. 그리고 문자열 수정을 할 때도 버퍼의 크기를 늘리고 줄이는 등의 내부적인 연산이 필요하므로 많은 양의 문자열 수정이 아닌 경우 String 을 쓰는 편이 좋다.

(3) StringBuilder 와 StringBuffer는 각각 언제 사용하나?

웹이나 소켓환경과 같이 비동기로 동작하는 경우가 많을 때는 StringBuffer 를 사용하는 것이 안전하다.

(4) StringBuilder 와 StringBuffer, String 누가 더 연산 속도가 빠른가?

String > StringBuffer > StringBuilder (쓰레드의 안정성을 버리고 가변 자료형인 StringBuilder 가 가장 빠르다)
