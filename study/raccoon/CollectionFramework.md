# ✎ Collection Framework
컬렉션 프레임워크란 다수의 데이터를 하나의 그룹으로 묶어 효율적으로 저장하고, 관리하는 자료 구조를 의미한다.

자바에서 제공하는 컬렉션 프레임워크는 JDK 1.2부터 ```java.util```에서 제공하기 시작했다.
![CollectionFramework Tree.png](image%2FCollectionFramework%2FCollectionFramework%20Tree.png)
## ✎ 컬렉션 프레임워크의 목적
자바 공식 홈페이지에서 밝히는 컬렉션 프레임워크의 목적은 통일성이다.(```The main design goal was to produce an API that was small in size and, more importantly, in "conceptual weight."```)

우리가 흔히 사용하는 Vector 자료구조를 예를 들어서 생각해보자.

1. 개발자 A가 Vector 자료구조를 사용하기 위해 구현 파일(VectorFromA.java)을 만들어서 사용함
2. 개발자 B가 Vector 자료구조를 사용하기 위해 구현 파일(VectorFromB.java)를 만들어서 사용함.

**위의 경우에서 B의 벡터 구현체가 알고리즘적으로 최적화가 되어있다고 가정하자.**

이런 경우 같은 벡터 자료구조를 사용하지만, A개발자가 구현한 벡터 자료구조로 작동하게 된다면 쓸데 없는 리소스 손실이 발생하게 된다.

이런 상황을 방지하기 위해 자바에서 자체적으로 고성능의 자료구조 및 C(Create)R(Read)U(Update)D(Delete)의 기능을 제공하여 개발자의 개발 밀도를 높이는 동시에 코드 재사용율을 높여서 코드를 더 읽기 쉽게 되었다.

## ✎ 컬렉션 프레임워크의 장점
- List, Queue, Set, Map 등의 인터페이스를 제공하고, 이를 구현하는 클래스를 제공하여 일관된 API 를 사용할 수 있다. 
- 가변적인 저장 공간을 제공한다. 고정적인 저장 공간을 제공하는 배열에 대비되는 특징이다. 
- 자료구조, 알고리즘을 구현하기 위한 코드를 직접 작성할 필요 없이, 이미 구현된 컬렉션 클래스를 목적에 맞게 선택하여 사용하면 된다. 
- 제공되는 API 의 코드는 검증되었으며, 고도로 최적화 되어있다.
- 또한 자바에서 제공하는 동시성에 대해 다양한 메소드를 제공하여 다양한 상황에 맞는 메소드를 가져다 사용할 수 있다.

### 컬렉션 프레임워크의 구성요소

**컬렉션 프레임워크는 총 11개의 요소로 구성되어있지만 크게 3가지로 구성된다.**

1. **인터페이스 (Interfaces)** : 각 컬렉션을 나타내는 추상 데이터에 대한 인터페이스 (List, Set, Map 등). 클래스는 이 인터페이스를 구현하는 방식으로 작성되었기 때문에 상세 동작은 달라도 일관된 조작법으로 사용할 수 있다.
2. **클래스 (Classes)** : 컬렉션 별 인터페이스의 구현 (Implementation). 위에서 언급했듯, 같은 List 컬렉션이더라도 목적에 따라 ArrayList, LinkedList 등으로 상세 구현이 달라질 수 있다.
3. **알고리즘 (Algorithms)** : 컬렉션이 제공하는 연산, 검색, 정렬, 셔플 (Shuffle) 등에 대한 메소드.

## Iterable
반복자라는 뜻이며, 표준화된 컬렉션에 저장된 요소를 읽어오는 방법이다.

이 상위 인터페이스는 인덱스가 없는 객체거나, 반복하면서 내부 객체를 수정, 제거할 때 유용하게 사용할 수 있는 자료구조이다.

List, ArrayList, Stack, Quque, LinkedList 등이 상속받고있다. 즉, 이러한 컬렉션 인터페이스를 상속받는 클래스들에 대해 Iterator 인터페이스 사용이 가능하다.

### ✎ List
List의 종류에는 크게 ArrayList, LinkedList가 존재한다.

- **Array List**

ArrayList는 Vector의 개선해서 구현한 api이므로, 구현원리나 기능이 매우 흡사하다.

