**Java Stream**

- Stream과 for ~ loop의 성능 차이를 비교해 주세요,
- Stream은 병렬처리 할 수 있나요?
- Stream에서 사용할 수 있는 함수형 인터페이스에 대해 설명해 주세요.
- 가끔 외부 변수를 사용할 때, final 키워드를 붙여서 사용하는데 왜 그럴까요? 꼭 그래야 할까요?

## JVM

### JAVA

- **Write Once Run Anywhere:** 한 번 쓰면 어디서든 실행 가능하도록
- CPU마다 받아들이는 기계어가 다른 문제를 JVM으로 해결
- JVM (Java Virtual Machine)
- Java 언어도 분류로는 컴파일 언어라 실행 전에 기계어로 해석해 주는 언어
- 자바를 컴파일하면 순수한 기계어가 아닌 **Java Bytecode** 생성
- Java Bytecode: JVM을 위한 어셈블리어
- JVM은 컴퓨터가 아닌 프로그램의 일종으로 각자의 컴퓨터마다 써야 하는 기계어가 다른데, 이 환경에 맞춘 기계어로 바꾸어 주는 것
- C++이나 다른 언어도 Java Bytecode로 번역해서 실행할 수 있지 않을까? → Kotlin
- JVM 계열의 언어를 네이티브로 컴파일도 가능하지만 보통 JVM의 장점을 활용하도록 함
- JVM이 프로그램을 실행시켜 주는 shell 역할을 진행함

### JDK와 JRE

- JDK: Java 코드를 Jaba Bytecode로 번역해 주는 것
- JDK
    - 자바를 개발하기 위한 도구 모음집
    - Java 언어를 jaba Bytecode로 변환해 주는 컴파일러 javac
    - Java Bytecode를 실행해 보기 위한 JVM
    - 개발을 위한 것이기 때문에 프로그램을 실행하는 데에는 필요하지 않음
- JRE: 프로그램의 실행을 위한 것만 따로 모은 것

![1.png](img%2F1.png)
### JVM의 장점과 단점

**장점**

- **Write Once Run Anywhere:** 한 번 쓰면 어디서든 실행 가능
- 보안: 호스트 시스템으로부터 격리
- 최적화: 자동 메모리 관리와 G.C 기능 제공

**단점**

- 속도: VM을 거쳐서 코드를 실행하기 때문에 직접 하드웨어 위에서 코드를 실행하는 것보다 느림
- 자원 사용: VM은 자체적인 운영 체제와 런타임 시스템을 유지하기 때문에 추가적인 메모리와 CPU 리소스 사용

## Final

### java의 동작 원리

- 소스코드를 작성하고 컴파일하면 각 클래스는 .class라는 바이트코드 파일로 변환됨
- 런타임 때 어떤 클래스의 멤버를 사용해야 한다면, 클래스 로더는 해당 클래스의 바이트코드를 찾음
- 검증과 초기화 과정을 거쳐 JVM의 Runtime Data Area에 바이트코드 적재
- 바이트코드들은 프로그램 실행 초기에 모두 적재되는 것이 아니라 런타임 때 해당 클래스가 필요한 경우 동적으로 적재
- **final로 선언된 상수를 사용할 때는 클래스로더가 해당 클래스를 로딩하지 않음**

    ```java
    public class Test {
    	piblic static final int ZERO = 0;
    } // 클래스 로더가 로딩하지 않음
    
    public class Test {
    	piblic static int ZERO = 0;
    } // 클래스 로더가 로딩함
    ```


### final

- final 키워드를 붙이면 무언가를 제한한다, 한 번만 할당한다
- 재할당하려고 하면 컴파일 오류가 발생
- 상수 필드는 컴파일 시간에 결정

### Compile-time Constants

- 컴파일 이후 값이 절대 변경되지 않는 상수
- final 키워드가 붙은 기본 자료형 혹은 문자열

```java
public static final String USERNAME = "unknonw";
```

### Run-time Constants

- 런타임 중 값이 변경되지는 않지만 프로그램을 실행할 때마다 다른 값이 할당될 수 있는 상수

```java
final String input = console.leadLine();
final double random = Math.random();
```

- 실행될 때마다 변경될 수 있지만, 실행 이후 런타임 때는 절대 변경되지 않음

## Interface vs Abstract Class

### Abstract


