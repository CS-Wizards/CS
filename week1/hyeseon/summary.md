# JVM
__개념__<br/>
Java Virtual Machine의 약자로, 자바 프로그램이 실행되는 환경을 제공하는 가상 컴퓨터입니다.<br/>
JVM은 자바 프로그램이 다양한 운영체제와 하드웨어에 독립적으로 실행될 수 있도록 합니다.

__기능__<br/>
1. 메모리 관리<br/>   
JVM은 힙, 스택, 메소드 영역, 프로그램 카운터 등의 메모리 영역을 관리합니다.

2. 가비지 컬렉션(GC, Garbage Collection)<br/>   
사용되지 않은 객체를 자동으로 메모리에서 해제하여 메모리 누수를 방지합니다.

3. 컴파일<br/>   
바이트 코드(.class)를 기계어로 변환하여 실행합니다. 이 과정에서 JIT(Just-In-Time) 컴파일러가 사용됩니다.

__동작 원리__<br/>

<img src="https://github.com/chunghye98/KB_CS_Study/assets/57451700/be8f4003-26a3-4fc2-b7cf-cc003e4b3b1c" width="300px">

1. 자바 클래스 로더(Class Loader)가 바이트코드 파일(.class)을 JVM의 런타임 데이터 영역(Runtime Data Areas) 로드합니다.
2. JVM은 바이트 코드를 `Linking` 합니다. 이 단계는 다음과 같이 세 단계로 나눌 수 있습니다.
   1. **검증**(Verifying): 바이트코드가 자바 언어 명세 및 JVM 명세에 명시된 대로 잘 구성되어 있는지 검사합니다.   
   2. **준비**(Preparing): static 변수를 각 유형의 기본값으로 초기화합니다. 예를 들어, int 타입은 0으로, reference 타입은 null로 초기화합니다.    
   3. **분석**(Resolving): 심볼릭 레퍼런스(ex. 메서드명, 클래스명, 필드명)를 실제 메모리 레퍼런스로 변환합니다.    
   4. **초기화**(Initializing): 클래스 초기화 함수와 클래스에 작성된 static 초기화 함수를 모두 합쳐 실행합니다.
3. 실행 엔진을 통해 바이트 코드가 실행됩니다. 실행 엔진은 바이트코드를 기계어로 컴파일해야 하며, 그 방식은 다음 두 가지가 있습니다.    
   1. **인터프리터**: 바이트 코드 명령어를 하나씩 읽어서 해설하고 실행합니다. 기본적으로 이 방식으로 동작합니다.    
   2. **JIT(Just-In-Time) 컴파일러**: 바이트코드 전체를 컴파일하고 이후에는 해당 메서드를 컴파일된 언어로 직접 실행하는 방식입니다. 캐시에 보관하기 때문에 한 번 컴파일된 코드는 계속 빠르게 수행됩니다. 따라서, 한 번만 실행되는 코드는 인터프리터를 사용하고, 자주 사용하는 메서드는 JIT 컴파일러를 사용합니다.

__JVM의 Runtime Data Area__<br/>

<img src="https://github.com/VSFe/Tech-Interview/assets/57451700/395d8d07-61a5-4ef7-a925-114d0600b84f" width="300px">

자바 프로그램이 실행되는 동안 JVM이 관리하는 메모리 구조입니다.
1. **메소드 영역**<br/>
모든 스레드가 공유하는 영역으로, JVM이 로드한 각 클래스 또는 인터페이스에 대한 정보가 저장됩니다. 이 정보에는 변수, 메서드, static 변수, 인터페이스 등이 포함됩니다.   
   - **런타임 상수 풀**<br/>
   각 클래스와 인터페이스의 상수뿐만 아니라, 메서드와 필드에 대한 모든 레퍼런스까지 담고 있는 테이블입니다. 즉, 어떤 메서드나 필드를 참조할 때 JVM은 런타임 상수 풀을 통해 해당 메서드나 필드의 실제 메모리상 주소를 찾아서 참조합니다.
