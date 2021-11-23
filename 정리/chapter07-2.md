# Chpater 07  스프링 삼각형과 설정 정보 🚀
___
 
## > 2021.11.23 Chapter 7-2 ##

## 정리 ##

- ### 목표 ###
    1. AOP에 대한 개념과 사용하는 예제를 이해할 수 있다.

---

- ![image](https://user-images.githubusercontent.com/73347933/142580897-9116c780-c44d-4d49-956b-70a359387952.png)


- ### AOP - Aspect? 관점? 핵심 관심사? 횡단 관심사? ###
    > 'AOP'는 Aspect-Oriented Programming의 약자이고 이를 번역하면 관점 지향 프로그램이이 된다. 
    - DI는 의존성 주입 / AOP는 로직 주입이라고 이해하면 된다.]
    - AOP를 통해 이전보다 유지보수 관점에서 편리하고 좋은 코드를 작성할 수 있다.
    - 코드 = 핵심 관심사와 횡단 관심사의 결합이다.
        - 횡단 관심사는 모듈별로 반복 및 중복되어 나타나는 부분이다.
        - 따라서, 당연히 이러한 반복 및 중복을 제거해야하는데 이때 AOP가 적용되는 것이다.
    - ![image](https://user-images.githubusercontent.com/73347933/142978646-bedcab91-349b-493e-87ed-d1f1e72ea2e1.png)
        - 위 그림 처럼, 메소드에서 코드를 주입할 수 있는 곳은 총 다섯이다.
            1. Around - AOP가 적용될 메소드의 시작부터 끝까지 전반적인 시점을 모두 관리한다.
            2. Before
            3. After
            4. AfterReturning - 적용될 메소드가 에러없이 성공적으로 실행된 이후의 시점을 의미한다.
            5. After Throwing - AOP가 적용될 메소드가 에러를 발생하여 Exception을 던지는 시점을 의미한다.

    - ![image](https://user-images.githubusercontent.com/73347933/142981519-85ec6a41-09c8-44db-9a21-1e7ab245546f.png)

    ```java
    public interface Person{
        void runSomething();
    }
    ```

    ```java
    public class Boy implements Person{
        public void runSomething(){
            System.out.println("컴퓨터로 게임을 한다.");
        }
    }
    ```

    ```java
    @Aspect
    public class MyAspect{
        @Before("execution(public void aop002.Boy.runSomething())")
        public void before(JoinPoint joinPoint){
            System.out.println("여기는 aop 부분");
        }
    }
    ```

    - @Aspect는 이 클래스를 이제 AOP에서 사용하겠다는 의미이다.
        - Aspect는 pointcut과 advice의 결합입니다. 
    - @Before는 대상 메소드 실행 전에 이 메소드를 실행하겠다는 의미이다.
        - 여기서 execution의 형식은 "execution(접근제어자 반환형 패지키 경로. 클래스타입. 메소드명(파라미터))"의 형식이다.
        - 단, 접근제어자는 생략이 가능하다.
        - target으로 형식을 지정할 수 있다.
            - target의 형식은 "target(패키지경로.클래스타입)"의 형식이다. execution과 달리 target은 정확한 클래스 경로와 타입을 적어줘야만 한다.

    - JoinPoint
        - JoinPoint는 AOP를 적용할 수 있는 지점을 나타냅니다. 스프링 AOP에서 joinPoint는 항상 메소드 실행을 나타냅니다.
    - PointCut
        - PointCut은 표현식이나 패턴들을 활용하는 AOP의 EL입니다. 이는 여러개의 joinPoint의 집합입니다.
        - 포인트컷 표현식
            - 지시자
            1. execution : 가장 정교한 pointcut을 만들어낼 수 있습니다.
            2. within : 타입 패턴 내에 해당하는 모든 것들을 pointcut으로 설정합니다.
            3. bean : bean이름으로 pointcut을 설정합니다.
                - (추가 표현식)[https://sa1341.github.io/2019/05/25/%EC%8A%A4%ED%94%84%EB%A7%81-AOP-%EA%B0%9C%EB%85%90-%EB%B0%8F-Proxy%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EA%B5%AC%EB%8F%99%EC%9B%90%EB%A6%AC/]
        - ex)
            - execution(public * * (..))
                - 모든 public 메소드에서 실행
            - execution(* set*(..))
                - set으로 시작하는 모든 메소드에 실행
            - execution(* com.prgrms.service.MemberSErvice.*(..))
                - com.prgrms.service.MemberService안에 모든 메소드에 실행
            - execution(* com.prgrms.service.*.*(..))
                - com.prgrms.service 패키지 안에 모든 메소드에 실행
            - within(com.prgrms.service.*)
                - com.prgrms.service 패키지 안에 모든 joinpoint에 실행
        - 이 외에도 this, traget, @target, @within 등등이 있습니다.
   
    - Weaving
        - pointcut으로 지정한 핵심 비즈니스 로직을 가진 메소드가 호출될 때 advice에 해당하는 공통 기능의 메소드가 삽입되는 과정을 의미합니다. weaving을 통해서 공통 기능과 핵심기능을 가진 새로운 프록시를 생성하게 됩니다.
- ### 프록시 ###
    - 클라이언트에서 target을 호출하면, target이 아닌 target을 감싸고 있는 proxy가 호출되어 실행되도록 구성되어 있습니다.
    - 이러한 프록시는 런타임시에 weaving을 통해서 생성됩니다. 
    - Spring이 AbstractAutoProxyCreator라는 BeanPostProcessor로 Bean을 감싸는 프록시 Bean을     만들어 이를 이용하여 호출을 한다.
        - BeanPostProcessor는 어떠한 Bean이 등록되면, 그 Bean을 가공할 수 있는 life-cycle 인터페이스이다.)

    - 스프링 AOP 요약
        1. 스프링 AOP는 인터페이스 기반이다.
        2. 스프링 AOP는 프록시 기반이다.
        3. 스프링 AOP는 런타임 기반이다.
    