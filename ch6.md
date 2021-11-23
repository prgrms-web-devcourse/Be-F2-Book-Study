## 어댑터 패턴(Adapter Pattern)

'호출당하는 쪽의 메서드를 호출하는 쪽의 코드에 대응하도록 중간에 변환기를 통해 호출하는 패턴'

어댑터 - 변환기 -> 서로 다른 두 인터페이스 사이에 통신이 가능하게 한다.

- **JDBC**가 어댑터 패턴을 이용해 다양한 데이터베이스 시스템을 단일한 인터페이스로 조작할 수 있게 해준다.
- 플랫폼별 **JRE**

어댑터 패턴은 개방 폐쇄 원칙을 적용한 설계 패턴이다.

클라이언트가 변환기를 통해 동일한 메서드명으로 두 객체의 메서드를 호출한다.

```java
// 어댑터 패턴이 적용되지 않은 코드
public class ServiceA {

   void runServiceA() {
      System.out.println("ServiceA");
   }
}

public class ServiceB {

   void runServiceB() {
      System.out.println("ServiceB");
   }
}

public class ClientWithNoAdapter {

   public static void main(String[] args) {
      ServiceA serviceA = new ServiceA();
      ServiceB serviceB = new ServiceB();

      serviceA.runServiceA();
      serviceB.runServiceB();
   }
}
```

```java
// 어댑터 패턴 적용
public class AdapterServiceA { // 변환기

   ServiceA serviceA = new ServiceA();

   void runService() {
      serviceA.runServiceA();
   }
}

public class AdapterServiceB { // 변환기

   ServiceB serviceB = new ServiceB();

   void runService() {
      serviceB.runServiceB();
   }
}

public class ClientWithAdapter {

   public static void main(String[] args) {
      AdapterServiceA adapterServiceA = new AdapterServiceA();
      AdapterServiceB adapterServiceB = new AdapterServiceB();

      // 동일한 메서드명으로 두 객체의 메서드를 호출
      adapterServiceA.runService();
      adapterServiceB.runService();
   }
}
```

어댑터 패턴은 **합성**, **객체를 속성으로 만들어서 참조**하는 디자인 패턴이다.

**단점**

- 구성요소를 위해 클래스를 증가시켜야 하기 때문에 복잡도가 증가할 수 있다.
- 클래스 Adapter 의 경우 상속을 사용하기 때문에 유연하지 않다.
- 객체 Adapter 의 경우는 대부분의 코드를 다시 작성해야 하기 때문에 효율적이지 않다.



## 프록시 패턴(Proxy Pattern)

'제어 흐름을 조정하기 위한 목적으로 중간에 대리자를 두는 패턴'

프록시 - 대리자, 대변인 -> 다른 누군가를 대신해 그 역할을 수행한다.

- 대리자를 사용하지 않고 직접 호출하는 코드

  ```java
  public class Service {
        public String runSomething() {
           return "서비스 짱!!!";
        }
     }
  
     public class ClientWithNoProxy {
  
        public static void main(String[] args) {
           // 프록시를 이용하지 않은 호출
           Service service = new Service();
           System.out.println(service.runSomething());
        }
     }
  }
  ```

- 프록시 패턴이 적용된 코드

  ```java
  public interface IService {
  
     String runSomething();
  }
  
  public class Proxy implements IService {
  
     IService iService;
  
     public String runSomething() {
        System.out.println("호출에 대한 흐름 제어가 주목적, 반환 결과를 그대로 전달");
  
        iService = new Service();
        return iService.runSomething();
     }
  }
  
  public class ClientWithProxy {
  
     public static void main(String[] args) {
        // 프록시를 이용한 않은 호출
        IService proxy = new Proxy();
        System.out.println(proxy.runSomething());
     }
  }
  ```

프록시 패턴의 경우 실제 서비스 객체가 가진 메서드와 같은 이름의 메서드를 사용하는데, 이를 위해 **인터페이스를 사용**한다.