2. **힙(Heap)**<br/>
인스턴스 또는 객체를 저장하는 공간으로 GC의 관리 대상입니다.
3. **스택(Stack)**<br/> 
각 스레드마다 별도로 존재하며, 메서드 호출 시마다 프레임이 생성됩니다. 한 프레임마다 지역 변수, 파라미터, 반환 값, 연산 중간 값이 저장됩니다.
4. **프로그램 카운터(Program Counter, PC)**<br/>
각 스레드마다 별도로 존재하며, JVM 명령어의 현재 위치를 가리키는 레지스터입니다.
5. **Native Method Stack**<br/>
자바 언어 외의 언어로 작성된 네이티브 코드를 위한 스택입니다.

## 그럼, 자바 말고 다른 언어는 JVM 위에 올릴 수 없나요?
가능합니다.<br/>
JVM은 자바 바이트 코드를 실행하는 가상 머신이기 때문에 자바 바이트 코드로 변환될 수 있는 언어는 JVM 위에서 동작 가능합니다. <br/>
예를 들어, Kotlin, Groovy 등의 언어가 있습니다.

## 반대로 JVM 계열 언어를 일반적으로 컴파일해서 사용할 순 없나요?
가능합니다.<br/>
각 언어는 자신만의 컴파일러를 가지고 있으며, 이 컴파일러로 소스 코드를 자바 바이트 코드로 변환할 수 있습니다.

## VM을 사용함으로써 얻을 수 있는 장점과 단점에 대해 설명해 주세요.
장점으로는 하드웨어나 OS에 독립적으로 사용할 수 있다는 점이 있습니다. 플랫폼에 독립적이기 때문에 대중적인 프로그램을 만들 경우 적합합니다.<br/>
단점으로는 성능 오버헤드가 발생할 수 있다는 점이 있습니다. 직접 실행하는 것보다 계층이 하나 더 추가되기 때문에 더 좋은 성능을 바라는 경우 적합하지 않을 수 있습니다.

## JVM과 내부에서 실행되고 있는 프로그램은 부모 프로세스 - 자식 프로세스 관계를 갖고 있다고 봐도 무방한가요?
아닙니다.<br/>
프로세스는 부모 프로세스에서 `fork`하여 자식 프로세스가 생성됩니다. JVM 내부에서 실행되고 있는 GC와 같은 프로그램들은 스레드로 동작하고 있기 때문에 부모-자식 관계로 볼 수 없습니다.
---
# final 키워드
final 키워드는 해당 요소의 변경을 제한(오버라이딩 금지)하는 역할을 하며 클래스, 변수, 메서드에 사용될 수 있습니다. 자바에서 불변성 및 안정성을 제공하는 역할을 합니다. final 키워드를 사용함으로써 코드의 안정성과 예측 가능성을 높이고, 의도치 않은 변경으로부터 보호할 수 있습니다. 
1. final 변수<br/>
한 번 초기화되면 더 이상 값을 변경할 수 없습니다.
2. final 메서드 <br/>
하위 클래스에서 오버라이드 할 수 없습니다. 설계에 있어 중요한 메서드를 보호하는 데 사용합니다.
3. final 클래스 <br/>
상속될 수 없는 클래스를 만듭니다. 예를 들어, `java.lang.String` 클래스와 같이 보안과 안정성이 필요한 클래스는 final로 선언되어 있습니다.

## 그렇다면 컴파일 과정에서, final 키워드는 다르게 취급되나요?
그렇습니다.<br/>
final 키워드가 변수에 사용된다면 상수로 간주합니다.<br/>
final 키워드가 메서드에 사용된다면 메서드를 오버라이드할 수 없으므로 해당 메서드를 정적 바인딩(컴파일 타임에 호출할 메서드 결정)으로 처리합니다.<br/>
final 키워드가 클래스에 사용된다면 클래스를 상속할 수 없으므로 클래스에 속한 모든 메서드를 정적 바인딩하여 직접 호출할 수 있습니다.
---
# 인터페이스와 추상 클래스의 차이
인터페이스와 추상클래스는 자바에서 추상화의 두 가지 방식입니다.<br/>

__인터페이스__<br/>
클래스가 구현해야 하는 메서드의 집합입니다. 기본적으로 추상 메서드만을 포함하며, 필드로는 상수만 사용할 수 있습니다.`implements` 키워드를 사용하여 구현할 수 있으며 다중 상속이 가능합니다.    

