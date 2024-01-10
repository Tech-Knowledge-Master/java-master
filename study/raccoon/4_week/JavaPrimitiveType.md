# ✎ Java Primitive Type
**Primitive Type** : 실제 데이터를 저장하는 타입

**Reference Type** : 객체의 주소를 저장하는 타입으로 메모리 주소를 통해 객체를 참조하는 타입

| 종류   | 데이터 형   | 크기 (byte / bit) | 표현 범위                                                      |
|------|---------|-----------------|------------------------------------------------------------|
| 논리형  | boolean | 1 / 8           | true or false                                              |  
| 문자형  | char    | 2 / 16          | '\u0000' ~ 'uFFFF' (16비트 유니코드 문자 데이터)                      |
| 정수형  | byte    | 1 / 8           | -128 ~ 127                                                 |
| 정수형  | short   | 2 / 16          | -32768 ~ 32767                                             |
| 정수형  | int     | 4 / 32          | -2147483648 ~ 2147483647( -21억 ~ + 21억)                    |
| 정수형  | long    | 8 / 64          | -9223372036854775808 ~ 9223372036854775807(-100경 ~ + 100경) |
| 실수형  | float   | 4 / 32          | 1.4E-45 ~ 3.4028235E38                                     |
| 실수형  | double  | 8 / 64          | 4.9E-324 ~ 1.7976931348623157E308                          |

## Primitive Type

### boolean
- stored in a single bit
- but, java stored in a single byte
- **default value** = false
### char
- 범위
  - 0 ~ 65,535
  - \u0000 ~ \uffff
- **default value** = \u0000
### byte
- 범위
  - -128 ~ 127
  - stored in 8 bits memory
- **default value** = 0
### short
- 범위
  - stored in 16 bits in memory
  - -2<sup>15</sup> ~ 2<sup>^15</sup> - 1
- **default value** = 0
### int
- 범위
  - stored in 32 bits in memory
  - -2<sup>31</sup> ~ 2<sup>^31</sup> - 1
  - java 8에서 새로운 함수를 사용하여 양수인 정수만 할당한 경우 **2<sup>^32</sup> - 1** 까지 저장할 수 있다.
- **default value** = 0
### long
- 범위
  - stored in 64 bits in memory
  - -2<sup>63</sup> ~ 2<sup>^63</sup> - 1
- **default value** = 0
  - 마지막에 f를 붙여야 한다. 기본이 double이기 때문이다.
### float
- 범위
  - stored in 32 bits in memory
  - 1.4 x 10<sup> -45 </sup> ~ 3.4 x 10<sup> 38 </sup>
- **default value** = 0.0
### double
- 범위
  - stored in 64 bits in memory
  - 4.9 x 10 <sup> -324 </sup> ~ 1.7 x 10 <sup> 308 </sup>
- **default value** = 0.0
  - 마지막에 D를 붙여서 표시한다.

## Reference Type
참조타입은 원시타입을 제외한 타입들(문자열, 배열, 열거, 클래스, 인터페이스)을 말한다.

따라서 참조타입은 Stack memory에 메모리 주소를 저장되고, 실제 데이터는 Heap memory에 저장된다.

이렇게 참조를 하고 있기 때문에, **Boxing/Unboxing**을 통해 실제 타입을 지정해줘야 한다.

하지만 Java 1.5 이후로 **Auto Boxing/Auto Unboxing**을 적용하여 명시적으로 지정하지 않아도 컴파일러가 자동적으로 지정하는 방법을 말한다.

## Primitive Type vs Reference Type
1. Null 포함 가능 여부
   - Primitive Type은 적용 불가
   - Reference Type은 적용 가능
2. Generic Type에서 사용 가능 여부
   - Primitive Type은 사용 불가
   - Reference Type은 사용 가능


### ✓ 참고 자료
- https://velog.io/@gillog/%EC%9B%90%EC%8B%9C%ED%83%80%EC%9E%85-%EC%B0%B8%EC%A1%B0%ED%83%80%EC%9E%85Primitive-Type-Reference-Type
- https://blog.yevgnenll.me/posts/java-primitive-type
- https://terry9611.tistory.com/65