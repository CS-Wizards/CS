# 1주차(7/24) - Java & Spring

## JVM

<aside>
💡 JVM 이란?
Java Virtual Machine 이다. (자바 가상 머신)
Java 프로그램이 실행될 때 사용되는 가상 컴퓨터 이다.

</aside>

### **Java 프로그램 실행 과정**

1. 자바 소스 코드(.java)를 작성한다.
2. 자바 컴파일러(javac)를 통해 자바 소스 코드(.java)를 자바 바이트 코드(.class)로 컴파일 한다.
3. JVM의 Class Loader는 동적 로딩에 필요한 클래스들을 로딩 및 링크하여 Runtime Data Area에 올린다.
4.  Runtime Data Area에  로딩된 바이트 코드를 Execution Engine을 통해 해석된다.
5. 변환된 class 파일을 JVM에서 JLT와 인터프리터를 이용해서 네이티브 코드로 변홚하여 실행한다

![Untitled](img/Untitled.png)

### **JVM 구성 요소**

1. **Class Loader**
2. **Excution Engine**
    1. JIT(Just In Time) 컴파일러
    2. Interpreter
    3. Garbage Collector
3. **Runtime Data Area**
    1. Method Area
    2. Heap
    3. PC Register
    4. JVM Stack
    5. Native Method Stack
4. ***Native Method Interface***
5. ***Native Method Library***

![Untitled](img/Untitled%201.png)

### JVM의 주요 기능

- 플랫폼 독립성(이식성)
- 메모리 관리
- 정적, 동적 바인딩
- 스레드 관리

### 질문

- 그럼, 자바 말고 다른 언어는 JVM 위에 올릴 수 없나요?
    
    <aside>
    💡 올릴수 있다.
    해당 언어의 컴파일러나 인터프리터가 Java 바이트 코드로 변환을 시킬 수 있으면 전부 JVM위에서 실행이 가능하다.
    ex) 코틀린, 스칼라, 그루비 등
    
    </aside>
    
- 반대로 JVM 계열 언어를 일반적으로 컴파일해서 사용할 순 없나요?
    
    <aside>
    💡 가능은 하다
    Java의 경우 GNU 프로젝트에서 만든 GCC(GNU Compiler Collection)의 Java 컴파일러를 사용하면 JVM 없이 사용이 가능하다.
    
    </aside>
    
- JVM을 사용함으로써 얻을 수 있는 장점과 단점에 대해 설명해 주세요.
    
    <aside>
    💡 장점
    1. 이식성
    2. 메모리 관리
    3. 동적 바인딩
    4. 보안
    
    </aside>
    
    <aside>
    💡 단점
    1. 실행속도 저하
    2. 메모리 사용량
    
    </aside>
    
- JVM과 내부에서 실행되고 있는 프로그램은 부모 프로세스 - 자식 프로세스 관계를 갖고 있다고 봐도 무방한가요?
    
    <aside>
    💡 아니요
    JVM 내부에서 실행되고 있는 프로그램은 JVM의 스레드를 통해 동작한다고 봐야한다.
    JVM은 자바 코드를 실행하기 위해  main() 메서드를 실행하는 스레드를 포함해 하나 이상의 스레드를 관리한다.
    또한 **완전히 독립적인 부모-자식 프로세스 관계와 달리, JVM으로 부터 메모리 할당, GC등의 지원을 받는다.**
    따라서 프로세스 - 스레드 관계라고 보는게 더 적합하다.
    
    </aside>
    

## final 키워드

<aside>
💡 final 키워드란?
해당 entity가 오직 한번 할당될 수 있음을 의미한다.

final 키워드는 변수, 메소드, 클래스에 사용될 수 있으며, 각각 의미가 약간 다르다.

final 변수 - 한번만 할당할 수 있는 상수를 정의 할 때 사용한다.
final 메소드 - 오버라이딩 할 수 없는 메소드
final 클래스 - 더 이상 상속할 수 없는 클래스

</aside>

### final에 대한 추가 정보

1. final 멤버 변수는 반드시 상수가 아니다.
    1. final의 정의는 ‘상수이다’ 가 아니라 한번만 초기화가 ‘가능하다’ 이다.