__추상 클래스__<br/>
구현되지 않은 추상 메서드뿐만 아니라 구현된 메서드와 필드를 가질 수 있습니다. 추상클래스는 다른 클래스가 상속받아야 하는 공통적인 동작과 상태를 정의할 수 있습니다. `extends` 키워드를 사용하여 구현할 수 있으며 단일 상속만 가능합니다.

## 왜 클래스는 단일 상속만 가능한데, 인터페이스는 2개 이상 구현이 가능할까요?
추상클래스는 부모-자식 간의 강한 관계를 가집니다. 상태 공유가 가능하기 때문에 많은 부모 클래스들에 같은 이름의 메서드가 정의되어 있을 때 자식 클래스에서 특정한 부모의 메서드를 사용하려고 할 때 복잡성이 증가할 수밖에 없습니다. 따라서 추상클래스는 단일 상속만 가능하게 만들었습니다. <br/>
반면 인터페이스는 상태 공유가 불가하여 다중 상속 시 발생하는 모호성 문제가 발생하지 않습니다. 따라서 다중 상속이 가능하게 만들었습니다.
---
# 리플렉션
__Reflection__<br/>
구체적 타입을 알지 못해도 런타임에 그 클래스의 정보(메서드, 타입, 변수 등)에 접근할 수 있게 해주는 자바 API입니다. 이는 런타임에 JVM의 메서드영역에 접근하기 때문에 정보를 알 수 있는 것입니다. 스프링이 실행 시점에 빈을 주입할 수 있는 것과 JPA의 Entity나 Response DTO가 기본 생성자를 꼭 가져야 하는 이유이기도 합니다. <br/>

Reflection은 JVM의 메모리 영역에 저장된 클래스 데이터에서 필요한 정보들을 꺼내옵니다. 다음 코드를 살펴봅시다.
```java
    // 자바의 다형성 덕분에 Object 객체 생성은 가능하다.
    public static void main(String[] args) {    
        Object obj = new Car("foo", 0);    
        obj.move(); // Car의 move() 메서드를 사용할 수 없다, 컴파일 에러!
    }
```
생성된 obj 라는 객체는 Object라는 클래스만 알 뿐 Car 클래스라는 구체적인 타입은 모릅니다. 이것을 가능하게 해주는 기능이 Relection입니다.
```java
    public static void main(String[] args) throws Exception {
        Object obj = new Car("foo", 0);
        Class carClass = Car.class;
        Method move = carClass.getMethod("move");
        
        // move 메서드 실행, invoke(메서드를 실행시킬 객체, 해당 메서드에 넘길 인자)
        move.invoke(obj, null);    
        Method getPosition = carClass.getMethod("getPosition");    
        int position = (int)getPosition.invoke(obj, null);    
        System.out.println(position);    // 출력 결과: 1
    }
```
Reflection API로 클래스 타입 Car를 알지 못해도 obj가 move 메서드에 접근 가능합니다.

## 의미만 들어보면 리플렉션은 보안적인 문제가 있을 가능성이 있어보이는데, 실제로 그렇게 생각하시나요? 만약 그렇다면, 어떻게 방지할 수 있을까요?
있습니다. <br/>
개발자가 비공개하고자 하는 필드나 메서드에 접근 가능하기 때문에 의도하지 않은 방식으로 프로그램을 동작하게 만들 수 있습니다. <br/>
이는 보안 관리자를 설정하여 방지할 수 있습니다. 자바 애플리케이션에 보안 관리자를 설정하여 Reflection을 통해 민감한 정보에 접근하는 것을 방지할 수 있습니다.
```java
System.setSecurityManager(new SecurityManager());
```

## 리플렉션을 언제 활용할 수 있을까요?
스프링이 실행 시점에 빈을 주입할 때와 개발 시 dto를 직렬화 또는 역직렬화할 때 사용 가능합니다.

---

## static class와 static method를 비교해 주세요.

