### 1. 자바 프로그램의 개발과 구동
![](https://images.velog.io/images/pro-park-gation/post/ad62fc5d-2e8a-4933-9212-6ae00fa50981/image.png)

(배포용 구조)
- JDK : JRE + Dev Tools(javac.exe, Debugger, ...)
-> 개발자가 설치하여 사용
- JRE : JVM + rt.jar(Libraries, Class Loader, ...)
-> 자바 프로그램 사용자가 설치하여 사용
- JVM : 바이트 코드를 해석, 실행

(1) *.java(자바 소스코드) -> *.class(바이트 코드)로 컴파일

(2) JRE가 Byte code를 JVM에게 전달(클래스 로더)

(3) JVM이 Byte code를 해석하여 프로그램 실행

*컴파일된 바이트 코드는 어떤 운영체제든 해당 운영체제에서 지원되는 JVM 위에서 실행 가능
-> 이식성이 좋다
![](https://images.velog.io/images/pro-park-gation/post/c25514cc-5c4f-4f66-96b0-5e17d9c4c788/image.png)


### 2. 자바에 남아있는 절차적/구조적 프로그래밍의 유산
**함수**
절차적/구조적 프로그래밍은 함수(Function)를 사용에 중점을 둔다.
함수는 중복 코드를 한 곳에 모으고, 논리를 함수 단위로 구분하여 이해하기 쉬운 코드 작성에 도움이 된다.

자바에서는 함수대신 메서드(Method)가 있는데, 모든 메서드는 클래스(Class)에 속한다.
데이터(변수)와 동작(메서드)을 클래스로 묶어서 관리하는 객체지향의 특성을 이해해야 한다.

### 3. JVM 구조

![](https://images.velog.io/images/pro-park-gation/post/6a115cda-dfae-4b83-a57b-d60d9aefe878/image.png)

Byte code(.class file)이 class loader에 의해 JVM의 method area에 로드되면 프로그램을 위한 메모리 공간이 할당된다. 메모리 공간은 다음과 같이 구분된다.

(1) method area(static) : 모든 클래스들이 공유하는 각 class의 정보(클래스, 인터페이스, static 변수/메서드)

(2) Heap(객체) : class의 object가 할당되는 공간

(3) stack(메서드 스택 프레임) : 메서드가 호출되며 필요한 것들(매개변수, 지역변수, 리턴값, ret 주소 등)이 쌓이는 공간

(4) PC registers : threads의 control block

(5) Native Method Stack : Java외 언어로 작성된 코드 적재

### 4. main()메서드로 보는 JVM
자바 프로그램의 시작은 main() 메서드이다.
(1) JRE가 main() 확인하면 JVM에게 byte code 전달

(2) 전처리 
  - java.lang 패키지의 class들을 JVM static area에 로드
  - byte code에 있는 모든 class, import 패키지들을 JVM static area에 로드

(3) JVM Stack 영역에 main 메서드의 스택 프레임 할당
  - argument들을 (이후 프로그램 실행시 지역 변수들까지) stack에 쌓음
(4) main 메서드의 첫 line 실행

(5) 블록(중괄호 - 메서드/조건문/반복문)을 만나면 stack에 블록 프레임 할당

(6) 블록내의 리턴값(있다면) 저장공간, 지역변수를 메모리에 할당

(7) 닫는 중괄호를 만나면 블록 프레임 소멸

(8) main()의 닫는 중괄호를 만날 때 까지 5,6,7 반복,
   main 메서드 프레임이 소멸되면 JVM 중지, 자원 반납\
   
   
### 5. 변수
- 외부 블록의 변수가 내부 블록의 변수에는 접근 X, 그 반대는 가능
- 지역/클래스 변수/객체 멤버 변수
  * 지역 변수는 메소드나 블록 내에 정의된 변수. 중괄호를 벗어나면 소멸됨
  * 클래스 변수는 클래스의 인스턴스가 모두 공유하는 변수
    + 인스턴스들 끼리 공유하므로 멀티 스레딩에서는 mutual exclusion 발생 가능
    -> lock 또는 semaphore 필요
  * 객체 멤버 변수는 해당 객체 내에서 접근(외부에서 접근이 가능하지만 보통 private으로 선언)
  
  