>Abstract Class<br>
**추상 클래스는 하나 이상의 추상 메소드를 포함하는 클래스**이다. 추상 메소드란 메소드 선언만 있고 구현이 없는 메소드로, 구현은 상속받은 클래스에서 진행한다. 추상 클래스는 직접적으로 인스턴스화할 수 없고 상속을 통해서만 사용한다. 추상 클래스는 추상 메소드 외에도 일반적인 메소드와 인스턴스 변수도 포함시킬 수 있다.


- 추상 클래스의 주요 목적은 공통된 기능과 속성을 가진 클래스들을 모델링하고 **코드 재사용성과 확장성을 높이기 위해서**
- 자식 클래스들이 공통적으로 가지고 있는 메소드와 변수를 묶은 개념

**추상 메소드 vs 일반 메소드**

- 추상메소드
    - 메소드 선언만 있고 구현이 없음
    - 행위에만 집중
    - **추상 메소드는 하위 클래스에서 반드시 구현되어 있어야 사용이 가능**
- 일반 메소드
    - 자세하고 구체적인 구현이 포함
    - 이미 구현되어 있어서 특별한 구현이 더 필요하지 않음하
- Example
    - 추상 클래스 구성

    ```java
    public abstract class Animal {
        public abstract void makeSound();
    }
    ```

    - 이후 Animal을 상속받은 강아지, 고양이 클래스에서 해당 메소드를 구체적으로 구현한다.

    ```java
    public class Cat extends Animal {
        @Override
        public void makeSound() {
            System.out.println("야옹야옹");
        }
    }
    ```

    ```java
    public class Dog extends Animal {
        @Override
        public void makeSound() {
            System.out.println("멍멍");
        }
    }
    ```


### Interface

>**인터페이스는 추상 메소드, 상수 및 디폴트 메소드로 이루어진 참조 타입**이다. 추상 클래스와 다르게 추상 메소드만 가지고 있으며 일반 메소드의 구현은 포함하지 않는다. **인터페이스는 어떤 골격을 정의**하는 것이다.

</aside>

- 인터페이스의 주요 목적은 클래스 사이의 명세를 정의하는 것
- 인터페이스는 코드의 유연성과 재사용성을 높
- 클래스가 여러 개의 인터페이스를 구현할 수 있으므로 다양한 기능을 가진 객체 생성 가능
- **인터페이스는 클래스 간의 결합도를 낮추고 코드의 의존성을 관리하기 쉽게 해 줌**
- **JAVA에서 클래스는 다중 상속을 지원하지 않지만 인터페이스는 지원**
- 인터페이스는 여러 개의 인터페이스를 상속받을 수 있음
- 인터페이스 간의 다중 상속으로 인해 충돌하는 메서드 구현이 있을 경우 해당 클래스에서 그 메소드를 재정의해야 함
- 인터페이스를 상속받은 클래스에서는 인터페이스에서 정의한 모든 추상 메서드를 구현해야 함
- **같은 인터페이스를 사용하는 클래스들은 동일한 행위를 한다는 것을 보장**

>의존 관계를 맺을 때 변화하기 쉬운 것 또는 자주 변화하는 것보다는 변화하기 어려운 것, **거의 변화가 없는 것에 의존**해야 한다. 한마디로 구체적인 클래스보다 인터페이스나 추상 클래스와 관계를 맺어야 한다.

</aside>

- 하나의 인터페이스는 여러 개의 인터페이스를 `extends` 키워드를 통해 상속받을 수 있음
- 이때 상속받은 인터페이스의 메소드들을 선택적으로 구현 가능
- Example
    - Interface 작성

    ```java
    public interface Shape {
        public double calculateArea();
    }
    ```

    - 이후 Shape을 상속받은 클래스 Circle에서 해당 메소드를 구체화

    ```java
    public class Circle implements Shape{
        private double radius;
    
        public Circle(double radius) {
            this.radius = radius;
        }
    
        @Override
        public double calculateArea() {
            return (radius * radius * 3.14);
        }
    }
    ```

    - 이를 확장시켜서 둘레를 구하는 `calculaterPerimeter` 함수도 인터페이스에 정의한 뒤, Circle과 Rectangel 클래스를 나누어서 작성

    ```java
    public interface Shape {
        public double calculateArea();
        public double calculaterPerimeter();
    }
    ```

    - 이후 Circle과 Rectangle 클래스에서 구현해 준다.

    ```java
    public class Circle implements Shape{
        private double radius;
    
        public Circle(double radius) {
            this.radius = radius;
        }
    
        @Override
        public double calculateArea() {
            return (radius * radius * 3.14);
        }
    
        @Override
        public double calculaterPerimeter() {
            return (2 * radius * 3.14);
        }
    }
    ```

    ```java
    public class Rectangle implements Shape {
        private double height;
        private double width;
    
        public Rectangle(double height, double width) {
            this.height = height;
            this.width = width;
        }
    
        @Override
        public double calculateArea() {
            return (height * width);
        }
    
        @Override
        public double calculaterPerimeter() {
            return (2 * (height + width));
        }
    }
    ```