static class 는 인스턴스화 될 수 없는 클래스를 의미합니다. 주로 정적 메소드, 정적 속성을 포함할 때 사용합니다. 예를 들어, 유틸리티 함수나 상수를 모아둘 때 사용합니다. 인스턴스화가 불가능하고 캡슐화를 하기 어려워 객체지향프로그래밍을 추구하는 Java에서는 이 static class를 사용하지 못하게 만들었지만 inner static class로는 사용할 수 있습니다. 예를 들어, Java의 Collections 클래스가 있습니다.
```java
public class Collections {
    // Suppresses default constructor, ensuring non-instantiability.
    private Collections() {
    }
    static class UnmodifiableCollection<E> implements Collection<E>, Serializable {
        private static final long serialVersionUID = 1820017752578914078L;

        final Collection<? extends E> c;

        UnmodifiableCollection(Collection<? extends E> c) {
            if (c==null)
                throw new NullPointerException();
            this.c = c;
        }

        public int size()                          {return c.size();}
        public boolean isEmpty()                   {return c.isEmpty();}
        public boolean contains(Object o)          {return c.contains(o);}
        public Object[] toArray()                  {return c.toArray();}
        public <T> T[] toArray(T[] a)              {return c.toArray(a);}
        public <T> T[] toArray(IntFunction<T[]> f) {return c.toArray(f);}
        public String toString()                   {return c.toString();}

        public Iterator<E> iterator() {
            return new Iterator<E>() {
                private final Iterator<? extends E> i = c.iterator();

                public boolean hasNext() {return i.hasNext();}
                public E next()          {return i.next();}
                public void remove() {
                    throw new UnsupportedOperationException();
                }
                @Override
                public void forEachRemaining(Consumer<? super E> action) {
                    // Use backing collection version
                    i.forEachRemaining(action);
                }
            };
        }

        public boolean add(E e) {
            throw new UnsupportedOperationException();
        }
        public boolean remove(Object o) {
            throw new UnsupportedOperationException();
        }

        public boolean containsAll(Collection<?> coll) {
            return c.containsAll(coll);
        }
        public boolean addAll(Collection<? extends E> coll) {
            throw new UnsupportedOperationException();
        }
        public boolean removeAll(Collection<?> coll) {
            throw new UnsupportedOperationException();
        }
        public boolean retainAll(Collection<?> coll) {
            throw new UnsupportedOperationException();
        }
        public void clear() {
            throw new UnsupportedOperationException();
        }

        // Override default methods in Collection
        @Override
        public void forEach(Consumer<? super E> action) {
            c.forEach(action);
        }
        @Override
        public boolean removeIf(Predicate<? super E> filter) {
            throw new UnsupportedOperationException();
        }
        @SuppressWarnings("unchecked")
        @Override
        public Spliterator<E> spliterator() {
            return (Spliterator<E>)c.spliterator();
        }
        @SuppressWarnings("unchecked")
        @Override
        public Stream<E> stream() {
            return (Stream<E>)c.stream();
        }
        @SuppressWarnings("unchecked")
        @Override
        public Stream<E> parallelStream() {
            return (Stream<E>)c.parallelStream();
        }
    }
}
```

static method는 클래스의 인스턴스에 속하지 않는 메소드입니다. 클래스 자체에서 호출할 수 있으며, 인스턴스화 없이 사용할 수 있습니다. 프로그램 실행 시 JVM의 runtime data area에 로드 시 method area에 올라가기 때문에 모든 쓰레드가 공유 가능합니다. 즉, 미리 메모리에 올라가있고 GC가 삭제하지 않기 때문에 접근이 빠릅니다.
예를 들어, Java의 Math 클래스가 있습니다.

__Java의 Math 클래스__
```java
public final class Math {

    private Math() {}

    public static final double E = 2.7182818284590452354;
    public static final double PI = 3.14159265358979323846;
    private static final double DEGREES_TO_RADIANS = 0.017453292519943295;
    private static final double RADIANS_TO_DEGREES = 57.29577951308232;

    @HotSpotIntrinsicCandidate
    public static double sin(double a) {
        return StrictMath.sin(a); 
    }
    ...
}
```

### static 을 사용하면 어떤 이점을 얻을 수 있나요? 어떤 제약이 걸릴까요?
__static__ 키워드를 통해 Static 영역에 할당된 메모리는 JVM에서 static 영역에서 관리됩니다. 모든 객체가 공유하는 메모리라는 장점을 가지지만, GC의 관리 영역 밖이기 때문에 프로그램 종료 시까지 메모리가 할당되기 때문에 많이 사용할 경우 시스템의 퍼포먼스에 영향을 줄 수 있습니다.

