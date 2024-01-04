# static 이란? 
- 프로그램 시작 시점에 클래스로더가 클래스를 해석하여 메서드 영역 또는 Metaspace 에 클래스 메타 데이터 및 정적 변수를 적재한다. 
- GC의 대상이 되지 않는다. 
- 프로그램 시작부터 종료 시점까지 사용 가능 

- cf. 동적 (dynamic) : 객체를 런타임 시 힙 영역에 할당하는 행위를 의미 

- `static`이 사용될 수 있는 곳 : 클래스의 변수, 메서드, 초기화 블럭 
- static 메서드는 인스턴스 생성하지 않고 호출 가능, static 메서드 내에서는 인스턴스 변수 사용 불가 


- 초기화 블럭 
	- 클래스가 로딩되고 클래스 변수를 힙에 올린 후 초기화 블럭이 수행된다. 
	- static 초기화 블럭, instance 초기화 블럭이 있다. 
	- static 초기화 블럭
		- 각 클래스당 최초 1회만 실행
		- static 초기화 블럭은 객체 생성 이전에 수행되므로 인스턴스 멤버에 접근 불가
		- 클래스 로딩 시 복잡한 초기화 과정을 수행해야 할 때 사용
		- `static { ... }` 
		
	- instance 초기화 블럭
		- 인스턴스가 생성될 때마다 수행된다. 
		- 수행 순서  
			인스턴스 멤버 초기화 -> 인스턴스 초기화 블럭 호출 -> 생성자 호출  


	```
	class SBTest{
		static int A;

		static {  //스태틱 블럭 
			test();
			A++;
			System.out.println("static block  : " + A);		
		}

		{
			instanceMethod();
			A++;
			System.out.println("instance block : " + A);
		}

		static void test() {
			A++;	
			System.out.println("static method  : " + A);
		}

		public SBTest() {
			instanceMethod();
			A++;
			System.out.println("constructor : " + A);
		}

		private void instanceMethod() {
			A++;
			System.out.println("instance method : " + A);
		}
	}

	--- 

	public class StaticBlocTest { 
  		public static void main(String[] args) {
			SBTest.test();  
			//1.클래스 로딩(1-1.멤버 초기화, 1-2.스태틱 블럭 호출) 2.스태틱 메서드 호출
			SBTest a = new SBTest();
		}
	}

		/**
		출력은 다음과 같다. 

		static method  : 1
		static block  : 2
		static method  : 3

		instance method : 4
		instance block  : 5
		instance method : 6
		contsructor  : 7
		**/
	
	```



### static 을 지양해야 하는 이유 
- 객체와 다른 라이프 사이클 -> 메모리 문제 (메모리 누수 발생 가능) 
	- static 멤버는 사용여부에 상관없이 프로그램 실행 시점에 메모리를 할당하고 대부분의 경우 프로그램 종료 시점까지 메모리에서 해제되지 않는다.
	- OutOtMemoryError 발생 가능
	- static 과 Collection 객체는 같이 사용하지 않는 것이 좋다. 
	
- 동시성 이슈  
	전역에서 접근 가능하므로 별도의 동기화 전략 필요 

- **런타임 다형성** 불가 (오버라이딩 불가하기에)  
	static으로만 이루어진 메서드를 사용하는 객체의 경우, 해당 객체를 메모리로 할당하여 사용하는 것이 아니고 객체.메서드로 바로 전급하여 호출한다. 

- 객체의 상태를 이용할 수 없음 
	- 정적 메서드를 사용하기 위해서는 필요로 하는 인자를 모두 외부에서 주입해야 한다. 정적 메서드 안에는 클래스의 인스턴스 필드를 사용할 수 없기 때문이다. 
	- 일반 메서드라면 객체 내의 인스턴스 변수를 통해 해당 메서드를 구현할 수 있으므로 변화하는 상태에 따른 기능 구현이 가능하다.  
	- 결국 객체 내에 정적 메서드가 많아지면 외부 값에 의존하는 **수동적인 객체**가 되어버린다. 
	
- 테스트하기 어려움  
	정적 필드는 전역으로 관리되기에 프로그램 전체에서 이 필드에 접근, 수정 가능하다.  
	따라서 해당 필드를 추론하기 어려워 테스트하기 까다롭다.  



### static 은 언제 사용할까?
- 상수 정의
- 유틸리티 클래스 정의
	- 유틸리티 클래스? 인스턴스 메서드, 인스턴스 변수를 제공하지 않고 데이터 처리를 위한 정적 메서드만 존재하는 클래스  
		ex. Math 클래스 