### Abstract Class vs Interface

**추상 클래스와 인터페이스 공통점**

- 둘 모두 추상 메서드를 포함할 수 있고 기능 확장에 용이하며 다형성을 지원
- 클래스 간의 관련성을 나타내는 용도로 사용 가능

**추상 클래스와 인터페이스 차이점**

- 추상 클래스는 추상 메소드 외에도 일반 메소드와 인스턴스 변수를 가질 수 있지만, 인터페이스는 추상 메소드, 디폴트 메소드, 정적 메소드, 상수만을 가질 수 있음
- 또한 추상 클래스는 단일 상속을 가지지만 인터페이스는 다중 상속을 지원
- **추상 클래스는 상속에 가깝고 인터페이스는 구현에 가까움**
- 추상 클래스의 상속은 is-a 관계지만 인터페이스의 구현은 has-a 관계

>Is-a 관계는 객체 지향 프로그래밍에서 사용되는 상속 관계를 의미한다. 이 관계는 클래스 간의 계층 구조를 형성하며, **한 클래스가 다른 클래스의 하위 클래스인 경우**를 나타낸다.
<br><br>Is-a 관계는 "A는 B이다"라는 문장으로 표현될 수 있다. 예를 들어, "사람은 동물이다"라는 문장에서 사람 클래스는 동물 클래스의 하위 클래스로서 Is-a 관계를 가지고 있다.
<br><br>Is-a 관계는 상속을 통해 구현됩니다. 하위 클래스는 상위 클래스의 특성과 동작을 상속받아 사용할 수 있습니다. 이를 통해 코드의 **재사용성을 높일** 수 있고, 객체 지향의 **다형성 개념을 활용**할 수 있습니다.

</aside>

>Has-a 관계는 객체 지향 프로그래밍에서 사용되는 연관 관계를 의미한다. 이 관계는 **한 클래스가 다른 클래스를 포함하고 있는 경우**를 나타낸다.
<br><br>Has-a 관계는 "A는 B를 가지고 있다"라는 문장으로 표현될 수 있다. 예를 들어, "자동차는 엔진을 가지고 있다"라는 문장에서 자동차 클래스는 엔진 클래스를 포함하고 있어 Has-a 관계를 가지고 있다.
<br><br>Has-a 관계는 **클래스 간의 구성(composition) 또는 집합(aggregation) 관계**로 구현될 수 있다. 구성 관계는 **한 클래스가 다른 클래스의 부분으로서 완전히 종속되는 관계**를 나타내며, 부분 객체의 생명 주기와 전체 객체의 생명 주기가 밀접하게 연관된다. 집합 관계는 한 클래스가 다른 클래스의 일부를 포함하지만, 포함된 객체의 생명 주기와 전체 객체의 생명 주기가 독립적이다.

</aside>

## Reflection

### 정적 바인딩 vs 동적 바인딩

- 바인딩: 프로그램에 사용된 구성 요소의 실제값 또는 프로퍼티를 결정짓는 행위

| 정적 바인딩 (Static Binding) | 동적 바인딩 (Dynamic Binding) |
| --- | --- |
| 컴파일 시간에 결정 | 실행 시간에 결정 |
| 프로그램이 실행되어도 변하지 않음 | 늦은 바인딩 |
| 오버로딩 | 오버라이딩 |
| private, final, static 붙은 메서드 | Java에서 다형성, 상속이 가능한 이유 |

### Reflection

- 런타임에 Class Type을 모르는 채로 객체를 생성하고 이용하기에 동적 바인딩 제공
- 동적으로 클래스를 사용해야 할 경우
- Test Code에서의 private 변수를 변경하고 싶거나 private method를 테스트할 경우
- IDE에서 자동 Mapping 기능을 사용할 때
- Jackson, GSON 등의 JSON Serialization
- Reflection은 Class/Interface, Constructor, Method, Field를 가져올 수 있음