### 컴파일 과정에서 static 이 어떻게 처리되는지 설명해 주세요.
컴파일러는 static으로 선언된 변수와 메소드, 초기화 블록을 클래스 수준에서 인식하고, 해당 정보를 .class 파일에 기록합니다.
클래스 로딩 단계에서 클래스 초기화가 이루어지는데, 이때 static 초기화 블록도 실행되며 JVM의 method data area에 로드됩니다. 

---
# Java의 Exception
Java에서 Exception은 프로그램 실행 중에 발생할 수 있는 예외적인 상황을 처리하기 위해 사용됩니다. 예외는 프로그램의 정상적인 흐름을 방해하는 이벤트를 말합니다. <br/>
Java의 예외는 Checked Exception과 UncheckedException으로 나눌 수 있습니다. 

## 예외처리를 하는 세 방법에 대해 설명해 주세요.
### try-catch 블록을 사용한 예외 처리
예외가 발생할 수 있는 코드를 `try` 블록에 작성하고, 해당 예외를 `catch` 블록에서 처리하는 방식입니다. 예외 발생 시 프로그램이 비정상적으로 종료되지 않고 예외를 처리할 수 있게 합니다.
```java
public class TryCatchExample {
    public static void main(String[] args) {
        try {
            int[] numbers = {1, 2, 3};
            System.out.println(numbers[5]); // ArrayIndexOutOfBoundsException 발생
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("예외 발생: " + e.getMessage());
        }
    }
}
```
### throws 키워드를 사용한 예외 전파
메소드 선언부에 `throws` 키워드를 사용하여 해당 메소드가 특정 예외를 던질 수 있음을 명시합니다. 예외를 던진 메소드를 호출하는 쪽에서 예외를 처리하게 합니다.
```java
import java.io.*;

public class ThrowsExample {
    public static void main(String[] args) {
        try {
            readFile("test.txt");
        } catch (IOException e) {
            System.out.println("파일 읽기 예외 발생: " + e.getMessage());
        }
    }

    public static void readFile(String fileName) throws IOException {
        FileReader file = new FileReader(fileName);
        BufferedReader fileInput = new BufferedReader(file);
        fileInput.close();
    }
}
```

### 사용자 정의 예외를 사용한 예외 처리
기본 제공 예외 클래스 외에 사용자 정의 예외 클래스를 만들어서 특정 상황에 대한 예외를 처리할 수 있습니다. `Exception` 클래스를 상속하여 사용자 정의 예외 클래스를 만듭니다.
```java
class MyException extends Exception {
    public MyException(String message) {
        super(message);
    }
}

public class CustomExceptionExample {
    public static void main(String[] args) {
        try {
            checkValue(0);
        } catch (MyException e) {
            System.out.println("사용자 정의 예외 발생: " + e.getMessage());
        }
    }

    public static void checkValue(int value) throws MyException {
        if (value == 0) {
            throw new MyException("값이 0입니다."); // 사용자 정의 예외 발생
        }
    }
}
```

## CheckedException, UncheckedException 의 차이에 대해 설명해 주세요.

__Checked Exception__ <br/>
컴파일러가 검사하는 예외로 반드시 예외 처리를 해야 합니다. `Exception` 클래스를 상속 받는 대부분의 예외가 CheckedException에 속합니다. I/O Exception, SQLException 등 외부 자원과의 상호작용 시 주로 발생합니다.

__Unchecked Exception__ <br/>
컴파일러가 검사하지 않는 예외로 예외처리를 강제하지 않습니다.
`RuntimeException` 클래스를 상속받는 예외들이 여기에 속합니다. NullPointException, ArrayIndexOutOfBoundsException 등 주로 프로그래밍 오류로 인해 발생하며 실행 시점에서 발생합니다. 

