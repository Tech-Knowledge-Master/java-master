# ☕︎ Identity and Equality
## 동일성 (Identity)
동일성은 비교 대상의 두 객체가 같은 힙 메모리를 참고하고 있다는 것을 의미한다.

Java에서는 동일성은 '=='으로 확인한다. 

## 동등성 (Equality)
동등성은 비교 대상인 두 객체가 논리적으로 같은 값을 가지는 것을 의미한다.

Java에서는 equals와 HashCode를 오버라이드 받은 후 equals로 동등성을 확인할 수 있다.

equals 함수를 재 정의 해야 하는데 그 코드는 다음과 같다.

여기서 hashCode를 같이 오버라이드 하는 이유는 자바에서 객체 주소값을 접근하는 메서드는 hashCode로 접근하기 때문이다.

또한 Java에서는 equals의 결과로 True가 나온 두 객체는 반드시 hashCode가 같아야 한다.

그 이유는 set같은 논리적으로 중복을 허용하지 않는 자료구조의 동등성 비교가 다음과 같기 때문이다.

![equals and hashCode.png](identityEqualsImages%2Fequals%20and%20hashCode.png)

```
@Override
public boolean equals(Object o) {
    if (this == o) {
        return true;
    } // 논리적으로 데이터 내용이 같은지를 확인하는 코드
    if (o == null || getClass() != o.getClass()) {
        return false;
    } // 클래스가 다르거나 비교하는 대상의 참조값이 null인지 확인하고, 맞다면 false리턴
    Number number1 = (Number) o; // 첫번째 코드에서 false를 리턴하였으므로, 참조 타입을 casting하여 재 정의 하고, 논리적으로 같은 값을 가리키는지 확인한다.
    return number == number1.number;
}
@Override
public int hashCode() {
    return Objects.hash(number);
}
```

### Identity vs Equality
위 내용에 따라서 동일성은 동등성을 보장하지만 동등성은 동일성을 보장하지 않는다.

### 참고 자료
- https://hudi.blog/identity-vs-equality/
- https://choiblack.tistory.com/43
- https://mangkyu.tistory.com/101
- https://inpa.tistory.com/entry/JAVA-%E2%98%95-equals-hashCode-%EB%A9%94%EC%84%9C%EB%93%9C-%EA%B0%9C%EB%85%90-%ED%99%9C%EC%9A%A9-%ED%8C%8C%ED%97%A4%EC%B9%98%EA%B8%B0
