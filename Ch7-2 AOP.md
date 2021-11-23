### AOP란?
- Aspect Oriented Programming
- 프로그램에서 core concern과 cross-cutting concern을 분리
- core concern : 주 업무(사용자가 이용하는 기능)
- cross-cutting concern : 보조 업무(로그, 보안, 트랜잭션)

- cross-cutting concern(로직)을 주입
  + 주입 시점
    * Around 메서드 전 구역
    * Before 메서드 시작 전
    * After 메서드 종료 후 
    * AfterReturning 정상 종료 후 
    * After Throwing 메서드에서 예외가 발생하면서 종료된 후
   
### AOP 용어
- Target : 핵심 기능을 담고 있는 모듈로, 부가기능을 부여할 대상
- JoinPoint
  + advice가 적용될 수 있는 위치
  + target 객체가구현한 인터페이스의 모든 메서드
- Pointcut
  + advice를 적용할 target의 메서드를 선별하는 Sp 
  + pointcut 표현식은 execution으로 시작하고 메서드의 Signature를 비교하는 방법을 주로 이용함
- Aspect
  + Aspect = Advice + Pointcut
  + Spring에서는 Aspect를 bean으로 등록해서 사용
- Advice 
  + advice는 target의 특정 joinpoint에 제공할 부가기능
  + Advice에는 @Before, @After, @Around, @AfterReturning, @AfterThrowing 등이 있음
   
   
### AOP 구현
- @Aspect : 클래스를 AOP에서 사용
- @Before : 해당 메서드 실행 전에 @Before에 선언된 메서드 실행

### AOP 적용시 장점
- Aspect/Pointcut/JoinPoint로 cross-cutting concern을 core concern으로 부터 분리해서, core concern 클래스가 SRP 원칙에 가까워짐
