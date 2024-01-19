# ✎ Java Access Modifier
접근 제어자(Access Modifier)를 사용하여 변수나 메서드의 사용 권한을 설정할 수 있다.

다음과 같은 접근 제어자를 사용하여 사용 권한을 설정할 수 있다.

- Private
- Default
- Protected
- Public

접근 제어자는 **private < default < protected < public** 순으로 보다 많은 접근을 허용한다.

## Private
private는 private가 붙은 변수나 메서드는 해당 클래스에서만 접근 가능하다.

## Default
접근 제어자를 따로 설정하지 않는다면 변수나 메서드는 default 접근 제어자가 자동으로 설정되어 동일한 패키지 안에서만 접근이 가능하다.

## Protected
동일 패키지의 클래스 혹은 해당 클래스를 상속받은 클래스에서만 접근 가능하다.

## Public
어떤 클래스에서도 접근이 가능하다.

## Java Access Modifiers with Method Overriding
메서드를 재정의 할 때, 기존 메서드의 접근 제어자보다 범위가 더 좁으면 안된다. 확장만 가능하다.


### 참고 자료
- https://wikidocs.net/232
- https://www.javatpoint.com/access-modifiers
- 