인터페이스를 사용하면 서비스 객체가 들어갈 자리에 대리자 객체를 대신 투입해 클라이언트 쪽에서는 실제 서비스 객체를 통해 메서드를 호출하고 반환값을 받는지, 대리자 객체를 통해 메서드를 호출하고 반환값을 받는지 전혀 모르게 처리할 수도 있다.

- 대리자는 실제 서비스와 같은 이름의 메서드를 구현한다. 이때 인터페이스를 사용한다.
- 대리자는 실제 서비스에 대한 참조 변수를 갖는다.(합성)
- 대리자는 실제 서비스와 같은 이름을 가진 메서드를 호출하고 그 값을 클라이언트에게 돌려준다.
- 대리자는 실제 서비스와 메서드의 호출 전후에 별도의 로직을 수행할 수도 있다.

개방 폐쇄 원칙, 의존 역전 원칙이 적용된 설계 패턴이다.

JPA에서 엔티티와 **연관관계를 맺고있는 엔티티들**을 **지연로딩**할 때 **프록시 객체**를 사용한다.



## 데코레이터 패턴(Decorator Pattern)

'메서드 호출의 반환값에 변화를 주기 위해 중간에 장식자를 두는 패턴'

데코레이터 패턴은 프록시 패턴과 구현 방법이 같다.

프록시 패턴은 클라이언트가 최종적으로 돌려받는 반환값을 조작하지 않고 그대로 전달하는 반면 **데코레이터 패턴은 클라이언트가 받는 반환값에 장식을 덧입힌다.**

- 프록시 패턴 : 제어의 흐름을 변경하거나 별도의 로직 처리를 목적으로 한다. 클라이언트가 받는 반환값을 특별한 경우가 아니면 변경하지 않는다.
- 데코레이터 패턴 : 클라이언트가 받는 반환값에 장식을 더한다.

```java
public interface IService {

   public abstract String runSomething();
}

public class Service implements IService {
   public String runSomething() {
      return "서비스 짱";
   }
}

public class Decorator implements IService {

   IService service;

   public String runSomething() {
      System.out.println("호출에 대한 장식 주목적, 클라이언트에게 반환 결과에 장식을 더하여 전달");
      service = new Service();
      return "정말" + service.runSomething(); // 반환값에 장식을 더한다.
   }
}

public class ClientWithDecorator {

	public static void main(String[] args) {
		IService decorator = new Decorator();
		System.out.println(decorator.runSomething());
	}
}
```

반환값에 장식을 더한다는 것을 빼면 프록시 패턴과 동일하다.

- 장식자는 실제 서비스와 같은 이름의 메서드를 구현한다. 인터페이스를 사용한다.
- 장식자는 실제 서비스에 대한 참조 변수를 갖는다.(합성)
- 장식자는 실제 서비스와 같은 이름을 가진 메서드를 호출하고, 그 반환값에 장식을 더해 클라이언트에게 돌려준다.
- 장식자는 실제 서비스의 메서드 호출 전후에 별도의 로직을 수행할 수도 있다.

개방 폐쇄 원칙과 의존 역전 원칙이 적용된 설계 패턴이다.



## 싱글턴 패턴(Singleton Pattern)

'클래스의 인스턴스, 즉 객체를 하나만 만들어 사용하는 패턴'

인스턴스를 하나만 만들어 사용하기 위한 패턴이다. 

커넥션 풀, 스레드 풀, 디바이스 설정 객체 등과 같은 경우 인스턴스를 여러 개 만들게 되면 불필요한 자원을 사용하게 되고, 프로그램이 예상치 못한 결과를 낳을 수 있다. -> 오직 인스턴스를 하나만 만들고 그것을 계속해서 재사용한다.

- new를 실행할 수 없도록 생성자에 private 접근 제어자를 지정한다.
- 유일한 단일 객체를 반환할 수 있는 정적 메서드가 필요하다.
- 유일한 단일 객체를 참조할 정적 참조 변수가 필요하다.

```java
public class Singleton {

   static Singleton singletonObject; // 정적 참조 변수

   private Singleton() { // private 생성자
   }

   // 객체 반환 정적 메서드
   public static Singleton getInstance() {
      if (singletonObject == null) {
         singletonObject = new Singleton();
      }

      return singletonObject;
   }
}
```