2. private 메소드와 final 클래스의 모든 메소드는 명시하지 않아도 final 처럼 동작한다.
    1. private 메소드와 final 클래스는 오버라이드가 불가능 하기 때문이다.

### final 키워드 사용시 장점

- 함수 파라미터에 키워드를 사용하면 함수 내부에서 해당 변수를 변경하는 경우 컴파일러가 에러를 발생 시켜 오류를 예방할 수 있다.
- 함수 호출 시에도 final 키워드를 사용하면 해당 변수를 다른 객체에 할당하거나 변경하는 것을 방지할 수 있다.

결국 코드의 안정성을 높이고 디버깅을 용이하게 하도록 도와준다.  → 예측 가능한 코드가 된다.

- 또한 불변 객체는 근본적으로 thread-safe 하므로, 따로 동기화를 할 필요가 없다.
- 최적화
    - 컴파일 타임에 상수로 취급될 수 있다.
    - JIT 컴파일러에서도 최적화가 가능하다.

### 질문

- 그렇다면 컴파일 과정에서, final 키워드는 다르게 취급되나요?
    
    <aside>
    💡 네 final 키워드는 변경이 불가능하기 때문에 컴파일시 상수값으로 대체합니다.
    
    </aside>
    

## 인터페이스와 추상클래스의 차이

### 인터페이스(interface)

<aside>
💡 인터페이스란?
인터페이스는 일종의 추상클래스이다.
다만 추상화의 정도가 더 높아 일반 메서드와 멤버변수를 가질 수 없다.

</aside>

### 인터페이스의 특징

- 인터페이스의 일반 메서드는 모두 추상메서드 상태이다.
- 오직 추상메서드와 상수만을 변수로 가질 수 있다.
- 모든 멤버변수는 public static final 이며 public static final는 생략이 가능하다.
- 모든 메서드는 public abstract이며 마찬가지로 생략 가능하다.

### 인터페이스의 필요성

- 추상클래스와 마찬가지로 구현의 강제로 표준화 처리
- 인터페이스를 통한 간접적인 클래스 사용으로 모듈 교체가 간편하다.
- 서로 상속관계가 없는 클래스들에게 인터페이스를 통한 관계 부여로 다형성을 확장할 수 있다. (직사각형, 직각삼각형에게 직각 인터페이스를 통해 관계부여 가능)
- 모듈 간 독립적인 프로그래밍이 가능하다.

### 인터페이스의 상속

- 인터페이스는 클래스와 다르게 다중 상속이 가능하다. (extends를 통해 상속)

![Untitled](img/Untitled%202.png)

### default 메서드

인터페이스에 선언 된 구현부가 있는 일반 메서드

### static 메서드

일반 static 메서드와 마찬가지로 별도의 객체가 필요없다.

구현체 없이 바로 인터페이스 이름으로 메서드에 접근이 가능하다.

### private 메서드

interface에 body를 가지는 메서드가 등장하면서 공통적으로 처리할 모듈이 발생했다.

이를 위해 외부로 공개할 필요가 없는 메서드 지정을 위해 private method가 추가됨

default 키워드를 사용하지 않으며, static 처리가 가능하다

### 추상클래스(abstract class)

<aside>
💡 추상클래스란?
하나 이상의 추상메서드를 가지고 있는 클래스

</aside>

### abstract class의 특징

- 클래스에 추상 메서드(구현부가 없는 메서드)가 있으므로 객체 생성이 불가능하다.
- 하지만 상위 클래스 타입으로써 자식을 참조 하는것은 가능하다.
- 추상 클래스는 상속 전용 클래스이다.
- 자식은 추상 클래스를 상속 받아서 추상 메서드를 재정의 해야한다.

### 추상 클래스를 상속 받았지만 추상 메서드를 재정의 하지 않으면?

- 해당 클래스는 추상 메서드를 가지게 된다 → 자식 클래스도 abstract을 통해 추상 클래스로 선언

### 추상 클래스를 사용하는 이유

- 구현의 강제성을 통한 기능보장, 프로그램의 안정성 향상

### 클래스의 다중상속이 불가능한 이유(다이아몬드 문제)

![Untitled](img/Untitled%203.png)

