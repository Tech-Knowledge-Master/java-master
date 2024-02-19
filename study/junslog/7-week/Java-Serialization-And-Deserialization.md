### 자바의 직렬화와 역직렬화에 대해 설명해주세요.

####  [ 0. 직렬화 개념 ]

1) 데이터 직렬화 ( Serialization )
- 메모리 상의 데이터를 디스크에 저장하거나,
  네트워크 통신에 사용하기 위한 형식으로 변환하는 것

2) 데이터 역직렬화 ( De-Serialization )
- 디스크에 저장한 데이터를 읽거나,
  네트워크 통신으로 받은 데이터를 메모리에 쓸 수 있도록 변환하는 것

즉, 직렬화/역직렬화는
데이터를 저장하거나 혹은 통신하기 위해
데이터를 변환하는데 사용하는 것이다.

---

#### [ 1. 직렬화가 필요한 이유 ]

프로그램 Stack 영역 데이터 : 값 형식의 데이터 ( int, float, char )
프로그램 Heap 영역 데이터 : 참조 형식의 데이터
Stack 영역에서 Heap 영역의 실체 데이터를 참조하는
참조 변수를 두는 구조

위 두 가지 데이터 중
디스크에 저장, 통신할 때는 '값' 형식의 데이터만 사용 가능하다.
참조 형식의 데이터 -> 실제 데이터 값이 아닌 힙에 할당된
메모리 번지 주소를 가지고 있기 때문.

#### [ 참조 형식의 데이터를 디스크 저장 / 통신에 사용할 수 없는 이유 ]

CASE1) 메모리 상의 참조 값 데이터를 디스크에 저장

" 프로그램 종료 시, Heap 영역의 주소값이 참조하고 있던
메모리가 해제되어, 참조값을 디스크에 저장하고 다시 불러와도
해당 객체 값을 참조할 수 없다. "

Detail
- 객체 A를 만들고 주소 값이 0x00004123이라고 가정하자.
- 이 값을 파일에 포함해서 저장했다면..
- 이후 프로그램 종료 후,
  다시 실행해서 주소 값 0x00004123을 다시 가져오더라도
  기존 A 객체의 데이터를 가져올 수 없음.
- 프로그램 종료 시, 할당되었던 메모리(0x00004123)은 해제되고
  없어지기 때문

CASE2) 네트워크 통신 시

" 각 PC마다 메모리 주소값이 다르기에,
네트워크를 통해 전송된 어떤 PC의 주소값이
수신자 PC의 주소값과 매핑이 되지 않기 때문이다. "

- 각 PC마다 사용하고 있는 메모리 공간 주소는 전혀 다르고,
  각자 상관관계가 없다.
- 내가 다른 PC로 전송한
  A객체 데이터의 참조값(0x00004123)은 무의미하다.
- 해당 데이터를 받은 PC의 메모리 주소 0x00004123에는
  전혀 다른 값이 존재하기 때문이다.


#### [ 직렬화를 하는 이유 ]

" 사용하고 있는 데이터를
파일 저장 혹은 데이터 통신에서
파싱할 수 있는 유의미한 데이터로 만들기 위함이다. "


- 디스크에 저장, 네트워크와 통신할 때
  값 형식의 데이터만 가능하고 참조형식의 데이터는 안된다.

- 직렬화 시,
  각 주소 값이 가지는 데이터를 전부 끌어 모아
  값 형식의 데이터로 변환해준다.

- 직렬화된 데이터는 언어에 따라
  텍스트 or 바이너리 형태가 된다.
  이러한 형태로 저장/통신할 때 파싱이 가능한 유의미한 데이터가 됨.

---

#### [ 데이터 직렬화의 종류 ]

1. CSV, XML, JSON 직렬화
- 사람이 읽을 수 있는 형태
- 저장 공간의 효율성이 떨어지고, 파싱하는 시간이 오래 걸린다.
- 데이터 양이 적을 때 주로 사용
- 최근에는 JSON 형태로 데이터 직렬화를 많이 한다.

2. Binary 직렬화
- 사람이 읽을 수 없는 형태
- 저장 공간을 효율적으로 사용 가능, 파싱하는 시간이 빠르다.
- 데이터 양이 많을 때 주로 사용
- 모든 시스템에서 사용 가능
  ex) Protocol Buffer, Apache Avro

3. Java 직렬화
- Java System 간의 데이터 교환이 필요할 때 사용

---

#### Java 직렬화와 역직렬화

< Java 직렬화 >
: 자바 시스템 내부에서 사용되는 객체, 데이터를
외부 자바 시스템에서도 사용할 수 있도록
Byte 형태의 데이터로 변환하는 기술

