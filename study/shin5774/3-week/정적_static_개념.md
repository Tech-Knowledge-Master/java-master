# 정적(static)이란 무엇인가요?

## 답변

```
정적이라는 의미를 가진 키워드로 이를 이용하여 정적 변수 및 메서드를 만들수 있습니다.
정적 메서드와 정적변수는 프로그램 시작시 생성되어 프로그램 종료전까지 사용할수 있습니다.
이 둘은 원래 method 영역에 저장되어 있기에 GC의 대상이 아니었지만
자바 8버전 이후로는 Heap영역에 저장되기에 GC의 대상이 되었습니다.
```

### 예상 꼬리 질문 및 답변

- 왜 Heap영역으로 저장위치가 바뀌었는지 설명해주세요.
  - 답

    8버전 이전에는 permanent 영역이라는 method 영역의 일부에 저장이 되었는데 이러다 보니 OOM 문제가 발생하였습니다.
  
    이를 막기 위해 8버전부터 permanent영역을 대체하는 metaspace영역이 나오게 되었고 그와 동시에 static object는 Heap 메모리에 저장되는 것으로 변경되었습니다.
- static은 어디에 사용되나요?
  - 답

    주로 final 키워드와 같이 사용되어 상수를 선언하는데 사용되고 프로그램에 한개만 존재한다는 특성을 이용하여 싱글톤 패턴의 구현에 사용됩니다.
## 내용 정리
- 정적(Static)

  정적이라는 의미를 가진 키워드로 이를 사용해 정적 변수 및 정적 메서드를 만들수 있다.

  이들은 프로그램과 생명주기를 같이하며, 8버전 이전까진 GC의 대상이 아니었다.

  하지만 8버전부터는 GC의 대상이 되었다.
  - 8버전의 변화점
  
    permanent영역이 사라지고 metaspace영역이 이를 대신한다.
    - permanent 영역
  
      Method 영역에 사용된 공간으로 class metadata,inherited String,class static variable을 저장한다.
    
      자바 8버전이후 사라지고 이러한 역할이 metaspace 영역으로 넘어갔다.
    
      기존에 저장하고 있던 데이터 중 class metadata는 native memory 영역으로, inherited String,class static은 Heap 영역으로 옮겨졌다.
  
    - metaspace 영역
    
      자바 8버전부터 생긴 영역. native memory의 영역으로 os에서 관리한다.
    - 왜 바뀌었는가?
    
      기존의 permanent 영역에 static object가 저장되었는데 이때 collection 형태로 static object가 생성된 후 collection에 객체가 추가되는 경우,
    
      메모리가 부족하여 OOM(Out Of Memory)문제가 발생할수 있었다.
      
      따라서 이러한 문제를 해결하기 위해 metaspace 영역을 새로 만들면서 동시에 static object를 Heap영역에 저장되게 함으로 GC의 대상이 되게 함으로 이 문제를 해결하였다.
  - 활용성
    - 상수
    
      final 키워드와 같이 사용해서 프로그램 중 값이 변하지 않는 상수를 설정하는데 사용함.
    - 싱글톤 패턴
  
      프로그램 실행 중 한개만 존재한다는 특성을 활용하여 객체 한개로만 번갈아가면서 사용하는 싱글톤 패턴을 구현하는데 사용된다.