```java
// 추상 클래스
public abstract Person(){
	public abstract void speak();
}

// Person 클래스를 구현한 Father와 Mother
public class Father(){
	@Override
	public void speck(){
		System.out.println("나는 아빠");
	}
}
public class Mother(){
	@Override
	public void speck(){
		System.out.println("나는 엄마");
	}	
}

// Father와 Mother를 동시에 상속받은 Child
public class Child(){
	public void test(){
		speck(); // 어떤 speak 메서드를 호출해야 할까?
	}
}
```

위 예시처럼 다중 상속을 허용하게 되면 문제가 발생할 수 있다. 

### 질문

- 왜 클래스는 단일 상속만 가능한데, 인터페이스는 2개 이상 구현이 가능할까요?
    
    <aside>
    💡 인터페이스는 실질적인 구현을 하지 않고 정의만 하기 때문이다.
    
    </aside>
    

## **리플렉션**

<aside>
💡 리플렉션 이란?
자바는 클래스와 인터페이스의 메타 정보를 Class 객체로 관리하는데, 이러한 메타 정보를 프로그램에서 읽고 수정하는 행위를 리플렉션 이라고 한다.
(메타 정보란 패키지, 타입, 멤버 정보등을 의미 한다.)

</aside>

리플렉션은 클래스 로더에 의해 JVM 메모리 영역에 저장된 클래스의 정보를 클래스의 정보에 접근해서 사용한다.

### 단점

- 리플렉션은 컴파일 시점이 아니라 런타임 시점에 클래스를 분석한다.
    - 즉, 성능저하가 발생한다.
    - 또한 컴파일 시점에 타입 체크 기능을 사용할 수 없다 (마찬가지로 런타임에 클래스 정보를 알기 때문)
- 내부 정보를 노출 하기 때문에 캡슐화를 저해한다.

⇒ 따라서 리플렉션은 아주 제한적으로 꼭 필요한 경우에만 사용해야 한다.

### 질문

- 의미만 들어보면 리플렉션은 보안적인 문제가 있을 가능성이 있어보이는데, 실제로 그렇게 생각하시나요? 만약 그렇다면, 어떻게 방지할 수 있을까요?
    
    <aside>
    💡 private, protected 변수와 메서드에 접근이 가능하기 때문에 보안적으로 문제가 발생 할 수 있다고 생각한다.
    최소한의 권한 부여, 보안 매니저 사용(Java SecurityManager)
    
    </aside>
    
- 리플렉션을 언제 활용할 수 있을까요?
    
    <aside>
    💡 프레임워크와 라이브러리에서 사용된다. (어노테이션, JPA, 자동완성)
    사용자가 생성한 객체의 타입을 컴파일 시점에 알 수 없다,
    리플랙션을 통해 해당 클래스의 정보를 가져와서 사용한다.
    
    테스트 코드를 작성시 private 변수나 메서드에 접근해야 하는 경우
    
    </aside>
    

## **static class와 static method**

### static

클래스 로드 시에 메모리에 할당 되며, 프로그램 종료시까지 유지된다.

이때 단 한번 메모리에 할당되고, 이후 생성되는 모든 인스턴스에서 공유된다.

특히 상수값이나 공유하는 자원을 관리할 때 유용하다.

### static method

- 객체 생성 없이 사용할 수 있는 메서드이다.
- static method는 인스턴스 멤버(인스턴스 변수, 인스턴스 메소드)를 사용할 수 없다
    - 인스턴스 멤버를 사용하려면 객체 생성이 필요하기 때문에
    - static은 항상 호출이 가능해야 한다 하지만 객체 생성이 필요할 경우, 객체가 있는지 없을지 모르기 때문이다.

### static class

static class는 보통 내부 클래스를 사용할때 사용한다.

inner class의 문제점은 외부를 참조 한다는 점과 메모리 누수 현상이 발생한다는 점이다.

**외부참조**

inner class를 생성하면 내부 클래스가 외부의 멤버를 사용하지 않아도, 숨겨진 외부 참조가 생성된다.

즉, 비정적 멤버 클래스의 인스턴스는 외부 클래스의 인스턴스와 암묵적으로 연결된다.

**메모리 누수**

inner class에서 외부 클래스는 필요 없어지고 내부 클래스만 남아있는 경우 외부 클래스가 GC에 의해 사라져야 할것 같지만, 외부 참조로 인해 inner class와 연결되어 있어 사라지지 않는다.

**static inner class**

