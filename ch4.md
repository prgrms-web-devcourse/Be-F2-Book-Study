## abstract 키워드 - 추상 메서드와 추상 클래스

추상 메서드 : 선언부는 있는데 구현부가 없는 메서드

- 추상 메서드를 하나라도 갖고 있는 클래스는 반드시 추상 클래스(Abstract Class)로 선언해야 한다.
- 추상 메서드 없이도 추상 클래스를 선언할 수는 있다.
- 추상 메서드는 하위 클래스에게 메서드의 구현을 강제한다.

추상 클래스 : 인스턴스 - 객체를 만들 수 없는 클래스. new를 사용할 수 없다.

- 추상 메서드를 포함하는 클래스는 반드시 추상 클래스여야 한다.



## 생성자

클래스의 인스턴스, 즉 객체를 만들 때마다 new 키워드를 사용한다.

- `클래스명()` 도 메서드다. 
- 반환값이 없고 클래스명과 같은 이름을 가진 메서드를 객체를 생성하는 메서드라고 해서 객체 생성자 메서드라 한다.
- 자바가 알아서 인자가 없는 기본 생성자를 자동으로 만들어 준다.
- 인자가 있는 생성자를 하나라도 만든다면 자바는 기본 생성자를 만들어 주지 않는다.

정확하게 표현하자면  `객체 생성자 메서드`



## 클래스 생성 시의 실행 블록, static 블록

- 클래스 생성자는 존재하지 않는다.
- static 블록 : 클래스가 **스태틱 영역에 배치될 때** 실행되는 코드 블록

- static 블록에서 사용할 수 있는 속성과 메서드는 당연히 static 멤버 뿐이다.
- 객체 멤버에는 접근할 방법이 없다.

- ~~프로그램이 시작될 때 모든 패키지와 모든 클래스가 T 메모리의 스태틱 영역에 로딩된다.~~ -> 실제로는 해당 패키지 또는 클래스가 처음으로 사용될 때 로딩된다.

- 인스턴스를 여러 개 만들어도 static 블록은 단 한 번만 실행된다.

클래스 정보는 해당 클래스가 코드에서 맨 처음 사용될 때 T 메모리의 스태틱 영역에 로딩되며 단 한 번 해당 클래스의 static 블록이 실행된다.

처음 사용될 때의 세 가지 경우

- 클래스의 정적 속성을 사용할 때
- 클래스의 정적 메서드를 사용할 때
- 클래스의 인스턴스를 최초로 만들 때

❓왜 프로그램이 실행될 때 바로 클래스들의 정보를 T 메모리의 static 영역에 로딩하지 않고 해당 클래스가 처음 사용될 때 로딩할까?

- **스태틱 영역도 메모리**이기 때문이다.
- 메모리는 최대한 늦게 사용을 시작하고 최대한 빨리 반환하는 것이 정석이다.
- 자바는 스태틱 영역에 한 번 올라가면 프로그램이 종료되기 전까지는 해당 메모리를 반환할 수 없지만 그럼에도 최대한 늦게 로딩함으로써 메모리 사용을 최대한 늦추기 위해서다.



## final 키워드

final 키워드가 나타날 수 있는 곳

- 클래스
- 변수
- 메서드

### final과 클래스

클래스에 final이 붙었다면 -> 상속을 허락하지 않겠다는 의미 : 하위 클래스를 만들 수 없다.

### final과 변수

변수에 final이 붙었다면 -> 변경 불가능한 상수

- 정적 상수는 **선언 시**에, 또는 **정적 생성자에 해당하는 static 블록 내부**에서 초기화가 가능하다.
- 객체 상수는 **선언 시**에, 또는 **객체 생성자** 또는 **인스턴스 블록**에서 초기화할 수 있다.
- 지역 상수 **선언 시**에, 또는 **최초 한 번**만 초기화가 가능하다.

### final과 메서드

메서드가 final이라면 -> 재정의(오버라이딩)을 금지



## instanceof 연산자

만들어진 객체가 특정 클래스의 인스턴스인지 물어보는 연산자 - 결과로 true 또는 false를 반납한다.

강력하기는 하지만 객체 지향 설계 5원칙 가운데 LSP(리스코프 치환 원칙)를 어기는 코드에서 주로 나타나는 연산자이기에 코드에 instanceof 연산자가 보인다면 **냄새 나는 코드가 아닌지**, 즉 **리팩터링의 대상**이 아닌지 점검해 봐야 한다.

클래스들의 상속 관계 뿐만 아니라 인터페이스의 구현 관계에서도 동일하게 적용된다.



## package 키워드

네임스페이스(이름공간)를 만들어주는 역할을 한다.

필요성 : 동일한 클래스 명으로 인한 충돌 발생 방지 (소유자 비유)



## interface 키워드와 implements 키워드

- 인터페이스는 public 추상 메서드와 public 정적 상수만 가질 수 있다.
- 인터페이스는 추상 메서드와 정적 상수만 가질 수 있기에 따로 메서드에 public, abstract / 속성에 public, static, final을 붙이지 않아도 자동으로 자바가 알아서 붙여준다.



## this 키워드

객체가 자기 자신을 지칭할 때 쓰는 키워드다.

- 지역 변수와 속성(객체 변수, 정적 변수)의 이름이 같은 경우 지역 변수가 우선한다.
- 객체 변수와 이름이 같은 지역 변수가 있는 경우 객체 변수를 사용하려면 this를 접두사로 사용한다.
- 정적 변수와 이름이 같은 지역 변수가 있는 경우 정적 변수를 사용하려면 클래스명을 접두사로 사용한다.



## super 키워드

단일 상속만을 지원하는 자바에서 바로 위 `상위 클래스의 인스턴스`를 지칭하는 키워드다.

super 키워드로 바로 위의 상위 클래스의 인스턴스에는 접근할 수 있지 super.super 형태로 상위의 상위 클래스의 인스턴스에는 접근이 불가능하다.