## 예외처리가 성능에 큰 영향을 미치나요? 만약 그렇다면, 어떻게 하면 부하를 줄일 수 있을까요?
예외처리는 예외 객체 생성, 스택 트레이스 정보 수집, 예외 처리기를 찾는 과정 등으로 인해 성능에 영향을 미칠 수 있습니다. 따라서, 필요한 경우에만 예외를 사용하고 불필요한 예외처리를 피하며 복구 가능한 예외에 대해서만 처리해야 부하를 줄일 수 있습니다. 스택 트레이스 말고 다른 커스텀 예외를 만드는 것도 방법이 될 수 있습니다.

---
# Synchronized
Java에서 `synchronized` 키워드는 동기화 메커니즘을 제공하여 여러 스레드가 동시에 접근할 수 있는 공유 자원에 대해 일관성을 유지할 수 있도록 합니다. 즉, `synchronized` 키워드는 Critical Section을 지정하여 하나의 스레드만이 그 구역을 실행할 수 있도록 합니다. 이를 통해 데이터 무결성을 보장하고 Race Condition을 방지할 수 있습니다.

## Synchronized 키워드가 어디에 붙는지에 따라 의미가 약간씩 변화하는데, 각각 어떤 의미를 갖게 되는지 설명해 주세요.

### 메소드
메소드 이름 앞에 synchronized 키워드를 사용하면 해당 메소드 전체를 임계 영역으로 설정할 수 있습니다.
```java
synchronized void increase() {
	count++;
	System.out.println(count);
}
```

### 코드블럭
동기화를 많이 사용하게 되면 효율이 떨어지므로 꼭 필요한 부분에만 블럭을 지정하여 임계 영역으로 설정할 수 있습니다. 
```java
void increase() {
	synchronized(this) {
		count++;
	}
	System.out.println(count);
}
```
위 코드의 경우 this 객체의 lock을 사용하게 됩니다. 

## 효율적인 코드 작성 측면에서, Synchronized는 좋은 키워드일까요?
synchronized 키워드를 사용하면 자바 내부적으로 메서드나 변수에 동기화를 하기 위해 block과 unblock을 처리하게 되는데, 이런 처리는 추가적인 작업이므로 너무 많아질 경우 성능 저하를 일으킬 수 있습니다. 
따라서 필요한 경우에만 synchronized 키워드를 사용하는 것이 중요합니다. 

## Synchronized 를 대체할 수 있는 자바의 다른 동기화 기법에 대해 설명해 주세요.

### java.util.concurrent 패키지의 동시성 클래스
java.util.concurrent.atomic 패키지는 원자적 변수 클래스를 제공합니다. 이 클래스들은 락을 사용하지 않고도 thread-safe한 연산을 제공합니다. 
```java
import java.util.concurrent.atomic.AtomicInteger;

public class AtomicCounter {
    private final AtomicInteger count = new AtomicInteger(0);

    public void increment() {
        count.incrementAndGet();
    }

    public int getCount() {
        return count.get();
    }
}
```

java.util.concurrent 패키지는 thread-safe한 Collection class를 제공합니다. 예를 들어, ConcurrentHashMap은 높은 동시성을 유지하면서도 thread-safe한 hash map을 제공합니다.

## Thread Local에 대해 설명해 주세요.

### Thread Local
java.lang 패키지의 ThreadLocal class 요약
1. Thread에 대한 로컬 변수를 제공한다.
2. 각각의 Thread가 변수에 대해서 독립적으로 접근할 수 있다.
즉, ThreadLocal은 한 Thread에서 실행되는 코드가 동일한 객체를 사용할 수 있도록 지원해줌으로써 여러 스레드가 동시에 실행되더라도 서로의 값을 간섭하지 않고 안전하게 데이터를 저장하고 사용할 수 있습니다.
다만, 스레드가 종료되기 전에 ThreadLocal 값을 제거하지 않으면 메모리 누수가 발생할 수 있고, 스레드 간 데이터를 공유하지 않는 특수한 경우에만 사용해야 복잡성과 버그를 방지할 수 있습니다. 