inner class를 static으로 선언하면, 외부 참조를 하지 않아 메모리 누수가 발생하지 않는다.

### 질문

- static 을 사용하면 어떤 이점을 얻을 수 있나요? 어떤 제약이 걸릴까요?
    
    <aside>
    💡 static 키워드로 선언된 변수나 메서드는 클래스 로드 시점에서 한번만 메모리에 할당되며, 모든 인스턴스가 해당 값을 공유한다.
    따라서 여러 객체가 동일한 데이터를 공유해 메모리를 절약할 수 있다.
    또한 객체를 생성하지 않고도 접근할 수 있어 전역적으로 접근해야 하는 경우 유용하다.
    
    반면, static 메서드는 인스턴스 변수와 인스턴스 메서드를 참조할 수 없다.
    또한 오버라이딩(재정의)가 불가능하며, 종료시까지 메모리에 유지되므로 메모리 문제가 발생할 수 도 있다.
    
    </aside>
    
- 컴파일 과정에서 static 이 어떻게 처리되는지 설명해 주세요.
    
    <aside>
    💡 static은 컴파일러에 의해 .java파일이 .class 파일로 변환이 되고, JVM에 .class 파일이 로드 될때 method 영역에 메모리가 할당되고 초기화 됩니다.
    
    </aside>
    

## **Java의 Exception**

### Error와 Exception

**Error(오류)**는 시스템이 종료되어야 할 수준의 상황과 같이 수습할 수 없는 **심각한 문제**를 의미한다. 개발자가 미리 예측하여 방지할 수 없다.

**Exception(예외)** 는 개발자가 구현한 로직에서 발생한 실수나 사용자의 영향에 의해 발생한다. 오류와 달리 개발자가 미리 **예측하여 방지할 수 있기에** 상황에 맞는 예외처리(Exception Handle)를 해야한다.

![Untitled](img/Untitled%204.png)

### Checked Exception

- Complie Exception이라고도 하며 Exception을 바로 상속받는다.
- 컴파일 시점에 예외를 catch 하는지 **정적으로 확인**한다. 컴파일 시점에 예외에 대해 처리(try/catch) 하지 않는다면 컴파일 에러가 발생한다.
- **트랜잭션 Rollback이 안된다.**

### Unchecked Exception

- Unchecked Exception은 RuntimeException을 상속받는다.
- 컴파일 시점에 **예외를 catch 하는지 확인하지 않는다**. 따라서 컴파일 시점에 예외가 발생하는지 판단할 수 없다.
- **트랜잭션 Rollback이 된다.**

### 예외 복구

Exception이 발생해도 어플리케이션은 정상적으로 동작함, 반복문을 통해 일정시도 만큼 재시도하여 예외 복구 시도

### 예외 처리 회피

해당 메소드를 호출한 곳으로 예외를 던짐

### 예외 전환

예외 처리 회피와 비슷하지만 더 명확한 예외로 변경해서 던짐

### throw vs throws

### `throw`

- **용도**: 예외를 명시적으로 발생시키기 위해
- **위치**: 메서드 내
- **사용 방법**: 예외 객체를 생성하여 예외를 던질 때 사용

```java
public void exampleMethod() {
    if (someCondition) {
        throw new IllegalArgumentException("잘못된 인수입니다");
    }
}
```

### `throws`

- **용도**: 메서드가 던질 수 있는 예외를 선언하기 위해
- **위치**: 메서드 선언부
- **사용 방법**: 메서드가 처리하지 않고 호출자에게 전달할 예외를 명시

```java
public void anotherMethod() throws IOException {
    // 메서드 내부에서 IOException을 던질 수 있음
    throw new IOException("입출력 예외 발생");
}
```

**정리**

- `throw`: 예외를 실제로 던질 때 사용하며, 메서드 내부에서 사용됩니다.
- `throws`: 메서드가 던질 수 있는 예외를 선언할 때 사용하며, 메서드 선언부에 위치합니다

### Exception 처리 과정

1. **예외 발생**: 예외가 발생하면 JVM은 예외 객체를 생성하고, 예외를 발생시킨 메서드의 호출 스택을 추적한다.

```java
public void method1() {
    int result = 10 / 0; // ArithmeticException 발생
}
```