단일 객체인 경우 공유 객체로 사용되기 때문에 속성을 갖지 않게 하는 것이 정석이다. 단일 객체가 속성을 갖게 되면 하나의 참조 변수가 변경한 단일 객체의 속성이 다른 참조 변수에 영향을 미치기 때문이다. (읽기 전용 속성을 갖는 것은 문제가 되지 않는다. - 스프링의 싱글턴 빈이 가져야 할 제약조건)

- private 생성자를 갖는다.
- 단일 객체 참조 변수를 정적 속성으로 갖는다.
- 단일 객체 참조 변수가 참조하는 단일 객체를 반환하는 getInstace() 정적 메서드를 갖는다.
- 단일 객체는 쓰기 가능한 속성을 갖지 않는 것이 정석이다.



## 템플릿 메서드 패턴(Template Method Pattern)

'상위 클래스의 견본 메서드에서 하위 클래스가 오버라이딩한 메서드를 호출하는 패턴'

상위 클래스에 공통 로직을 수행하는 **템플릿 메서드**와 하위 클래스에 오버라이딩을 강제하는 **추상 메서드** 또는 선택적으로 오버라이딩할 수 있는 **훅(Hook) 메서드**를 두는 패턴이다.

```java
public abstract class Animal {

   // 템플릿 메서드
   public void playWithOwner() {
      System.out.println("귀염둥이 이리온...");
      play();
      runSomething();
      System.out.println("잘했어");
   }

   // 추상 메서드
   abstract void play();

   // Hook(갈고리) 메서드
   void runSomething() {
      System.out.println("꼬리 살랑 살랑~");
   }
}

public class Dog extends Animal {

   // 추상 메서드 오버라이딩
   @Override
   void play() {
      System.out.println("멍! 멍!");
   }

   // Hook(갈고리) 메서드 오버라이딩
   @Override
   void runSomething() {
      System.out.println("멍! 멍! 꼬리 살랑 살랑~");
   }
}

public class Cat extends Animal {

   // 추상 메서드 오버라이딩
   @Override
   void play() {
      System.out.println("야옹~ 야옹~");
   }

   // Hook(갈고리) 메서드 오버라이딩
   @Override
   void runSomething() {
      System.out.println("야옹~ 야옹~ 꼬리 살랑 살랑~");
   }
}

public class Driver {

	public static void main(String[] args) {
		Animal bolt = new Dog();
		Animal kitty = new Cat();

		bolt.playWithOwner();
		kitty.playWithOwner();
	}
}
```

의존 역전 원칙을 활용한다.



## 팩터리 메서드 패턴(Factory Method Pattern)

'오버라이드된 메서드가 객체를 반환하는 패턴'

하위 클래스에서 팩터리 메서드를 오버라이딩해서 객체를 반환하게 한다.

```java
public abstract class Animal {

   // 추상 팩터리 메서드
   abstract AnimalToy getToy();

}

// 팩터리 메서드가 생성할 객체의 상위 클래스
public abstract class AnimalToy {

   abstract void identify();
}

public class Dog extends Animal {

   // 추상 메서드 오버라이딩
   @Override
   AnimalToy getToy() {
      return new DogToy();
   }
}

// 팩터리 메서드가 생성할 객체
public class DogToy extends AnimalToy {

   @Override
   void identify() {
      System.out.println("나는 테니스공! 강아지의 친구");
   }
}

public class Cat extends Animal {

   // 추상 메서드 오버라이딩
   @Override
   AnimalToy getToy() {
      return new CatToy();
   }
}

// 팩터리 메서드가 생성할 객체
public class CatToy extends AnimalToy {

   @Override
   void identify() {
      System.out.println("나는 캣타워! 고양이의 친구");
   }
}

public class Driver {

   public static void main(String[] args) {
      // 팩터리 메서드를 보유한 객체들 생성
      Animal bolt = new Dog();
      Animal kitty = new Cat();

      // 팩터리 메서드가 반환하는 객체들
      AnimalToy boltBall = bolt.getToy();
      AnimalToy kittyTower = kitty.getToy();

      // 팩터리 메서드가 반환한 객체들을 사용
      boltBall.identify();
      kittyTower.identify();
   }
}
```

