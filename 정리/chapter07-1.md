# Chpater 07  스프링 삼각형과 설정 정보 🚀
___
 
## > 2021.11.19 Chapter 6 ##

## 정리 ##

- ### 목표 ###
    1. IoC / DI에 대한 이해를 할 수 있다.
    2. DI의 여러 방법에 대해 이해할 수 있다.

---

- ![image](https://user-images.githubusercontent.com/73347933/142580897-9116c780-c44d-4d49-956b-70a359387952.png)


- ### 의존성이란? ###
    > '전체가 부분에 의존한다'라는 개념으로 이해하면 된다.
    - ![image](https://user-images.githubusercontent.com/73347933/142581550-acf133f7-c830-4efa-b6c5-ec10f2b6ffff.png)
    
    ```java
    public class Car{
        Tire tire;
        
        public Car() {
            tire = new KoreaTire();
        }

        public String getTireBrand(){
            return "장착된 타이어 : " + tire.getBrand();
        }
    }
    ```
    - 클래스 다이어그램에서 보여지듯이 Car와 Tire는 의존관계가 발생한다.
    
- ### 스프링 없이 의존성 주입하기 1 - 생성자를 통한 의존성 주입 ###
    - ![image](https://user-images.githubusercontent.com/73347933/142582575-4c3d6f8d-4076-42ca-aa56-bcc22eb4f32b.png)
    - 이전과 다르게 Car의 생성자에 인자가 생긴 것을 확인할 수 있다.

    ```java
    public class Car{
        Tire tire;
        
        public Car(Tire tire){
            this.tire = tire;
        }

        public String getTireBrand(){
            return "장착된 타이어 : " + tire.getBrand();
        }
    }
    ```
    - 생성자를 통한 의존성 주입 방식을 이용하니 이전의 **new** 키워드가 사라진 것을 확인할 수 있다.
    - 이러한 방식을 사용할 경우, 기존의 코드보다 훨씬 유연성이 높다는 것을 확인할 수 있다. 그 이유는, 우리는 Car 객체를 생성할 때 부터의 코드를 또다시 수정할 필요가 없기 때문이다.
    - 더불어, 확장성도 좋다. 추가적인 브랜드의 타이어가 생길 경우 각각 인터페이스만 구현한다면 이전의 코드를 크게 수정할 필요가 없기 때문이다. 
    - **여기서는 전략 패턴이 적용되었다**

- ### 스프링 없이 의존성 주입하기 2 - 속성을 통한 의존성 주입 ###
    - 생성자 방식을 사용할 경우, 추후 변경을 할 때 불편한 부분이 있다. 
    - 예로 들면, 운전자가 원할 때 Car의 Tire를 교체하여야 한다는 것이다.
    - **그래서 속성을 통한 의존성 주입에 대한 필요성이 거론되었다.**
        - 어느 의존성 주입이 더 좋다는 의견이 분분하지만, 현재는 생성자를 통한 의존성 주입 방식을 선호하는 이야기가 많습니다. 
    - ![image](https://user-images.githubusercontent.com/73347933/142585395-df6ab637-149f-482c-8d57-9082e0c6d4ab.png)
    ```java
    public class Driver{
        public static void main(String[] args){
            Tire tire = new KoreaTire();
            Car car = new Car();
            car.setTire(tire);
        }
    }
    ```
- ### 스프링을 통한 의존성 주입 - XML 파일 사용 ###
    - 스프링을 통한 의존성 주입은 생성자를 통한 방법과 속성을 통한 방법이 존재한다.
    ```java
    ApplicationContext context = new ClassPathXmlApplicationContext(xml 파일);
    Car car = context.getBean()
    ```

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

        <bean id="tire" class="expert002.KoreaTire"></bean>
        <bean id="americaTire" class="expert002.AmericaTire"></bean>
        <bean id="car" class="expert002.Car"></bean>

    </beans>
    ```
    - xml 파일을 변경하고 프로그램을 실행하면 바로 변경사항이 적용되므로 자바 코드를 변경 / 재컴파일 / 재배포할 필요성이 없다.
    ```xml
        <property name="tire" ref="koreaTire"></property>
    ```
    - property 태그를 이용해 주입하는 것은 곧 car.setTire(tire)라고 하는 것과 같은 역할을 한다.

- ### 스프링을 통한 의존성 주입 - @Autowired를 통한 속성 주입 ###
    - @Autowired 어노테이션을 이용하면 설정자 메소드를 이용하지 않아도 스프링 프레임워크가 설정 파일을 통해 설정자 메소드 대신 속성을 주입해 준다.
    ```xml
    <bean id = "car" class = "expert004.Car"></bean>
    ```
    - @Autowired 어노테이션을 통해 car의 property를 자동으로 엮어줄수 있어서 생략이 가능하다.


- ### 스프링을 통한 의존성 주입 - @Resource를 통한 속성 주입 ###
    - @Resource는 자바 표준 어노테이션으로 @Autowired와 같은 역할을 한다. 
    - @Autowired는
        - 스프링 프레임워크에서 지원한다.
        - type과 id가운데 type이 매칭 우선순위가 높다.
        - @Qualifier("")와 협업이 가능하다.
    - @Resource는
        - 표준 자바에서 지원한다.
        - id의 우선순위가 높다. 그다음이 type이다.
        - @Resource에 옵션으로 name 값을 부여할 수 있다.

- ### 정리 ###
    - 변수에 값을 할당하는 곳에서도 의존 관계가 발생한다. 모든 곳에 의존 관계는 존재한다.
    - DI는 외부에 있는 의존 대상을 주입하는 것을 말한다.
    - 의존 대상을 구현하고 배치할 때 SOLID와 응집도는 높이고 결합도는 낮춰야 한다.
    