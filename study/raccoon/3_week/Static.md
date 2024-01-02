# ✎ Static
Static 키워드를 통해 생성된 정적멤버들은 Heap영역이 아닌 Static영역에 할당된다.
![static.png](image%2Fstatic%2Fstatic.png)
Static 영역에 할당된 메모리는 모든 객체가 공유하여 하나의 멤버를 어디서든지 참조할 수 있는 장점을 가지지만 Garbage Collector의 관리 영역 밖에 존재하기에 Static영역에 있는 멤버들은 프로그램의 종료시까지 메모리가 할당된 채로 존재하게 된다.

static을 사용하는 또 다른 이유는 값을 공유할 수 있기 때문이다.
static으로 설정하면 같은 **메모리 주소**만을 바라보기 때문에 static 변수의 값을 공유하게 되는 것이다.
![static memory.png](image%2Fstatic%2Fstatic%20memory.png)
### 참고 자료
- https://coding-factory.tistory.com/524
- https://wikidocs.net/228
- https://velog.io/@yonii/JAVA-Static%EC%9D%B4%EB%9E%80