의존 역전 원칙을 활용한다.



## 전략 패턴(Strategy Pattern)

'클라이언트가 전략을 생성해 전략을 실행할 컨텍스트에 주입하는 패턴'

**전략 패턴을 구성하는 세 요소**

- 전략 메서드를 가진 **전략 객체**
- 전략 객체를 사용하는 **컨텍스트(전략 객체의 사용자/소비자)**
- 전략 객체를 생성해 컨텍스트에 주입하는 **클라이언트(제3자, 전략 객체의 공급자)**

클라이언트는 다양한 전략 중 하나를 선택해 생성한 후 컨텍스트에 주입한다.

전략을 다양하게 변경하면서 컨텍스트를 실행할 수 있다.

```java
// 전략 인터페이스
public interface Strategy {

   void runStrategy();
}

public class StrategyGun implements Strategy {

   @Override
   public void runStrategy() {
      System.out.println("탕탕탕");
   }
}

public class StrategySword implements Strategy {

   @Override
   public void runStrategy() {
      System.out.println("챙챙챙");
   }
}

public class StrategyBow implements Strategy {

   @Override
   public void runStrategy() {
      System.out.println("쒝쒝쒝");
   }
}

// 전략을 사용하는 컨텍스트
public class Soldier {

   void runContext(Strategy strategy) {
      System.out.println("전투 시작");
      strategy.runStrategy();
      System.out.println("전투 종료");
   }
}

// 전략 패턴의 클라이언트
public class Client {

   public static void main(String[] args) {
      Strategy strategy = null;
      Soldier soldier = new Soldier();

      // 총(전략)을 전달하여 수행하게 한다.
      strategy = new StrategyGun();
      soldier.runContext(strategy);

      // 칼(전략)을 전달하여 수행하게 한다.
      strategy = new StrategySword();
      soldier.runContext(strategy);

      // 활(전략)을 전달하여 수행하게 한다.
      strategy = new StrategyBow();
      soldier.runContext(strategy);
   }
}
```

같은 문제의 해결책으로 **상속을 이용하는 템플릿 메서드 패턴**과 **객체 주입을 통한 전략 패턴** 중에서 선택/적용할 수 있다.

단일 상속만이 가능한 자바 언어에서는 상속이라는 제한이 있는 템플릿 메서드 패턴보다는 전략 패턴이 더 많이 활용된다.

개방 폐쇄 원칙과 의존 역전 원칙이 적용된다.



## 템플릿 콜백 패턴(Template Callback Pattern - 견본/회신 패턴)

'전략을 익명 내부 클래스로 구현한 전략 패턴'

전략 패턴의 변형으로, 스프링의 DI(의존성 주입)에서 사용하는 특별한 형태의 전략 패턴이다.

전략 패턴과 모든 것이 동일한데 **전략을 익명 내부 클래스로 정의해서 사용한다**는 특징이 있다.

```java
public interface Strategy {

   void runStrategy();
}

public class Soldier {

   void runContext(String weaponSound) {
      System.out.println("전투 시작");
      executeWeapon(weaponSound).runStrategy();
      System.out.println("전투 종료");
   }

   // 전략을 생성하는 코드가 컨텍스트로 들어왔다.
   private Strategy executeWeapon(final String weaponSound) {
      return new Strategy() {
         @Override
         public void runStrategy() {
            System.out.println(weaponSound);
         }
      };
   }
}

public class Client {

   public static void main(String[] args) {
      Soldier soldier = new Soldier();

      soldier.runContext("총");

      soldier.runContext("칼");

      soldier.runContext("도끼");
   }
}
```

스프링은 이런 형식으로 리팩터링된 템플릿 콜백 패턴을 DI에 적극 활용하고 있다.

개방 폐쇄 원칙과 의존 역전 원칙이 적용된다.