- JVM의 메모리에 상주(Heap or Stack)되어 있는 객체 데이터를
  바이트 형태로 변환하는 기술

< Java 역직렬화 >
: Byte형태로 변환된 데이터를 다시 객체로 변환하는 기술

- 직렬화된 바이트 형태의 데이터를 객체로 변환해서
  JVM으로 상주시키는 기술

---

#### Java 직렬화

- 직렬화 조건
  java.io.Serializable I/F를 구현해야 한다.
  [ 코드 1 참조 ]

- 직렬화 방법
  java.io.ObjectOutputStream 을 사용하여 직렬화를 진행한다.
  [ 코드 2 참조 ]
<br><br>

#### 직렬화와 자바 직렬화
![Serialization_And_Java_Serialization.png](img%2FSerialization_And_Java_Serialization.png)

[ 코드 1 - java.io.Serializable I/F 구현 ]
```java
public class Member implements Serializable {
private String name;
private String email;
private int age;

		public Member(String name, String email, int age){
				this.name = name;
				this.email = email;
				this.age = age;
		}

		@Override
		public String toString() {
				return String.format("Member{name='%s', email='%s', age='%s'}", name, email,age);
		}
}
```

**[ 코드 2 - java.io.ObjectOutputStream 사용하여 직렬화 ]**

```java
public static void main(String[] args) {
    Member member = new Member("김배민", "deliverykim@baemin.com", 25);
    byte[] serializedMember;
    try(ByteArrayOutputStream baos = new ByteArrayOutputStream()){
        try(ObjectOutputStream oos = new ObjectOutputStream(baos)){
            oos.writeObject(member);
            // serializedMember -> 직렬화된 MEMBER 객체
            serializedMember = baos.toByteArray();
        }
    }
    // byte 배열로 생성된 직렬화 데이터를 base64로 변환
    System.out.println(Base64.getEncoder().encodeToString(serializedMember));
}
```

#### 역직렬화

- 조건
  - 직렬화 대상이 된 객체의 클래스가
    **Class Path**에 존재 & **import 되어 있어야 한다**.
    - Class Path
    - JVM의 Class Loader가
      Class 파일을 찾는 데 기준이 되는 파일 경로
  - Java 직렬화 대상 객체는 동일한 `serialVersionID`를 가지고 있어야 한다.

- 방법
  - `java.io.ObjectInputStream` 을 사용하여 역직렬화

```java
public static void mai(String[] args){
		// 직렬화 예제에서 생성된 Base64 데이터
		String base64Member = "...직렬화 객체 내용";
		byte[] serializedMember = Base64.getDecoder().decode(base64Member);
		try (ByteArrayInputStream bais = new ByteArrayInputStream(serializedMember)){
				try(ObjectInputStream ois = new ObjectInputStream(bais)){
						// 역직렬화된 Member 객체를 읽어오기.
						Object objectMember = ois.readObject();
						Member member = (Member) objectMember;
						System.out.println(member);
				}
		}
}
```

#### Java에서도 CSV, JSON을 쓰면 되는데, Java Serialization을 사용하는 이유?

- 직렬화의 장,단점을 알고 상황에 맞게 사용할 줄 알아야 한다!

[ Java 직렬화의 장점 ]

- 자바 직렬화는 자바 시스템에서 개발에 최적화되어 있음.
- 복잡한 데이터 구조의 클래스의 객체라도, 직렬화 기본 조건만 지키면
  큰 작업 없이 바로 직렬화, 역직렬화가 가능하다.
- 데이터 타입이 자동으로 맞춰진다.
  역직렬화가 되면 기존 객체처럼 바로 사용 가능.

[ Java 역직렬화의 단점 ]

- **< 클래스 구조 변경과 직렬화/역직렬화 호환성 >**

클래스 구조가 변경되면,
구조 변경 이전에 직렬화된 객체의 역직렬화 시 문제가 발생한다.
- 이러한 케이스 발생 시
`**java.io.InvalidClassException**` 발생
- 각 자바 시스템에서 사용하고 있는 모델의 버전 차이로 인해 발생하는 문제
- 이를 해결하기 위해, 모델의 버전 간 호환성을 유지하는
`**SUID(serialVersionUID)**`를 정의해야 함.

- **< 너무 엄격한 타입 체크 >**
  객체의 변수 명이 같지만, 변수 타입이 달라지면
  Type Exception 이 발생함.

String → StringBuilder로 변경 시 역직렬화 불가능.
int → long으로 변경 시 역직렬화 불가능.

- <용량 문제>

  아주 간단한 객체의 내용도 2배 이상의 용량 차이가 발생
  JSON과 같은 경량화된 형태로 직렬화하는 것도 좋다.