### Security

- 리플렉션을 통해 제한자를 우회하여 private 필드에 접근하는 등의 보안 문제 발생 가능
- 이런 문제를 방지하기 위해 최소한의 권한을 부여하거나 코드 검사와 테스트를 통해 조심스러운 사용 필요

## static class & static method

### static

- 자바가 컴파일된 후 클래스 로더로 실행할 때 생성됨
- 같은 클래스라면 하나의 static field, static method만 가지게 됨
- JVM이 시작될 때 static 영역에 한 번 저장되어 프로그램이 종료될 때 해제되는 것
- 모든 객체가 공유하는 메모리라는 장점을 가지지만 Garbage Collection이 관리하지 않음

### static class

- 가장 바깥의 클래스는 static class
- non-static class는 static, non-static class 모두 상속받을 수 있음
- static class는 static class만 상속받을 수 있음
- static class의 인스턴스는 서로 동일하지 않음
    - 메모리 주소가 다름
- Outer class를 참조할 일이 없다면 Inner class는 static을 붙여 static nested class로 사용하는 것이 좋음
- static 내부 클래스는 자신의 바깥 클래스 인스턴스의 임시적 참조를 유지하지 않음
    - 메모리 누수의 일반적인 원인 예방 가능
    - 클래스의 각 인스턴스 당 더 적은 메모리 사용
    - static이 아닐 경우에 외부는 필요 없고 내부만 필요하다면 외부 클래스를 내부가 참조하고 있기 때문에 외부를 제거해 주지 않아서 메모리 누수 발생
- static inner class는 외부 객체를 만들지 않아도 생성 가능

## Exception

### Error & Exception
![2.png](img%2F2.png)

**Error**

- 시스템이 뭔가 비정상적인 상황이 발생한 경우
- 주로 JVM이 발생시키는 것이며 이는 애플리케이션 코드에서 잡으려고 해도 방법이 없음
- `OutOfMemoryError`, `ThreadDeath`, `StackOverflowError`

**Exception**

- 입력 값에 대한 처리가 불가능하거나 프로그램 실행 중에 참조된 값이 자롬ㅅ된 경우
- 프로그램 흐름에 어긋나게 하는 상황
- 로직에서 발생한 실수 혹은 사용자의 영향

### CheckedException and UncheckedException
![3.png](img%2F3.png)

**UncheckedException**

- RuntimeException을 상속받는 모든 예외
- 컴파일 시점에 예외를 catch하는지 확인하지 않음
- 컴파일 시점에 예외 발생 유무 확인 불가능
- 예외가 발생하는 메서드에서 throws 예약어를 활용해 예외를 처리할 필요가 없음

**CheckedException**

- RuntimeException을 상속받지 않는 모든 예외
- Compile Exception이라고도 함
- 컴파일 시점에 예외를 catch하는지 정적으로 확인
- 컴파일 시점에 예외에 대한 처리가 안 되어 있다면 with try/catch 컴파일 에러가 발생

### 예외 처리 방법

- 예외 복구
    - 예외가 발생해도 앱이 정상적으로 작동하도록 함

    ```java
    static int MAX_RETRY = 10;
    
    int retry = MAX_RETRY;
    while (retry > 0) {
    	retry -= 1;
    	try{
        	
        } catch(SomeException e) {
        	
        } finally {
        	
        }
    }
    // MAX_RETRY 초과 시 예외 Throw
    throw new MAXTRYEXCEPTION();
    ```

- 예외 처리 회피
    - 예외가 발생하면 throws를 통해 호출한 쪽으로 던지고 자신의 책임 제거

    ```java
    // 예시 1
    public void add() throws SQLException {
    }
    
    // 예시 2 
    public void add() throws SQLException {
        try {
        } catch(SQLException e) {
            throw e;
        }
    }
    ```

- 예외 전환
    - 호출한 쪽에 더욱 명확하게 인지할 수 있도록 던진다

    ```java
    public void add(User user) throws DuplicateUserIdException, SQLException {
        try {
        } catch(SQLException e) {
            if(e.getErrorCode() == MysqlErrorNumbers.ER_DUP_ENTRY) {
                throw DuplicateUserIdException();
            }
            else throw e;
        }
    }
    ```


