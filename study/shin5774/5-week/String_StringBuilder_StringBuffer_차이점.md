# String, StringBuilder, StringBuffer 각각의 차이에 대해 설명해주세요.

## 답변

```
String은 객체가 한번 생성이 되면 이후 크기가 변하지 않는 불변객체입니다. 값의 추가나 concat 메서드의 경우도 새로운 객체를 만들어서 이를 참조하는것입니다.
StringBuffer와 StringBuilder는 String과 다르게 메모리 공간이 가변적입니다. 여기서 StrnigBuffer는 멀티스레드 환경에서의 동기화를 보장하고 StringBuilder는 동기화를 보장하지 않습니다. 이로 인해 StringBuilder가 더 성능이 좋습니다.
따라서 String의 경우는 단순하게 값을 읽는경우에 사용하면좋고 StringBuilder와 StringBuffer의 경우는 문자열이 계속해서 변하는경우에 사용하기 좋습니다. 이후에는 멀티쓰레드 환경의 유무에 따라 각각 사용하면 좋을것 같습니다.
```

## 내용 정리

- String

  immutable,즉 불변이다. 따라서 String 객체는 한번 생성되면 할당된 메모리 공간은 변하지 않는다.

  (+,concat 연산도 사실 더해진 새로운 객체를 만들어내는것)

  (+ 연산에서 실제 컴파일전 내부적으로 StringBuilder 클래스로 변환이 되어진다.

  ⇒ hello + world →

  new StringBuilder(”hello”).append(”world).toString()

  )

  따라서 문자열 연산이 많은 경우에는 성능이 좋지않다. 하지만 간단하게 사용가능하고 동기화 걱정이 없기에 안전하다.

- StringBuffer / StringBuilder

  메모리 공간이 mutable하다. 두 클래스간의 차이점은 멀티스레드 환경에서의 동기화 여부이다.

  StringBuffer가 동기화를 보장하고 StringBuilder가 동기화를 보장하지 않는다.

  ⇒ 단일 스레드 환경이라면 StringBuilder를 사용하고 멀티스레드 환경이라면 StringBuffer를 사용하는게 성능적으로 좋다.

- 각 자료형의 적절한 사용 시기

  String: 단순하게 읽는 조회연산

  StringBuffer/StringBuilder: 문자열 추가 및 변경등의 작업이 많은 경우.