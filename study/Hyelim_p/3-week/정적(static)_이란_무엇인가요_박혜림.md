# 정적(static)이란 무엇인가요?
<답변의 키포인트>
> 클래스 내부에서 공유되는 변수 혹은 메서드를 의미한다. JVM의 클래스 로더에 의해 MetaSpace 영역의 Method Area에 로드된다. 다른 메모리 구역에서 전역으로 참조할 수 있다.
> 
클래스로더가 클래스를 로딩해서 method 메모리 영역에 적재될때 클래스별로 관리된다. 따라서 클래스의 로딩이 끝나는 즉시 바로 사용이 가능하다.

Java 8 이후 Permgen 영역이 사라지고 Metaspace 영역으로 바뀌어 static object 의 레퍼런스는 Metaspace 에 실제 객체는 Heap 에 존재한다.

static 을 많이 사용할 경우에는 메모리 누수가 많이 발생할 수 있으며, 멀티스레드 환경에서 공유 변수의 문제점등이 발생할 수 있다.

---



### 싱글톤 패턴

싱글톤 패턴은 특정 클래스의 인스턴스가 하나만 생성되도록 하는 디자인 패턴인데, 이 때 싱글톤 인스턴스를 저장하기 위한 정적 필드가 사용된다.
싱글톤 패턴을 이용하면 객체 생성 비용이 절감되며 유일객체가 보장되고 공유하기에도 용이하다. 유일객체가 공유되기에 객체의 자원이 무분별한 곳에서
변경될 여지가 다분하다. 따라서 싱글톤 패턴은 무분별하게 사용하기 보다 불변성필드와 메서드위주로 제공하는 객체로 사용해야한다.


```java
public class Singleton {

    private static Singleton instance = new Singleton();
    
    private Singleton() {
        // 생성자는 외부에서 호출못하게 private 으로 지정해야 한다.
    }

    public static Singleton getInstance() {
        return instance;
    }

    public void say() {
        System.out.println("hi, there");
    }
}
```
위 객체는 클래스를 런타임 메모리에 올리때 클래스의 static 필드도 초기화 해서 올리기 때문에 의도치 않게 객체의 수명 시간이 길어진다.
이를 해결하기 위해 지연로딩을 사용해 객체를 실제 사용하는 시점에 생성하도록 할 수 있다.

### 문제점: 병렬 스레드 에서의 싱글톤 패턴의 문제
```java
public class MyExample {
    private static MyExample INSTANCE; 
    
    private MyExample() {}

    public static MyExample getInstance() {
        if (INSTANCE == null) {
            INSTANCE = new MyExample(); 
        }
        
        return INSTANCE;
    }
}
```

### 문제점: 동기화 오랜 시간 소요
```java
public class MyExample {
    private static MyExample INSTANCE;
    
    private MyExample() {}

    public static synchronized MyExample getInstance() {
        if (INSTANCE == null) {
            INSTANCE = new MyExample();
        }
        
        return INSTANCE;
    }
}
```

### 문제점: 더블체크 락킹기술 초기화 오류의 문제
```Java
public class MyExample {
  private static MyExample INSTANCE;

  private MyExample() {}

  public static MyExample getInstance() {
    if (INSTANCE == null) {
      synchronized (MyExample.class) {
        if (INSTANCE == null) {
          INSTANCE = new MyExample();
        }
      }
    }
    return INSTANCE;
  }
}
```
컴파일러에 따라 초기화가 제대로 이루어지지 않을 수도 있다. -> Volatile 사용

### 결론: 클래스 로드 타임 이용
클래스 로드 타임을 이용하면 동시성이 보장된다. 이를 활용하면 지연로딩과 스레드 세이프를 지킬 수 있다.
```java
public class MyExample {
    
    private MyExample() {}

    private static class Holder {
        private static final MyExample INSTANCE = new MyExample();
    }
    
    public static MyExample getInstance() {
        return Holder.INSTANCE; 
    }
}

```
INSTANCE는 Holder라는 클래스가 메모리에 올라갈 때 초기화된다.