### 성능

- 예외가 발생하면 JVM은 예외를 처리할 수 있는 Exception Handler가 포함된 메서드를 찾기 위해 Call Stack을 검색
- 검색 과정에서 비용 증가가 발생
- Call Stack을 순회하면서 클래스명, 메서드명, 코드 줄번호 등 정보를 수집하여 stackTrace로 정보를 만들기 때문에 비용 크게 증가

### 해결 방법

- Return Object: 예외 발생 대신 empty 객체를 리턴하거나 다른 적절한 응답으로 처리
- Custom Exception: Call Stack 순회 과정을 줄이고 앞단에서 에러 처리

## Synchronized

### Synchronized

- 멀티 쓰레드 프로세스에서는 다른 쓰레드의 작업에 영향을 미칠 수 있음
- 진행 중인 작업이 다른 쓰레드에게 간섭받지 않게 하려면 동기화가 필요
- 동기화를 위해서는 간섭받지 않아야 하는 문장을 임계 영역을 설정
- 임계 영역은 Lock을 얻은 단 하나의 쓰레드만 출입 가능
- 메서드 전체 영역을 임계 영역으로 지정

    ```java
    public synchronized void withdraw(int money){
        	if(balance >= money){
            	try{
                	Thread.sleep(1000);
                }catch(Exception e){}
                balance -= money;
            }
        }
    ```

- 특정한 영역을 임계 영역으로 지정: synchronized(객체의 참조 변수) {}

    ```java
    public synchronized void withdraw(int money){
    	synchronized(this) {
        	if(balance >= money){
            	try{
                	Thread.sleep(1000);
                }catch(Exception e){}
                balance -= money;
            }
        }
    }
    ```


### ThreadLocal

- thread마다 독립적인 변수를 가질 수 있게 해 주는 것
- 스레드 단위로 로컬 변수를 사용할 수 있기 때문에 마치 전역변수처럼 여러 메서드에서 활용할 수 있음
- 아래와 같이 활용

    ```java
    public void set(T value) {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null) {
            map.set(this, value);
        } else {
            createMap(t, value); 
        }
    }
    ```


## Stream

### Java Stram

- 람다를 활용해 배열과 컬렉션을 함수형으로 간단하게 처리할 수 있는 기술
- 기존의 for 문과 Iterator를 사용하면 코드가 길어져서 가독성과 재사용성이 떨어지며 데이터 타입마다 다른 방식으로 다뤄야 하는 불편함
- 스트림은 데이터 소스를 추상화하고, 데이터를 다루는 데 자주 사용되는 메소드를 정의해 두어서 소스에 상관없이 모두 같은 방식으로 다룰 수 있어 코드의 재사용성이 높아짐
- 원본 데이터 소스를 변경하지 않고 읽기만 함
- 일회용이라 한 번 사용하면 닫혀서 재사용이 불가능
- 최종 연산 전까지 중간 연산을 수행하지 않음
- 작업을 내부 반복으로 처리
- 병렬 처리가 쉬움 → 멀티쓰레드 사용
- 기본형 스트림을 제공하여 오토박싱과 언박싱 등의 불필요한 과정 생략
    - Stream<Integer>이 아닌 IntStream

### Stream 만들기

- 배열 스트림 : Arrays.stream()

```java
String[] arr = new String[]{"a", "b", "c"};
Stream<String> stream = Arrays.stream(arr);
```

- 컬렉션 스트림: .stream()

```java
List<String> list = Arrays.asList("a","b","c");
Stream<String> stream = list.stream();
```

- Stream.builder()

```java
Stream<String> builderStream = Stream.<String>builder()
    .add("a").add("b").add("c")
    .build();
```

- 람다식 Stream.generate(), iterate()

```java
Stream<String> generatedStream = Stream.generate(()->"a").limit(3);
// 생성할 때 스트림의 크기가 정해져있지 않기(무한하기)때문에 최대 크기를 제한해줘야 한다.

Stream<Integer> iteratedStream = Stream.iterate(0, n->n+2).limit(5); //0,2,4,6,8
```

- 기본 타입형 스트림

```java
IntStream intStream = IntStream.range(1, 5); // [1, 2, 3, 4]
```

- 병렬 스트림: parallelStream()

```java
Stream<String> parallelStream = list.parallelStream();
```

### Stream Operation