Object 객체를 상속받아 배열을 만들어 생성하지만 vector의 동적 기능과 흡사하게 기존 크기의 1.5배만큼 커진 배열을 할당하고, 복사하는 방식으로 동적으로 메모리를 관리한다.

- **Linked List**
![LinkedList.png](image%2FCollectionFramework%2FLinkedList.png)
LinkedList는 자료구조에서 나온 그 Linked List이다.

LinkedList는 추가, 삽입, 제거가 많이 호출될 때 사용하면 좋은 자료구조이다. 

### ✎ Queue
**Queue**

Queue에는 크게 **Queue**와 **DeQue**가 존재한다.

**Queue**라는 자료구조는 **FIFO**(First In First Out)의 구조를 가지는 자료구조이다.

**Deque**는 Double-Ended Queue의 줄임말로 큐의 양쪽에서 데이터를 삽입과 삭제를 할 수 있는 자료구조를 의미합니다
![Deque.png](image%2FcollectionFramework%2FDeque.png)
- **Priority Queue**

일반적인 큐의 구조 FIFO(First In First Out)를 가지면서, 데이터가 들어온 순서대로 데이터가 나가는 것이 아닌 우선순위를 먼저 결정하고 그 우선순위가 높은 데이터가 먼저 나가는 자료구조이다.
    
    1. 높은 우선순위의 요소를 먼저 꺼내서 처리하는 구조이다.
    2. 우선순위 큐에 들어가는 원소는 비교가 가능한 기준이 있어야한다.
    3. 내부 요소는 힙으로 구성되어 이진트리 구조로 이루어져 있다.
    4. 내부구조가 힙으로 구성되어 있기에 시간 복잡도는 O(NLogN)이다.

우선순위를 중요시해야 하는 상황에서 주로 쓰인다.
- **ArrayDequeue**

일반적으로 Deque는 선언이 안되므로, ArrayDeque나 LinkedList로 선언을 해줘야 한다.

ArrayDeque는 **Thread-Safe을 보장하지 않으므로** 사용할때 주의해야 한다.
### ✎ Set
Set은 저장 순서를 유지하지 않고, 요소의 중복을 허용하지 않는다.

크게 TreeSet, HashSet이 존재한다.

- TreeSet

TreeSet은 이진 검색 트리(binary search tree) 형태로 데이터를 저장하는 컬렉션 클래스이다. 

Set 인터페이스의 특징인 중복 저장 비허용과 순서 유지 안 됨을 똑같이 가지고 있다.


- HashSet
![Hashing.png](image%2FcollectionFramework%2FHashing.png)
Hashing을 사용하여 구현한 Set자료구조이다.

### ✎ Map
Map 인터페이스는 키(key)와 값(value)로 구성된 Entry 객체를 저장하는 구조를 가지고 있다. 

키와 값이라는 두 데이터를 연결하는 것이 매핑이다.

해싱(hashing)을 사용하기 때문에 방대한 양의 데이터를 검색하는데 있어서 뛰어난 성능을 보인다.

Map 인터페이스를 구현하는 클래스는 HashMap, Hashtable, TreeMap, SortedMap 등이 있다.(Hashtable과 HashMap은 Vector와 ArrayList의 관계와 같아서 새로운 버전인 HashMap 사용을 권장한다.)

Map은 키와 값을 쌍으로 저장하기 때문에 **iterator()를 직접 호출할 수 없다.** 때문에 keySet(), entrySet()와 같은 메서드를 이용해 Set 형태로 반환된 컬렉션에 iterator()를 호출해야 한다.

### 참고 자료
- https://hudi.blog/java-collection-framework-1/
- https://velog.io/@kai6666/Java-%EC%BB%AC%EB%A0%89%EC%85%98-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC-Collection-Framework
- https://docs.oracle.com/javase/8/docs/technotes/guides/collections/overview.html
- https://velog.io/@ecvheo1/JAVA-ArrayList-add-%EB%A9%94%EC%86%8C%EB%93%9C-%EB%8F%99%EC%9E%91-%EB%B0%A9%EC%8B%9D%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C
- https://onlyfor-me-blog.tistory.com/319
- https://velog.io/@gillog/Java-Priority-Queue%EC%9A%B0%EC%84%A0-%EC%88%9C%EC%9C%84-%ED%81%90
- https://devlog-wjdrbs96.tistory.com/245
- https://myvelop.tistory.com/164
- 