### 사용 예제
```java
public class ThreadLocalExample {
    // ThreadLocal 인스턴스 생성
    private static final ThreadLocal<Integer> threadLocalValue = new ThreadLocal<Integer>() {
        @Override
        protected Integer initialValue() {
            return 0;
        }
    };

    public static void main(String[] args) {
        Runnable task = () -> {
            for (int i = 0; i < 5; i++) {
                // 현재 스레드의 ThreadLocal 값 가져오기
                int value = threadLocalValue.get();
                // 값 증가시키기
                threadLocalValue.set(value + 1);
                System.out.println(Thread.currentThread().getName() + ": " + threadLocalValue.get());
            }
        };

        Thread thread1 = new Thread(task, "Thread-1");
        Thread thread2 = new Thread(task, "Thread-2");

        thread1.start();
        thread2.start();
    }
}

```

---
# Java Stream
Java 8부터 추가된 기술로 람다를 이용해 배열과 컬렉션을 함수형을 간단하게 처리할 수 있는 기술입니다. 기존의 for문 등의 반복문을 사용하면 코드의 가독성과 재사용성이 떨어지며 데이터 타입마다 다른 방식으로 다뤄야 하는 불편함이 있었습니다. 스트림은 데이터 덩어리를 추상화하여 데이터 타입에 관계없이 모두 같은 방식으로 다룰 수 있으므로 코드의 재사용성과 가독성이 더 좋아졌습니다.

## Stream과 for ~ loop의 성능 차이를 비교해 주세요
작은 데이터 셋에서는 for 루프가 더 빠르고 효율적입니다.큰 데이터셋은 병렬 스트림을 사용하는 것이 더 좋을 수 있습니다. 여러 코어를 사용하여 직렬로 처리하는 for 루프보다 더 빠를 수 있습니다.
__참고__<br/>
[Java Stream API는 왜 for-loop보다 느릴까?](https://sigridjin.medium.com/java-stream-api%EB%8A%94-%EC%99%9C-for-loop%EB%B3%B4%EB%8B%A4-%EB%8A%90%EB%A6%B4%EA%B9%8C-50dec4b9974b)

## Stream은 병렬처리 할 수 있나요?할 수 있습니다. 개발자가 직접 스레드를 생성할 필요 없이 parallelStream()만 사용하면 작업들을 분할 가능한 만큼 쪼개고, 쪼개진 작업들을 thread를 통해 작업 후 결과를 합치는 과정으로 결과를 만들어 냅니다.
__참고__<br/>
[[Java] 병렬 스트림(ParallelStream) 사용 방법 및 주의사항출처: https://dev-coco.tistory.com/183 [슬기로운 개발생활:티스토리]](https://dev-coco.tistory.com/183)

## Stream에서 사용할 수 있는 함수형 인터페이스에 대해 설명해 주세요.
filter 메소드에서 사용하는 Predicate<T> 인터페이스가 있습니다. 주어진 조건을 테스트할 수 있습니다. map 메소드에서 사용하는 Function<T, R> 인터페이스는 입력을 받아 출력을 반환할 수 있습니다. forEach 메소드에서 사용하는 Consumer<T> 인터페이스는 입력을 받아서 소비합니다. 반환값이 없습니다. 스트림 요소를 생성할 때 사용하는 Supplier<T> 인터페이스는 아무 입력 없이 출력을 제공합니다. 이외에도 다양한 함수형 인터페이스를 사용할 수 있습니다.

## 가끔 외부 변수를 사용할 때, final 키워드를 붙여서 사용하는데 왜 그럴까요? 꼭 그래야 할까요?
병렬 스트림을 사용할 때 thread의 공유 자원으로 인해 문제가 생길 수 있기 때문에 사용하는 변수의 값을 변경하지 않고 안정성을 높이기 위해 사용해야 합니다.

---
# 참고
[JVM Internal](https://d2.naver.com/helloworld/1230)<br/>
[JVM 구조와 JAVA의 동작 원리](https://velog.io/@sgwon1996/JAVA%EC%9D%98-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC%EC%99%80-JVM-%EA%B5%AC%EC%A1%B0)<br/>
[자바의 static과 final 키워드 깊게 이해하기](https://f-lab.kr/insight/understanding-java-static-final?gad_source=1&gclid=CjwKCAjwqf20BhBwEiwAt7dtdSxEiIOHs_QJLrqbBFAsmbMR3H13lP3MwJZ9d5txWLWbT999oW_0fxoC60QQAvD_BwE)