1. **예외 객체 전파**: JVM은 해당 예외를 발생시킨 메서드에서 예외 처리 코드를 찾고, 예외 처리 코드가 없는 경우 예외 객체를 호출 스택의 상위 메서드로 전파시킨다.

```java
public static void main(String[] args) {
    method2(); // method2()에서 발생한 예외가 전파됨
}
```

1. **예외 처리**: 예외 객체가 상위 메서드로 전파되면, 해당 예외를 처리할 수 있는 catch 블록을 찾고, 없다면 예외는 다시 상위 메서드로 전파된다.

```java
public class Main {
    public static void method1() {
        int result = 10 / 0; // ArithmeticException 발생
    }

    public static void method2() {
        method1(); // method1()에서 발생한 예외가 전파됨
    }

    public static void main(String[] args) {
        try {
            method2(); // method2()에서 발생한 예외가 전파됨
        } catch (ArithmeticException e) { // 예외를 처리할 수 있는 catch 블록
            System.out.println("예외가 발생했습니다: " + e.getMessage());
        }
    }
}
```

1. **예외 처리 실패**: 예외 객체가 최상위 메서드까지 전파되어도, 예외를 처리할 수 있는 catch블록이 없는 경우 JVM은 예외를 처리하지 못한 것으로 판단하여 해당 예외를 처리할 수 있는 Default Exception Handler를 사용하여 예외를 처리한다.
2. **Default Exception Handler 실행**: Default Exception Handler는 예외 객체에 대한 정보를 출력하고, 해당 예외를 처리하거나 예외 객체에 대한 스냅샷 정보를 수집하여 디버깅을 위한 정보로 제공한다.

### 예외 처리 시 비용이 발생하는 이유

호출 스택을 탐색하는 과정도 비용이 발생하지만, fillInStackTrace() 메서드가 호출 스택을 순회하며 클래스명, 메서드명, 코드 줄 번호 등의 정보를 모아 stacktrace로 만드는 과정 또한 비용 증가의 원인이다. 

### fillInStackTrace() 메서드

- fillInStackTrace() 메서드는 Java의 **Throwable** 클래스에 정의되어 있다.
- 예외 객체의 현재 스레드 실행 스택 트레이스를 채워 넣는다.
- 해당 메서드가 호출되면, 현재 실행 중인 스레드의 스택 트레이스를 업데이트하여 예외 객체가 생성된 위치 대신 메서드가 호출된 위치를 기준으로 새로운 스택 트레이스를 기록 한다.

### 예외 캐싱

- static final로 선언하여 예외를 미리 캐싱 할 수 있다
- 매번 같은 종류의 예외들을 new로 생성하는 방법보다 효율적이다.

```java
public class CustomException extends RuntimeException {
     private static final InvalidUserException INVALID_USER_EXCEPTION
								 = new InvalidUserException("사용자가 유효하지 않습니다");
}
```

### 질문

- 예외처리를 하는 세 방법에 대해 설명해 주세요.
    
    <aside>
    💡 1. 예외 복구
    2. 예외 처리 회피
    3. 예외 전환
    
    </aside>
    
- CheckedException, UncheckedException 의 차이에 대해 설명해 주세요
    
    <aside>
    💡 Checked Exception은 컴파일 시점에 예외를 catch 하는지 정적으로 확인하며, 트랜잭션 롤백이 안된다.
    Unchecked Exception은 컴파일이 시점에 예외가 발생하는지 판단할 수 없고, 트랜잭션 롤백이 된다.
    
    </aside>
    
- 예외처리가 성능에 큰 영향을 미치나요? 만약 그렇다면, 어떻게 하면 부하를 줄일 수 있을까요?
    
    <aside>
    💡 fillInStackTrace() 메서드를 재정의 하거나, 예외를 캐싱을 해서 부하를 줄일 수 있다.
    
    </aside>
    

## **Synchronized**

<aside>
💡 Synchronized 키워드는 멀티스레드 환경에서 동기화를 위해 사용된다.
한번에 하나의 스레드만 특정 코드에 접근할 수 있도록 보장한다.
(연산 결과가 메모리에 써질때까지 다른 쓰레드는 대기한다) 
원자성과 가시성

</aside>

### 메서드 동기화

메서드를 선언할때 **Synchronized** 키워드를 추가해서 메서드 전체를 동기화 한다.

