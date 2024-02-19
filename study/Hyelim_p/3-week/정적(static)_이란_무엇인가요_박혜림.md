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

1. 인스턴스를 생성
2. 변수에 인스턴스 주소 값 대입

위의 순서로 이루어져야하지만 컴파일러 최적화에 의해 위의 순서가 바뀌면 인스턴스가 완전히 생성되지 않은 시점에 주소값이 대입된다.
이때 다른 스레드가 진입하면 instance 가 null 이 아니기 때문에 초기화되었다 가정하고 그 다음 단계로 진행한다.

이 문제는 자바 1.5 이상부터 volatile 로 해결이 가능하다. - volatile 이 붙은 변수는 컴파일 시 재배치를 하지 않도독 함

하지만, volatile 변수들은 램에서 처리하기 때문에 접근 비용(read/write 처리)이 비싸다.
>  ### volatile
>   * cpu 메모리 영역에 캐싱된 값이 아닌 항상 최신의 값을 가지도록 램에서 값을 참조하도록 하는 키워드
>   * 모든 시점에 모든 스레드가 동일한 값을 가지도록 동기화함
>   * 캐시 없이 최신의 값을 보게 함 <br/>
>  https://jronin.tistory.com/110 -> 참고해 volatile 추가 공부 필요
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

> 내부 클래스<br/>
내부 클래스가 외부 클래스의 멤버를 가져와 사용하는 경우가 아닌 경우 반드시 내부 클래스를 선언할 때는 static 키워드를 붙여주자, 그렇지 않으면 내부 클래스에서 외부 클래스를 참조하고 있는 관계 때문에, 메모리 누수 문제가 발생한다.



