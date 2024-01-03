# ✎ Class
클래스란 유사한 속성과 동작을 가지는 개체의 그룹을 나타내는 코드입니다.
이 것을 실제 생활에 비유하자면 카페라는 클래스가 존재하고, 스타벅스는 하나의 객체를 나타낸다고 볼 수 있습니다.

## Class의 속성
1. 클래스는 실제 개체가 아니다. 즉 어떤 개체들의 청사진 혹은 템플릿이다.
2. 클래스는 메모리를 차지하지 않는다.
3. 클래스는 다양한 데이터 형태의 변수 및 메소드의 그룹이다.
4. 클래스는 다음과 같은 것들이 포함된다.
   - 데이터 멤버 
   - 메소드 
   - 생성자 
   - 중첩 클래스 
   - Interface

## Class의 구성
1. 수정자(접근자): A class can be public or has default access (Refer this for details). ex, public, private
2. 클래스 키워드: class keyword is used to create a class. ex, class
3. 클래스 이름: The name should begin with an initial letter (capitalized by convention). 
4. 슈퍼 클래스(상위 클래스): The name of the class’s parent (superclass), if any, preceded by the keyword extends. A class can only extend (subclass) one parent. 
5. Interfaces(if any): A comma-separated list of interfaces implemented by the class, if any, preceded by the keyword implements. A class can implement more than one interface. 
6. Body: The class body is surrounded by braces, { }.

# ✎ Object
객체는 객체 지향 언어에서 가장 기본이 되는 단위이며, 실제 개체를 나타낸다.

객체를 선언하게 되면 클래스를 인스턴스화 하게 되는데, 이 때 클래스는 메모리 위에 올라가게 된다.

한 클래스에서 나온 모든 객체는 클래스의 속성과 동작을 공유하지만 해당 객체의 상태(변수의 값 등)은 서로 독립적으로 움직인다.

![classAndObject.webp](image%2FclassAndObject%2FclassAndObject.webp)

## Object 생성 방법
1. new 키워드 사용
   ```
   Test t = new Test();
   ```
2. Class.forName(String className) 메소드 사용
   ```
   Test obj = (Test)Class.forName("com.p1.Test").newInstance();
   ```
3. clone() 메소드 사용
   - 단 위 메소드는 기존 객체를 복사할 때 사용할 수 있다.
   ```
   Test t1 = new Test();
   Test t2 = (Test)t1.clone();
   ```
4. Deserialization
   - 위 방법은 파일에 객체를 저장하고, 불러오는 기술이다.
   ```
   FileInputStream file = new FileInputStream(filename);
   ObjectInputStream in = new ObjectInputStream(file);
   Object obj = in.readObject();
   ```

## 익명 객체
익명 개체는 인스턴스화되지만 참조 변수에 저장되지 않는 개체입니다.
- 즉각적인 메소드 호출에 사용된다.
- 메서드 호출 후에는 삭제된다.
- 그들은 다양한 라이브러리에서 널리 사용된다. 예를 들어, AWT 라이브러리에서는 이벤트 캡처(예: 키 누르기)에 대한 일부 작업을 수행하는 데 사용된다.

---
### 참고 자료
- https://www.w3schools.com/java/java_classes.asp
- https://www.geeksforgeeks.org/classes-objects-java/
- https://www.w3schools.com/java/java_modifiers.asp
- https://www.w3schools.com/java/ref_keyword_class.asp
- 