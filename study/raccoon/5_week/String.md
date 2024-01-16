# String
Java에서 String Type은 기본적으로 참조 타입(Reference Type)이라고 정의합니다..

따라서 stack 영역에서 데이터가 조합되는 것이 아니라 Heap 영역에서 데이터가 조합되는 것이빈다.

기본적으로 Java에서 String 객체는 불변이다.

1. JVM에서 따로 String Constant Pool이라는 독립적인 메모리 영역을 만들고, 이들을 상수화 하여 공유하는데 이 과정에서 Data Cashing이 일어나고 그 성능적 이득을 가질 수 있다.
2. Multi-Thread에서 데이터 안전을 보장할 수 있다.
3. Security를 보장할 수 있다.

## String 객체 생성 방식
1. String Literal방식
   ```
   String name = "raccoon";
   ```
   - 이렇게 생성된 String은 위에서 말한 String Constants Pool이라는 Data 메모리 영역에 저장된다. 따라서 GC의 영향을 받지 않게 되고, 같은 논리적 내용을 가지고 있는 새로운 String 객체를 생성해도 같은 메모리를 참고하게 된다.
2. String Object방식
   ```
   String name = new String("raccoon");
   ```
   - 위 방식은 객체를 만든 것이기 때문에 Heap 메모리에 저장되고 같은 논리적 내용이더라도 다른 객체이므로, 서로 다른 메모리를 참고하게 된다.

### Intern method
intern 메소드가 실행되면 이 객체가 String Constants Pool에 존재하는지 확인하고, 만약 없다면 새로 만든 후 그 메모리를 가지는 객체로 리턴하게 된다.

그래서 intern()을 사용하면 equals를 사용하지 않아도 문자열 비교가 가능하다.

```
String name1 = new String("raccoon"); // new 연산자를 이용한 방식
String name2 = "raccoon";

name1 = name1.intern();
if(name1 == name2) {
   return true;
} // true가 리턴된다.
```

## String Builder
그럼 자바에서 문자열은 불변으로 관리된다. 그러나 코드를 짜다보면 가변하는 문자열이 필요하게 되는데, 무작정 '+' 연산으로 문자열을 더하게 되면 중간 문자열이 너무 많이 생성되어 너무나 많은 데이터리르 차지하게 된다.

이런 문제를 해결하고자 가변 문자열을 만들어 주는 것이 String Builder이다.

가변 방식은 vector의 동작방식과 거의 비슷하다.

```
StringBuilder stringBuilder = new StringBuilder();
stringBuilder.append("raccoon").append("닉네임");
System.out.println(stringBuilder);
System.out.println(stringBuilder.toString());
```

## String Buffer
String Builder는 멀티 쓰레드 환경에서 데이터 안전하지 않다.

그 대신 String Buffer는 멀티 쓰레드 환경에서 데이터 안전을 보장하는 대신 성능은 String Builder보다 떨어진다.

### 참고 자료
- https://inpa.tistory.com/entry/JAVA-%E2%98%95-String-%ED%83%80%EC%9E%85-%ED%95%9C-%EB%88%88%EC%97%90-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-String-Pool-%EB%AC%B8%EC%9E%90%EC%97%B4-%EB%B9%84%EA%B5%90
- https://onlyfor-me-blog.tistory.com/317
- https://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html
- https://steady-coding.tistory.com/569