- Calculating: ****기본형 타입을 사용하는 경우 스트림 내 요소들로 최소, 최대, 합, 평균 등을 구하는 연산을 수행

    ```java
    IntStream stream = list.stream()
    	.count()   //스트림 요소 개수 반환
        .sum()     //스트림 요소의 합 반환
        .min()     //스트림의 최소값 반환
        .max()     //스트림의 최대값 반환
        .average() //스트림의 평균값 반환
    ```

- Reduction: 스트림의 요소를 하나씩 줄여가며 누적 연산을 수행

    ```java
    IntStream stream = IntStream.range(1,5);
    	.reduce(10, (total, num)-> total + num);
        //reduce(초기값, (누적 변수,요소)->수행문)
        // 10 + 1+2+3+4+5 = 25
    ```

- Collecting: 스트림의 요소를 원하는 자료형으로 변환

    ```java
    //예시 리스트
    List<Person> members = Arrays.asList(new Person("lee",26),
    									 new Person("kim", 23),
    									 new Person("park", 23));
    
    // toList() - 리스트로 반환
    members.stream()
    	.map(Person::getLastName)
        .collect(Collectors.toList());
        // [lee, kim, park]
    
    // joining() - 작업 결과를 하나의 스트링으로 이어 붙이기
    members.stream()
    	.map(Person::getLastName)
        .collect(Collectors.joining(delimiter = "+" , prefix = "<", suffix = ">");
        // <lee+kim+park>
    
    //groupingBy() - 그룹지어서 Map으로 반환
    members.stream()
    	.collect(Collectors.groupingBy(Person::getAge));
    	// {26 = [Person{lastName="lee",age=26}],
        //  23 = [Person{lastName="kim",age=23},Person{lastName="park",age=23}]}
    
    //collectingAndThen() - collecting 이후 추가 작업 수행
    members.stream()
    	.collect(Collectors.collectingAndThen (Collectors.toSet(),
        									   Collections::unmodifiableSet));
    	//Set으로 collect한 후 수정불가한 set으로 변환하는 작업 실행
    ```

- Matching 특정 조건을 만족하는 요소가 있는지 체크한 결과를 반환
    - anyMatch (하나라도 만족하는 요소가 있는지)
    - allMatch( 모두 만족하는지)
    - noneMatch (모두 만족하지 않는지)

    ```java
    List<String> members = Arrays.asList("Lee", "Park", "Hwang");
    boolean matchResult = members.stream()
    						.anyMatch(members->members.contains("w")); //w를 포함하는 요소가 있는지, True
    
    boolean matchResult = members.stream()
    						.allMatch(members->members.length() >= 4); //모든 요소의 길이가 4 이상인지, False
    
    boolean matchResult = members.stream()
    						.noneMatch(members->members.endsWith("t")); //t로 끝나는 요소가 하나도 없는지, True
    
    ```

- Iterating: forEach로 스트림을 돌면서 실행되는 작업

    ```java
    members.stream()
    	.map(Person::getName)
        .forEach(System.out::println);
        //결과를 출력 (peek는 중간, forEach는 최종)
    ```

- Finding: 스트림에서 하나의 요소를 반환

    ```java
    Person person = members.stream()
    					.findAny()   //먼저 찾은 요소 하나 반환, 병렬 스트림의 경우 첫번째 요소가 보장되지 않음
                        .findFirst() //첫번째 요소 반환
    ```


### Stream 성능

- Stream은 함수형 프로그래밍 언어에서 이야기하는 sequence와 동일한 용어
- sequence: task의 순서를 나열한 것
- sequential programming: sequence대로 일을 처리하고 함수를 파라미터로 넘기는 행위
- stream은 자료구조를 어떻게 다룰지를 논의
- 스트림을 반환한다는 것은 연산의 파이프라인을 반환한다는 것
    - 스트림은 lazy한데 매번 중간 연산마다 조건을 실행하지 않음
    - 대신 중간 연산마다 연산의 파이프라인을 리턴
    - 최종 연산 과정에 들어와서야 이전 중간 연산들을 모두 합친 후 최종 연산에 돌입
- JIT Compiler가 for-loop을 워낙 40년 이상 다뤄왔다 보니까 for-loop에 대한 internal optimization이 이미 잘 되어 있음
- 하지만 스트림은 2015년 이후에 도입되었으므로 아직 컴파일러가 최적화를 제대로 하지 못함