### 1. SRP(단일 책임 원칙) : 어떤 클래스를 변경해야 하는 이유는 오직 하나뿐이어야 한다.
클래스는 단 한개의 책임을 가져야 한다.
-> 클래스가 가진 여러 책임별로 변경 사항이 생길 수 있다. 
-> 클래스가 하나의 책임을 가지면 변경되는 이유는 그 책임의 내용이 변경되어서 이다.

책임의 단위는 변화되는 부분과 관련이 있다.
Class의 변수/메소드를 사용하는 사용자(Class 외부)마다 다른 의미로 사용하거나(변수), 다른 메소드를 사용한다면 단일 책임에 원칙에 위배될 확률이 크다.

ex) Domain Class(model)가 정보 반환(getter), 정보 저장(save), 정보 출력(print)의 역할을 한다면,
정보 저장은 저장 매체에 따라서
- saveDB
- saveMemory
정보 출력은 출력 매체에 따라서 코드가 변경된다.
- printScreen
- printConsole

SRP 원칙은 변수, 클래스, 패키지, 모듈, 데이터베이스 테이블가지 적용할 수 있는 원칙

두가지 알려진 방법
1) 클래스에서 변화가 있는 부분은 sub class로, 변화가 없는 부분(공통)은 super class로 
2) 클래스 일부를 extract하여 멤버로 





### 2. OCP(개방 폐쇄 원칙) : 자신의 확장에는 열려 있고, 주변의 변경에 대해서는 닫혀 있어야 한다.
- 주변 변경에 닫혀 있다
한 클래스가 사용하는 다른 클래스의 인스턴스가 여러 종류일 때, 인스턴스마다 다른 사용법이 필요
  + 참조 대상인 클래스가 추상체(상위 클래스, 인터페이스)를 제공하면 사용자는 구현체가 달라져도 변화가 없음

- 자신의 확장
  + 자신의 추상체가 있으면 확장(다른 구현체)에는 무리가 없음

예제
- JDBC 인터페이스
  + DB를 사용하는 자바 app은 DB 종류에 상관없이 JDBC 인터페이스가 제공하는 메서드만 호출
- 자바
  + 자바 프로그래머는 JVM 덕분에 프로그램이 동작할 환경(os..)에 영향받지 않음
  
**변화하는 부분을 추상화(상위 클래스/인터페이스)하자**



### 3. LSP(리스코프 치환 원칙) : 서브 타입은 언제나 자신의 기반 타입으로 교체할 수 있어야 한다.

상위 타입이 풍부해야 하위 타입의 인스턴스를 상위형 참조 변수에 대입해 그 역할을 할 수 있다.
- 상위 타입이 풍부하지 않으면 형변환을 자주 해야됨
- 또는 if ( 하위 A instance of 상위) 같은 로직이 필요해져서 코드가 복잡해짐  
  + 여기서는 개방 폐쇄 원칙도 위반하게 됨(하위 타입이 많아지고 그 변경이 사용자에게 영향을 미치면 사용자는 외부 변경에 닫혀있지 않게됨)

- 예시
  + 자바 Collection에서 상위 타입만으로 manipulation 하는 것에 무리가 없다




### 4. ISP(인터페이스 분리 원칙) : 클라이언트는 자신이 사용하지 않는 메서드에 의존 관계를 맺으면 안된다.

- 클라이언트가 관심있는 기능만 공개할 것
  + 사용해선 안되는 기능을 이용하면 안됨
  + 관심없는 기능이 변해도 영향을 받으면 안됨
  
  
```java
// ISP를 만족하지 않는다. 
interface Printer { 
    copyDocument(); 
    printDocument(document: Document); 
    stapleDocument(document: Document, tray: Number); 
} 

// 사용되지 않는 메서드를 구현하고 있다.
class SimplePrinter implements Printer { 
    public copyDocument() {} // 사용 하지 않음 
    public printDocument(document: Document) { 
    	console.log("simple copy", document); 
    } 
    public stapleDocument(document: Document, tray: Number) {} // 사용 하지 않음 
}

```

```java
// ISP를 만족한다. 
interface Printer { 
    printDocument(document: Document); 
} 

interface Stapler { 
    stapleDocument(document: Document, tray: number); } 
    
interface Copier { 
    copyDocument(); 
} 

class SimplePrinter implements Printer { 
    public printDocument(document: Document) {} 
} 

class SuperPrinter implements Printer, Stapler, Copier { 
    public copyDocument() {} 
    public printDocument(document: Document) {} 
    public stapleDocument(document: Document, tray: number) {} 
}
```

### 5. DIP(역전 의존 원칙) : 자신보다 변하기 쉬운 것에 의존하지 말것.

자신보다 변하기 쉬운 것에 의존하던 것
-> 추상화된 **인터페이스**나 **상위 클래스**를 두어 변하기 쉬운 것의(구상체) 변화에 영향받지 않게 하는 것이 의존 역전 원칙이다.

User - Pay(추상체) - samsung/kakao/naver(구현체)


## 느낀점 
이게 다 유지보수를 잘 하려는 의도에서 생긴 원칙 같다!


