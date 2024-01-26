# try-with-resource에 대해서 설명해주세요.
- try-with-resources는 Java 7부터 도입된 기능으로, 
리소스를 사용한 작업을 수행한 후에 해당 리소스를 명시적으로 닫아주는 코드를 간편하게 작성할 수 있도록 도와주는 구문
- 주로 입출력 스트림, 네트워크 연결, 데이터베이스 연결과 같이 명시적으로 닫아줘야 하는 리소스를 다룰 때 사용된다.

- 기존에 Closable 에 부모 인터페이스로 AutoCloseable 를 추가하여, 변경작업에 대한 수고로움을 덜고 하위 호환성을 완벽하게 달성했다.
- AutoCloseable 인터페이스를 구현하고 있는 자원에 대해 try-with-resources를 적용 가능하도록 하였고, 
이를 사용함으로써 코드가 유연해지고, 누락되는 에러없이 모든 에러를 잡을 수 있게 되었다.
- try-with-resources 구문은 try 키워드 뒤에 소괄호 () 안에 리소스를 선언하는 방식으로 구성된다.
이 try 블록이 끝날때 자동으로 close() 가 호출되어 리소스를 닫게 된다.

### 사용예시
```java
public class TryWithResourcesExample {
    public static void main(String[] args) {
        try (BufferedReader reader = new BufferedReader(new FileReader("example.txt"))) {
            String line = reader.readLine();
            System.out.println(line);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
### 사용하는 이유
- 이 구문을 사용하면 명시적인 finally 블록이나 리소스 해제 코드를 작성하지 않아도 되므로 코드가 간결해지고 가독성이 향상된다.
- try-catch-finally 사용시 자원 반납에 의해 코드가 복잡해지고, 작업이 번거롭고, 자원 반납이 누락될 수도 있고, 에러 스택 트레이스라 누락될 수도 있다.
