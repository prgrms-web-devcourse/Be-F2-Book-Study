# Chpater 06  스프링이 사랑한 디자인 패턴 🚀
___
 
## > 2021.11.16 Chapter 6 ##

## 정리 ##

- ### 목표 ###
    1. 스프링의 각 디자인 패턴을 이해하고 사용할 수 있다.

---

- ### 프록시 패턴(Proxy Pattern) ###
    > 제어 흐름을 조정하기 위한 목적으로 중간에 대리자를 두는 패턴
    
    - 프록시 패턴은 왜 사용하는 것일까?
        - 실제 객체를 수행하기 이전에 전처리릏 가너ㅏ, 기존 객체를 캐싱할 수 있다. 
        - 캐싱과정을 통해 부하를 줄일 수 있다.
    - 프록시 패턴의 단점
        - 프록시 패턴을 다수 이용할 경우, 다수의 객체가 추가적으로 생성 -> 성능 저하
    - 종류
        1. 가상 프록시
        2. 원격 프록시
        3. 보호 프록시
    - 예제
    ```java
        public interface IService{
            String runSomething();
        }
    ```

    ```java
        public class Service implements IService{
            public String runSomething(){
                return "서비스";
            }
        }
    ```

    ```java
        public class Proxy implements IService{
            IService service1;

            public String runSomething(){
                System.out.println("프록시 클래스");

                service1 = new Service();
                return service1.runSomething();
            }
        }
    ```

    ```java
        public class ClientWithProxy{
            public static void main(String[] args){
                IService proxy = new Proxy();
                System.out.println(proxy.runSomething());
            }
        }
    ```
    - [참고 블로그](https://today-retrospect.tistory.com/102?category=464082)

- ### 데코레이터 패턴 ###
    - 프록시 패턴과 유사하지만, 프록시 패턴은 그대로 값을 전달하는 반면, 데코레이터 패턴은 클라이언트가 받을 반환값에 기능을 추가해서 반환한다.

    - 프록시 패턴과 동일한 구조이기에 **변화**에 초점을 맞추면 된다.

    - 위 프록시 패턴의 Proxy 클래스 코드를 아래 코드처럼 변경할 수 있다.
    ```java
        public class Proxy implements IService{
            IService service1;

            public String runSomething(){
                System.out.println("프록시 클래스");

                service1 = new Service();
                return "데코레이터 패턴 " + service.runSomething();
            }
        }
    ```

- ### 싱글톤 패턴 ###
    > 클래스의 인스턴스, 즉 객체를 하나만 만들어 사용하는 패턴

    - 왜 사용하는 것일까?
        - 이전보다 메모리 낭비를 방지할 수 있다.
        - 데이터 공유를 하기가 쉽다.
    - 단점 및 주의할 부분
        - 싱글톤의 역할이 커질수록 해당 싱글톤 객체를 사용하는 다른 객체간의 결합도가 높아져 객체 지향 설계 원칙에 어긋나게 된다.
            - 객체 지향 설계의 '개방-폐쇄'
        - 동시성 문제 발생
    - 종류
        1. Eager Initialization
        2. Static Block Initialization
        3. Lazy Initialization
            1. Lazy Initialization with Synchronized
            2. Lazy Initialization with DCL
            3. Lazy Initialization with Enum
            4. Lazy Initialization / LazyHolder / Bill Pugh Singleton

    
    - 아래는 LazyHolder에 대한 예제 코드입니다.

    ```java
        public class LazyInitializationHolder {
            public LazyInitializationHolder() {
            }
        
            private static class Singleton {
                private static final LazyInitializationHolder instance = new LazyInitializationHolder();
            }
        
            public static LazyInitializationHolder getInstance() {
                return Singleton.instance;
            }
        
            public void printHashCode() {
                System.out.println("HashCode of Instance : " + Singleton.instance.hashCode());
            }
        }
    ```

- ### 템플릿 메소드 패턴 ###
    > 상위 클래스의 견본 메소드에서 하위 클래스가 오버라이딩한 메소드를 호출하는 패턴
    - 추상 클래스 
        - 템플릿 메소드를 정의하는 클래스
        - 하위 클래스에 공통 알고리즘을 정의
    - 예시
        - Java의 AbstractMap<K,V>
            - AbstractMap은 Map 인터페이스를 implement한다.
            - HashMap, TreeMap은 AbstractMap을 상속받아 구현된다.
                - 그래서 get으로 구현되어있다.
    - ![image](https://user-images.githubusercontent.com/73347933/141942583-4dc0c550-50b6-4ec1-9004-4b408f2759ed.png)
    ```java
    public abstract class Animal{
        // 템플릿 메소드
        public void playWithOwner(){
            System.out.println("템플릿 메소드 패턴");
            play();
            runSomething();
            System.out.println("타일러팀");
        }

        // 추상메소드
        abstract void play();

        // Hook 메소드
        void runSomething(){
            System.out.println("hook 메소드");
        }
    }
    ```
    ```java
    public class Dog extends Animal{
        @Override
        // 추상 메소드 오버라이딩
        void play(){
            System.out.println("추상메소드 오버라이딩");
        }

        @Override
        // Hook 메소드 오버라이딩
        void runSomething(){
            System.out.println("Hook 메소드 오버라이딩");
        }
    }
    ```

- ### 팩토리 메소드 패턴 ###
    > 하위 클래스에서 팩토리 메소드를 오버라이딩해서 객체를 반환하게 하는 것
    - 확장 및 유지보수에 용이하다.
    - DI(의존관계 역전 원칙)을 성립한다.
    - 단점
        - 팩토리 메소드 패턴을 구현하기 위해 많은 서브 클래스를 도입하는 과정에서 코드가 복잡해 질 수 있다.

    ```java
    public abstract class Vehicle {
 
        public abstract String getBrand();
    
        public abstract String getYear();
 
        @Override
        public String toString() {
            return this.getBrand() + "회사의 " + this.getYear() + "년식 모델입니다.";
        }
    }
    ```
    ```java
    public class Car extends Vehicle {
        private String brand;
        private String year;
    
        public Car(String brand, String year) {
            this.brand = brand;
            this.year = year;
        }
    
        @Override
        public String getBrand() {
            return this.brand;
        }
    
        @Override
        public String getYear() {
            return this.year;
        }
    }
    ```
    ```java
    public class Motorcycle extends Vehicle {
        private String brand;
        private String year;
    
        public Motorcycle(String brand, String year) {
            this.brand = brand;
            this.year = year;
        }
    
        @Override
        public String getBrand() {
            return this.brand;
        }
    
        @Override
        public String getYear() {
            return this.year;
        }
    }
    ```
    ```java
    public class VehicleFactory {
        public static Vehicle getVehicle(String type, String brand, String year) {
            if (type.equalsIgnoreCase("car")) {
                return new Car(brand, year);
            } else if (type.equalsIgnoreCase("motorcycle")) {
                return new Motorcycle(brand, year);
            }
            return null;
        }
    }
    ```
    ```java
    public class Main {
        public static void main(String[] args) {
            Vehicle car = VehicleFactory.getVehicle("car", "BMW", "2021");
            Vehicle motorcycle = VehicleFactory.getVehicle("motorcycle", "Ducati", "2017");
    
            System.out.println(car.toString());
            System.out.println(motorcycle.toString());
        }
    }
    ```
- ### 전략 패턴 ###
    > 전략 패턴은 어떠한 알고리즘을 사용할 것인지 선택이 가능하게 하는 행위 패턴이며 이를 이용하여 보다 유연하고 재사용가능한 객체 지향 설계 방법

    - 기능은 같지만 다른 전략을 가진 클래스들을 캡슐화하여 서로 교환이 가능하도록 하는 패턴이다.

    - 유지보수가 용이 / OCP 원칙을 만족하기 때문에 확장을 할 때 편리하다.

    - 전략 패턴의 세 요소
        1. 전략 메소드를 가진 전략 객체
        2. 전략 객체를 사용하는 컨텍스트
        3. 전략 객체를 생성해 컨텍스트에 주입하는 클라이언트

    ```java
        public class Korean implements StudyStrategy {
            @Override
            public void study() {
                System.out.println("국어 공부해야지!");
            }
        }
    ```
    ```java
        public class Study {
            private StudyStrategy studyStrategy;
        
            public void setStudyStrategy(StudyStrategy studyStrategy) {
                this.studyStrategy = studyStrategy;
            }
        
            public void decideSubject() {
                if (studyStrategy == null) {
                    System.out.println("어떤 과목을 공부할까?");
                } else {
                    studyStrategy.study();
                }
            }
        }
    ```
    ```java
    public class Main {
        public static void main(String[] args) {
            Study study = new Study();
            study.decideSubject();
    
            study.setStudyStrategy(new Korean());
            study.decideSubject();
    
            study.setStudyStrategy(new Math());
            study.decideSubject();
    
            study.setStudyStrategy(new English());
            study.decideSubject();
        }
    }
    ```

- ### 어댑터 패턴 ###

    - 변환기의 역할을 하는 패턴이다.
        - 여기서, 변환기란, 서로 다른 두 인터페이스 사이에 통신이 가능하게 하는 것이다.
    - 장점
        - 관계가 없는 인터페이스 간 같이 사용 가능
        - 프로그램 검사 용이
        - 클래스 재활용성 증가 등
    - ![image](https://user-images.githubusercontent.com/73347933/141954090-18b41a22-a25e-47b3-bc40-700be0f9a334.png)
    ```java
    public class ServiceA{
        ServiceA sa1 = new ServiceA();

        void runService(){
            sa1.runServiceA();
        }
    }
    ```
    ```java
    public class ServiceB{
        ServiceB sb1 = new ServiceB();
        void runService(){
            sb1.runServiceB();
        }
    }
    ```
    ```java
    public class ClientWithAdapter{
        public static void main(String[] args){
            ServiceA sa1 = new ServiceA();
            ServiceB sb1 = new ServiceB();

            sa1.runService();
            sb1.runService();
        }
    }
    ```
- ### 템플릿 콜백 패턴 ###
    - 전략 패턴의 변형
    - 전략을 익명 내부 클래스로 구현한 전략 패턴
    - DI에서 사용하는 특별한 형태의 전략 패턴이다.
    - 전략 패턴의 일종이므로, OCP와 DIP가 적용된 설계 패턴이다.
    ```java
    public interface Strategy{
        public abstract void runStrategy();
    }
    ```
    ```java
    public class Soldier{
        void runContext(String weaponSound){
            System.out.println("전투 시작");
            executeWeapon(weaponSound).runStrategy();
            System.out.println("전투 종료");
        }

        private Strategy executeWeapon(final String weaponSound){
            return new Strategy(){
                @Override
                public void runStrategy(){
                    System.out.println(weaponSound);
                }
            }
        }
    }
    ```

- 이전에 작성한 디자인 패턴
- [싱글톤 패턴](https://today-retrospect.tistory.com/98?category=464082)
- [프록시 패턴](https://today-retrospect.tistory.com/102?category=464082)
- [빌더 패턴](https://today-retrospect.tistory.com/107?category=464082)
- [팩토리 메소드 패턴](https://today-retrospect.tistory.com/110?category=464082)
- [전략 패턴](https://today-retrospect.tistory.com/112?category=464082)