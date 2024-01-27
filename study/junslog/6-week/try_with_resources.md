### Try-With-Resources에 대해 설명해주세요.

### try-with-resources vs try-catch-finally

```
자원의 사용 이후 자동적으로 자원을 close() 해주는 기능입니다.
JDK 7 이후 추가되었고, 기존의 자원 반납 코드로 인한 코드의 복잡성을 줄이고,
에러나 실수로 인해 자원을 반납하지 못하는 경우를 차단하는 기능입니다.
```

- JDK 7 이전의 try-catch-finally

=> 사용 후 반납해주어야하는 자원들은 Closable I/F를 구현하고 있고,
사용 후 명시적으로 close()를 호출해주어야 한다.

=> JDK 7 이전에는 close()를 호출하기 위해서, try-catch-finally를 이용해서
NULL 검사와 함께 직접 호출해야 했다.

```java
public static void main(String[] args) throws IOException{
    FileInputStream is = null;
    BufferedInputStream bis = null;
    try {
        is = new FileInputStream("file.txt");
        bis = new BufferedInputStream(is);
        int data = -1;
        while((data = bis.read()) != -1){
            System.out.print((char) data);
        }
    } finally {
        // close resources    
        if ( is != null ) is.close();
        if ( bis != null ) bis.close();
    }
}
```

이러한 과정은 다음과 같은 문제를 가지고 있다.
- 자원 반납에 의해 코드가 복잡해진다.
- 작업이 번거롭다.
- 실수로 자원을 반납하지 못하는 경우 발생
- 에러 발생 시 자원을 반납하지 못하는 경우 발생
- 에러 스택 트레이스가 누락되어 디버깅이 어렵다.

try-with-resources 문법을 통해
- 코드를 간결하게 만들 수 있고
- 번거로운 자원 반납 작업을 하지 않아도 되며
- 실수나 에러로 자원을 반납하지 못하는 경우를 방지할 수 있다.
- 또한, 모든 에러에 대한 스택 트레이스를 남길 수 있다.