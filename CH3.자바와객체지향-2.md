### 1. 상속 : 재사용 + 확장
> - 오해 1: 계층구조
  + _할아버지-아버지-아들_ 같은 계층 구조가 아님!!
- 사실 1: 분류도
  Class는 분류다! / 상속 관계는 벤 다이어 그램이다!
  동물
  포유류    조류
  고래 사람  참새 독수리

> - 오해 2 : is a 관계  
- 사실 2 
  + Sub Class is Super Class (X)
    * 아들은 아버지이다(X)
    * 아버지는 할아버지이다(X)
  + Sub Class is a **kind of** Super Class (O)
    * 고래는 포유류(동물)의 한 분류이다(O)
    
> - 오해 3 : 상속(Inheritance)의 뜻
- 사실 3 : extends(확장)
  + Sub Class는 Super Class를 바탕으로(**재사용**) 특성을 **확장**한 것!


  
> - 상속의 개념
  + 확장 : 상위 Class의 특성(attribute, method)을 기본적으로 갖고 있음
  + 세분화
  + Super(추상화, 일반화, Concrete) - Sub(구체화, 특수화, specification)
  
  
- 다중 상속 / 인터페이스
  + 자바는 다중 상속을 허용하지 않음
    * 2가지 Super Class를 extends하는 Sub Class는 같은 method signature를 가진 methods중 어느것을 호출할지 모름
  + 인터페이스는 여러개 implement 가능
    * 인터페이스 : **be able to**
    ~ 기능을 할 수 있는
    Cloneable/Serializable/Comparable/Runnable
    Class 에게 특정 기능(interface의 method)의 구현을 강제함
    
- Extends Class VS Implements Interface
  + Extends : "Sub Class extends Super Class"
  Sub Class가 Super Class의 특성을 확장
  + Implements : Class implements Interface(~able)
  Class는 Interface에 정의된 기능(method)를 할 수 있다(able)
  
  
  + 객체 지향 설계 5원칙
    * 상속 : 상위 클래스가 풍성할 수록 좋다
      + by LSP(리스코프 치환 원칙)
    "Sub Class의 인스턴스는 Super Class의 인스턴스인 척을 할 수 있어야 함"
    
    
    * 구현 : 인터페이스가 구현을 강제할 메서드 수가 적을 수록 좋다
    by ISP(인터페이스 분할 원칙)
      + 인터페이스(기능)가 이것저것 할 수 있다면 ~able로 이름 짓기가 곤란하기도 함.
      + 호출하는 쪽이 해당 메서드를 호출 할 수 있을지가 관심영역 -> 관련이 있는 메서드가 아닌 이상 되도록 적은 메서드를 갖게 하는것이 좋을 것 같다
      + 관심 대상이 아닌 메서드를 포함하면, 인터페이스의 구현체가 관심 대상이 아닌 추상메서드도 override해야하는 문제가 있다
    
    
- Class와 객체 이름
  + Class는 분류이므로 넓은 범위의 이름으로(Person)
  + 객체는 유일한 것이므로 특수한 이름으로(student, teacher)
    
- Extends와 Implements - UML
  + Sub Class -> Super Class (실선꼬리)
  + Class -> Interface (점선 꼬리) 또는 사탕 막대
  
- Extends와 T 메모리
Sub Class의 인스턴스를 생성하면 heap에 Super Class 인스턴스도 생긴다
(Object Class는 당연히 생성됨)
  + Super를 Animal, Sub를 person라 하면
    * Person student = new Person();
   student 참조변수는 Person 인스턴스 영역을 참조한다
    * Animal student new Person();
   student 참조변수는 Animal 인스턴스 영역을 참조한다
   
  + 참조형(참조변수 타입)에 따라서 접근할 수 있는 객체의 영역이 다름
  + Super type의 참조변수는 override된 Sub Class 인스턴스의 메소드를 호출해 줌
   
  
### 2. 다형성 : 사용편의성
Super - Sub / Class - Interface 간 다형성 의미가 형성됨

- Overriding : Super Class의 method와 같은 method signature를 갖는 method를 Sub Class에서 재정의
  + Sub type 참조변수는 본인이 override한 method를 먼저 찾게됨
- Overloading : 같은 메서드 이름이지만 다른 인자로 메서드를 중복 정의
  + overloading이 다형성 개념에 속하는지는 약간 논란이 있으나
  + 같은 기능을 다른 인자로 하는 여러가지 메소드를 같은 이름으로 정의하여 호출할 수 있는 **사용자 편의성**을 제공한다
  + Generic을 이용하면 하나의 method로 다양한 method를 구현한 효과!
 
### 3. 캡슐화 : 정보 은닉
- 접근 제어자 public/protected/(default)/private

  - public : 모두가 접근 가능
  - protected : 같은 패키지 또는 상속(Sub Class)한 Sub Class의 인스턴스에서 접근 가능
  - default : 같은 패키지
  - private : 객체 내에서만 접근 가능
  
- *주의!!*
  + 다른 .jar에 같은 패키지(이름)에 속한 클래스간 public, protected, default로 선언된 멤버에 접근 가능

- static 멤버는 Class명.멤버로 접근하는 것을 추천
  + 객체.멤버 : 메소드 스택 프레임의 객체 참조변수 ->(Super Class의 멤버라면 Super Class 객체->) 클래스 영역의 멤버 접근
  

### 4. 참조 변수의 복사
참조 변수는 객체의 주소를 갖는 변수
기본형 변수의 복사와 마찬가지로 값을 복사하는 것은 동일
But, 객체의 주소를 복사함 
= 다른 객체의 주소를 갖게 됨
= 참조의 대상이 달라짐