```java
public synchronized void synchronizedMethod() {
    // 동기화된 메서드 내용
}
```

### 블록 동기화

메서드 내의 특정 블록을 동기화 처리한다.

```java
public void methodWithSynchronizedBlock() {
    synchronized (this) {
        // 동기화된 블록 내용
    }
}
```

### 질문

- Synchronized 키워드가 어디에 붙는지에 따라 의미가 약간씩 변화하는데, 각각 어떤 의미를 갖게 되는지 설명해 주세요
    
    <aside>
    💡 메소드 전체에 잠금을 걸던가, 메소드의 일부 영역을 실행할 때만 잠금을 걸 수 있다.
    
    </aside>
    
- 효율적인 코드 작성 측면에서, Synchronized는 좋은 키워드일까요?
    
    <aside>
    💡 하나의 스레드만 임계구역에서 작업을 수행하고 나머지 스레드들은 대기하기 때문에 성능저하가 발생할 수 있다.
    또한 임계구역에 들어가기 위해 락을 얻어야 하는데 이 과정에서 데드락(교착상태)이 발생 할 수 있다.
    
    </aside>
    
- Synchronized 를 대체할 수 있는 자바의 다른 동기화 기법에 대해 설명해 주세요
    
    <aside>
    💡 논블로킹 방식중 하나인 Atomic 클래스를 통해 동기화를 할 수 있다.
    CAS를 사용해서 동기화 문제를 해결한다.
    (동시성을 보장하기 위해 Java에서 제공하는 Wrapper Class)
    
    </aside>
    
- Thread Local에 대해 설명해 주세요.
    
    <aside>
    💡 스레드 간에 데이터를 공유하지 않고, 각 스레드 마다 독립적인 변수 값을 사용할 수 있게 해준다.
    스레드 간 충돌 없이 데이터를 사용 할 수 있지만 메모리 누수가 발생할 수 있다.
    
    </aside>
    

## **Java Stream**

<aside>
💡 Stream이란?
Java 8에 도입된 기능으로 순차 및 병렬적인 집계연산을 지원하는 연속된 요소이다.
(데이터 처리 연산을 지원 하도록 소스에서 추출된 요소)
스트림은 데이터의 흐름이고 우리가 사용하는 스트림 API는 해당 데이터를 어떻게 사용하지를 다루는 파이프라인이다.

</aside>

### 질문

- Stream과 for ~ loop의 성능 차이를 비교해 주세요,
    
    <aside>
    💡 기본적으로 for-loop가 stream보다 성능이 좋지만 함수 내부의 시간 복잡도가 매우 커지면 큰 성능차이가 발생하지 않는다.
    
    </aside>
    
- Stream은 병렬처리 할 수 있나요?
    
    <aside>
    💡 Stream은 parallel() 메서드를 통해 병렬 처리가 가능하다.
    대용량 데이터를 처리 할 때는 병렬처리를 통해 성능이 개선될 수 있지만, 데이터가 적은 경우에는 오히려 성능이 저하될 수 도 있다.
    [https://dogpaw.tistory.com/6](https://dogpaw.tistory.com/6)
    
    </aside>
    
- Stream에서 사용할 수 있는 함수형 인터페이스에 대해 설명해 주세요.
    
    <aside>
    💡 함수형 인터페이스란 하나의 추상메서드만 가지는 인터페이스를 의미한다.
    이러한 인터페이스를 람다 표현식이나 메서드 참조를 통해 구현해서 사용할 수 있다.
    Predicate<T>, Function<T, R>등 다양한 함수형 인터페이스가 있다.
    
    </aside>
    
- 가끔 외부 변수를 사용할 때, final 키워드를 붙여서 사용하는데 왜 그럴까요? 꼭 그래야 할까요?
    
    <aside>
    💡 stream에서 람다 표현식을 사용하는데, 람다식은 함수형 객체로 final로 선언된 변수만 읽을 수 있기 때문이다.
    
    람다 표현식에서 final로 선언된 변수만 읽을수 있는 이유는
    람다식은 자신의 정의된 컨텍스트에서 변수를 캡쳐해 사용할 수 있는데, 해당 변수는 변경이 불가능 해야한다. ( 프로그래밍 모델과 메모리 모델의 일관성을 때문에)
    
    </aside>