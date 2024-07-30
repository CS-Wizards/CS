# Java의 GC
## Java의 GC에 대해 설명해 주세요.

__개념__<br/>
Garbage Collection(GC)는 프로그래밍 언어에서 더 이상 사용되지 않는 객체를 자동으로 메모리에서 해제하는 메모리 관리 기법입니다.

__목적__<br/>
메모리 누수를 방지하고, 개발자가 수동으로 메모리를 관리하는 부담을 줄이는 것이 GC의 목적입니다.

__GC의 대상 영역__<br/>
JVM의 Runtime Data Area에서 Heap 메모리 영역은 다음 그림과 같이 쪼갭니다.

<img src="https://velog.velcdn.com/images%2Frecordsbeat%2Fpost%2F682408fc-f29e-42e9-b980-3d6f1d6c4989%2Fimage.png" width="50%" />


**Young** : 비교적 최근에 참조가 일어난 곳
- eden : young 영역 중에서도 특히 방금 막 생성된 것들이 위치한다
- survivor : eden에서 생존한 것들이 당분간 위치하는 곳

**Old**: 특정 횟수 이상을 살아남은 참조가 살아있는 곳

**Permanent**: Method Area의 메타 정보가 기록된 곳

__동작 방식__<br/>
1. Stop the World<br/>
   GC를 실행하기 위해 JVM이 애플리케이션의 실행을 멈추는 작업입니다. GC가 실행될 때는 GC를 실행하는 스레드를 제외한 모드 스레드들의 작업이 중단되고, GC가 완료되면 작업이 재개됩니다.

2. Mark and Sweep<br/>
   `Mark` : 사용되는 메모리와 사용되지 않는 메모리를 식별하는 작업    
   `Sweep` : Mark 단계에서 사용되지 않음으로 식별된 메모리를 해제하는 작업

Stop the World를 하고 GC는 스택의 변수 또는 객체를 스캔하면서 각각이 어떤 객체를 참조하고 있는지 탐색합니다. 사용되고 있는 메모리를 식별(Mark)하고, Mark되지 않은 객체들을 메모리에서 제거(Sweep)합니다.

__GC 종류__
1. Minor GC<br/>
   Young 영역에 대한 GC, Eden 영역이 다 차면 발생합니다. 살아남은 객체는 Survivor 영역으로 옮겨집니다.    
   한 개의 Survivor 영역이 다 차면 다른 Survivor 영역으로 옮겨 1개의 Survivor 영역은 반드시 빈 상태가 되게 만듭니다.
2. Major GC<br/>
   Old 영역에 대한 GC, Young 영역에서 오래 살아남은 객체가 Old 영역으로 이동하여 Old 영역의 메모리가 부족해지면 Major GC가 발생합니다.

__GC 알고리즘__
1. Serial GC<br/>
   싱글스레드 애플리케이션을 위한 GC, Young 영역에서는 Mark Sweep 알고리즘을 수행한다. Old 영역에서는 Mark Sweep 알고리즘에 Compact 작업을 추가합니다. Compact란 Heap 영역을 정리하기 위한 단계로 유효한 객체를 한 곳으로 몰고 객체가 존재하지 않는 부분으로 만드는 방식입니다.

2. Parallel GC<br/>
   멀티스레드 애플리케이션을 위한 GC, 여러 개의 스레드를 통해 Parallel 하게 GC를 수행함으로써 GC의 오버헤드를 상당히 줄여줍니다. 그러나, 애플리케이션을 멈추는 것을 피하진 못합니다.

3. CMS(Concurrent Mark Sweep) GC<br/>
   Parallel GC와 마찬가지로 여러 개의 스래드를 이용합니다. 다른 GC와는 다르게 Mark Sweep 알고리즘을 Concurrent하게 수행합니다. 애플리케이션의 지연 시간을 최소화하여 애플리케이션이 구종 중일 때 멈추지 않고 GC를 이용 가능합니다. 그러나 다른 GC 방식보다 하드웨어 자원을 더 많이 필요로 하며 compact 단계를 수행하지 않아 메모리 파편화가 심해질 수도 있습니다. 만약 이 때문에 메모리 공간이 부족해지면 Serial GC와 똑같이 동작합니다.

4. G1(Garbage First) GC<br/>
   장기적으로 문제될 수 있는 CMS GC를 대체하기 위해 개발되었고, Java 7부터 지원합니다.
   기존 GC 알고리즘 알고리즘은 Heap 영역을 Young과 Old 영역으로 나누었지만 G1 GC는 물리적으로 메모리 공간을 나누지 않습니다. 대신 Region 이라는 개념을 도입하여 Heap을 균등하게 여러 지역으로 나누고, 각 지역을 역할과 함께 논리적으로 구분(Eden, Survivor, Old)하여 객체를 할당합니다. 더해서 Humonogous와 Availab/e/Unused 라는 역할을 추가하여 Region 크기의 50%를 초과하면 Humonogous, 사용되지 않는 Region을 Available/Unused 로 지정합니다. 이후 Gargabe가 많은 Region에 대해 우선적으로 GC를 수행합니다.

한 지역에서 Minor GC가 실행되면 Garbage가 가장 많은 지역을 찾아 Mark And Sweep을 합니다. Eden 지역에서 GC가 수행되면 살아남은 객체를 Available/Unused 지역을 옮기고 Survivor 로 변경합니다. Eden 영역은 Available/Unused 으로 변경됩니다.

시스템이 계속 운영되다가 객체가 너무 많아 메모리를 빠르게 회수할 수 없을 때 Major GC가 수행됩니다. G1 GC는 다른 알고리즘과 다르게 Garbage가 많은 영역에서만 GC를 Concurrent하게 수행되기 때문에 애플리케이션의 지연을 최소화 시킵니다.

이러한 구조는 다른 방식보다 처리 속도가 빠르고 효율적이기 때문에 Java9부터 기본 GC로 사용되게 되었습니다.


__장점__<br/>
1. 메모리 누수 방지
2. 코드 간결성
   메모리 코드를 작성하지 않아도 되므로, 코드가 간결해집니다.

__단점__<br/>
1. 시스템 자원을 사용하기 때문에 성능 오버헤드가 발생할 수 있습니다.
2. GC가 언제 실행될지 예측하기 어려울 수 있습니다.

### finalize() 를 수동으로 호출하는 것은 왜 문제가 될 수 있을까요?
Object 클래스의 finalize() 메서드는 호출되더라도 즉시 실행된다는 보장이 없으며, 언제 수행되는지도 알 수 없습니다.
즉, 예측 불가능하기 때문에 애플리케이션 실행 중에 리소스 문제가 발생할 수 있으며 디버깅하기도 쉽지 않습니다.

finalize() 메서드 안에서 예외가 발생한다 해도 무시가 되며 finalize() 메서드도 종료됩니다.

더해서, finalize()를 재정의하는 것만으로도 성능 저하가 발생합니다.

이와 같은 이유로 인해 finalize()를 수동으로 호출하는 것은 무제가 될 수 있습니다.

### 어떤 변수의 값이 null이 되었다면, 이 값은 GC가 될 가능성이 있을까요?
변수의 참조를 null로 변경한다면, 원래 참조하고 있던 메모리는 참조가 끊어지기 때문에 GC의 대상이 될 수 있습니다.


### 참고
[[Java] Garbage Collection(가비지 컬렉션)의 개념 및 동작 원리 (1/2)
출처: https://mangkyu.tistory.com/118 [MangKyu's Diary:티스토리]](https://mangkyu.tistory.com/118

[[Java] 다양한 종류의 Garbage Collection(가비지 컬렉션) 알고리즘 (2/2)
출처: https://mangkyu.tistory.com/119 [MangKyu's Diary:티스토리]](https://mangkyu.tistory.com/119)

[Garbage Collector 제대로 알기](https://velog.io/@recordsbeat/Garbage-Collector-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EA%B8%B0)



# equals()와 hashcode()에 대해 설명해 주세요.

자바의 모든 클래스가 상속 받는 Object 클래스에 속한 메서드입니다.
equals() 메서드는 두 객체의 동등성을 비교하는 데 사용됩니다. hashCode 메서드는 객체의 해시코드 값을 반환하는 데 사용됩니다.

equals() 메서드를 재정의하지 않으면 객체의 동등성 비교는 객체의 참조를 비교하는 것으로 제한됩니다. 따라서 값으로 두 객체를 논리적으로 같게 간주하고자 한다면 equals() 메서드를 오버라이딩 해야 합니다. hashCode() 메서드는 해시 기반의 컬렉션에 저장할 때 사용하는 해시 코드를 제공합니다. equals()의 결과가 true인 두 객체의 해시코드는 반드시 같아야 한다는 자바의 규칙 때문에 hashCode() 도 같이 재정의해줘야 합니다. 두 메서드를 함께 재정의하지 않을 경우, 해시 기반 컬렉션에서 객체가 올바르게 작동하지 않을 수도 있습니다.

즉, 항상 오버라이딩 할 필요는 없지만, 객체의 논리적 동등성을 비교하거나 해시 기반 컬렉션에서 사용될 경우 반드시 둘 다 오버라이딩 해야 합니다.

<br/>

__equals() 와 hashCode() 오버라이딩__

```java
public class Person {
    private String id;
    private String name;

    public Person(String id, String name) {
        this.id = id;
        this.name = name;
    }

    // id와 name 필드가 같으면 두 객체가 동등하다고 간주한다. 
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return id.equals(person.id) && name.equals(person.name);
    }

    // id와 name을 기반으로 hashCode를 생성한다. 
    @Override
    public int hashCode() {
        return Objects.hash(id, name);
    }

}
```

<br/>

> __HashMap의 동작 원리__:
>1. 해시 코드 생성(hashCode() 메소드 호출)
>2. 해시 코드에 따른 버킷(배열의 인덱스와 같은 느낌) 결정
>3. 버킷에 객체 저장
>4. 객체 검색 : 검색할 객체의 해시 코드 생성, 해시 코드에 해당하는 버킷을 찾아, 그 버킷 내에서 equals() 메소드를 사용하여 실제 객체를 찾는다.

![image](https://github.com/chunghye98/KB_CS_Study/assets/57451700/7ef65dc6-ba8c-4bd9-9b91-ad5db7f32212)

<br/>

> __동일성__: 두 객체가 동일한 메모리 주소를 가리키는지를 의미한다. 즉, 두 객체가 같은 인스턴스인지 확인한다. 자바에서는 `==` 연산자를 사용하여 동일성을 비교한다.    
> __동등성__: 두 객체가 논리적으로 같은지를 의미한다. 즉, 객체의 상태나 값이 같은지를 확인한다. 자바에서는 `equals()` 메소드를 오버라이딩하여 객체의 동등성을 정의한다.


### 본인이 hashcode() 를 정의해야 한다면, 어떤 점을 염두에 두고 구현할 것 같으세요?

먼저 equals() 메서드를 재정의할 것 같습니다. equals()와 hashcode() 메서드를 둘 다 오버라이딩하여 사용해야 해시 자료구조에서 문제없이 사용할 수 있기 때문입니다.

### 그렇다면 equals() 를 재정의 해야 할 때, 어떤 점을 염두에 두어야 하는지 설명해 주세요.
객체의 값들이 논리적으로 동등한지를 알고 싶어서 재정의하는 것임을 생각해야 합니다. 어떤 값을 기준으로 동등하다고 간주할지 결정해야 합니다. 예를 들어, Person이라는 
클래스에 socialNum 이라는 멤버 변수가 존재한다면, 이 socialNum 값만 같아도 논리적으로 같은 객체라고 판단하도록 오버라이딩할 수 있습니다.

---

### 참고
[자바의 Object 클래스와 equals, hashCode 메서드의 중요성](https://f-lab.kr/insight/java-object-class-equals-hashcode?gad_source=1&gclid=Cj0KCQjw4MSzBhC8ARIsAPFOuyVMJOSlO4-yNxA2CcqBvh6i7yymsQ-Lh1N7ikkeYi4kSIo_UPPCb0YaAkxREALw_wcB)

# IoC와 DI에 대해 설명해 주세요.

제어의 역전(IoC, Inversion of Control)과 의존성 주입(DI, Dependency Injection)은 스프링의 핵심 개념입니다. 이 기법을 통해 스프링은 코드 간의 결합도를 낮추고 유연성을 높일 수 있습니다.

## IoC(Inversion of Control)
제어의 역전은 프로그램의 흐름을 개발자가 아닌 프레임워크가 관리하는 것을 의미합니다. 이를 제어의 역전이라 합니다. 객체의 생성과 생명주기 관리를 개발자가 아닌 프레임워크가 담당하게 함으로써 개발자는 비즈니스 로직에 더 집중할 수 있게 됩니다. 스프링에서는 Application Context에서 Bean들의 객체, 생성, 설정, 관리를 담당합니다. 
예를 들어, 프로그램에 대한 제어 흐름에 대한 권한은 모두 Config 클래스가 가지고 있습니다. Config 클래스에서 구현 객체를 생성하고 이 구현객체들은 자신의 로직을 실행하기만 합니다. 즉, 프로그램의 제어 흐름을 개발자가 직접 제어하는 것이 아니라 외부에서 관리하는 것을 제어의 역전이라 합니다.

> Framework : 내가 작성한 코드를 제어하고, 대신 실행하는 것을 프레임워크라 한다. ex)Junit    
> Library : 내가 작성한 코드가 직접 제어와 흐름을 담당한다면 라이브러리라 한다. ex)java.lang



## DI(Dependency Injection)
의존성 주입은 Runtime 시 객체 간의 의존성을 외부에서 주입하는 방식을 의미합니다. 즉, 애플리케이션 실행 시점에 외부에서 실제 구현 객체를 생성하고 클라이언트에 전달해서 클라이언트와 서버의 실제 의존관계가 연결되는 것을 의존성 주입이라 합니다. 
DI를 통해 클라이언트의 코드를 변경하지 않고 클라이언트가 호출하는 대상의 타입 인스턴스를 변경할 수 있습니다. 또한, 정적인 클래스 의존관계를 변경하지 않고(내부 코드를 변경하지 않고 주입할 구현체만 변경하면 됨) 동적인 객체 인스턴스 의존관계를 쉽게 변경할 수 있습니다.

DI를 통해 객체는 필요한 의존성만을 제공 받으므로 코드 변경에 더 유연하게 대응할 수 있고, 단위 테스트가 용이해집니다. 필드, setter, 생성자 주입을 사용할 수 있습니다.

## 정리
제어권이 프로그램에 넘어간 것을 IoC라 하고 의존성 주입에 초점을 맞춘 개념이 DI 이다. 

## 후보 없이 특정 기능을 하는 클래스가 딱 한 개라면, 구체 클래스를 그냥 사용해도 되지 않나요? 그럼에도 불구하고 왜 Spring에선 Bean을 사용 할까요?

언제나 프로그램은 수정이 될 수 있기 때문에 하나의 클래스가 구체 클래스를 사용하도록 만들면 나중에 유지보수하기 힘들어집니다. 따라서 처음 실행할 때 프로그램에 맞는 클래스를 사용할 수 있도록 Bean을 사용하는 것이 좋습니다.


## Spring의 Bean 생성 주기에 대해 설명해 주세요.
데이터베이스 커넥션 풀이나, 네트워크 소켓처럼 애플리케이션 시작 시점에 필요한 연결을 미리 해두고, 애플리케이션 종료 시점에 연결을 모두 종료하는 작업을 진행하려면, 객체의 초기화와 종료 작업이 필요합니다.

> ex. Spring Boot에서 기본 설정인 HikariCP(db connection pool)를 자동으로 빈으로 등록합니다. 

### 스프링 빈의 이벤트 라이프 사이클
`스프링 컨테이너 생성 -> 스프링 빈 생성 -> 의존 관계 주입 -> 초기화 콜백 -> 사용 ->
소멸 전 콜백 -> 스프링 종료` 

- 초기화 콜백: 자원 할당, 설정 확인 및 초기화 등
- 소멸 전 콜백: 자원 해제, 상태 저장, 정리 작업(입시 파일 삭제, 캐시 정리 등등)

> 생성자 주입은 스프링 빈 생성 단계에서 일어나긴 한다. 반면, 필드 주입이나 setter 주입은 의존 관계 주입 단계에서 일어난다.

> 생성자 안에서 무거운 초기화 작업을 하는 것보다 생성 부분과 초기화 부분을 명확하게 나누는 편이 유지보수하기 좋다. 간단한 초기화 작업이라면 생성자에서 한번에 처리하는 것이 더 나을 수도 있다. 

### 사용 방법
1. `InitializingBean`, `DisposableBean` 인터페이스 implements 해서 afterPropertiesSet(), destroy() 등 메서드 오버라이드하여 사용 
   스프링 전용 인터페이스이기 때문에 너무 스프링에 의존한다는 단점을 가진다. 현재는 잘 사용하지 않는 방법이다. 
2. Bean 등록 시 initMethod, destroyMethod 속성을 지정하여 사용
   스프링 빈이 스프링 코드에 의존하지 않으며 코드가 아니라 설정 정보를 사용하기 때문에 코드를 고칠 수 없는 외부 라이브러리에도 초기화, 종료 메서드를 적용할 수 있다.
3. `@PostConstruct`, `@PreDestroy` 어노테이션을 메서드에 사용하면 바로 빈 생성주기에 등록된다. Spring에서 이 방법을 권장한다.
   자바 표준에 등록된 인터페이스이므로 다른 컨테이너에서도 동작한다. 다만, 외부 라이브러리에는 적용하지 못한다.  

## 프로토타입 빈은 무엇인가요?

### 빈 스코프
스프링은 기본적으로 빈을 싱글톤 스코프로 생성합니다. 스코프란 빈이 존재할 수 있는 범위를 의미합니다. 싱글톤 스코프는 스프링 컨테이너의 시작과 종료까지 유지되는 가장 넓은 범위의 스코프입니다. 스프링 컨테이너는 `프로토타입` 스코프를 통해 빈의 생성과 의존관계 주입까지만 관여하고 더는 관리하지 않는 짧은 범위의 스코프도 지원합니다.

```java
import java.beans.BeanProperty;

// 컴포넌트 스캔 자동 등록
@Scope("prototype")
@Component
public class HelloBean {
}

//수동 등록
@Scope("prototype")
@Bean
PrototypeBean HelloBean() {
	return new HelloBean();
}
```

프로토타입 스코프를 가진 빈은 스프링 컨테이너에서 요청할 때마다 새로운 인스턴스가 생성됩니다. 각 인스턴스는 독립적이므로 내부 상태가 서로 영향을 주지 않습니다. 또한, 프로토타입 빈의 소멸 작업은 스프링 컨테이너가 관리하지 않으므로 개발자가 직접 처리해야 합니다.

여러 명이 싱글톤으로 관리되는 객체를 사용한다면 예상치 못하게 값이 변경되는 문제가 발생할 수 있기 때문에 이러한 문제가 예상되는 객체는 프로토타입 빈으로 바꿔 사용할 수 있습니다. 예를 들어, 멀티스레드 환경, 사용자 세션마다 별도의 데이터가 필요한 경우에 프로토타입 빈을 사용할 수 있습니다. 

### 참고
[스프링 프레임워크의 핵심 개념: IOC와 DI 이해하기](https://f-lab.kr/insight/understanding-spring-ioc-di?gad_source=1&gclid=Cj0KCQjw4MSzBhC8ARIsAPFOuyU9paG6kzdnOaZhuMV8J1DmuS_05a04bYjlNrPUiATA0v1_lSZf-S4aAmApEALw_wcB)<br/>
[Spring에서 프로토타입 스코프는 어떤 경우에 사용하나요?
](https://okky.kr/questions/990588)<br/>
[스프링 핵심 원리 - 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard)


# DispatcherServlet 의 역할에 대해 설명해 주세요.

__개념 및 역할__<br/>
DispatcherServlet은 웹 애플리케이션의 요청을 받아 처리하는 프론트 컨트롤러 역할을 합니다. DispatcherServlet은 요청을 받아서 적절한 핸들러로 배분하고, 핸들러가 처리한 결과를 적절한 뷰로 렌더링하여 응답을 생성합니다.

__처리 과정__<br/>
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbcff5H%2FbtstbdRuSr9%2FpNKnGdMwftSWmiGLHA7yL0%2Fimg.png)

- 클라이언트의 요청을 디스패처 서블릿이 받음
- 요청 url을 기반으로 적절한 핸들러를 찾기 위해 HadlerMapping 조회
- (인터셉터의 preHandle 호출)
- 찾은 핸들러를 실행하기 위해 HandlerAdapter를 호출하여 핸들러(컨트롤러) 메서드 실행 후
   - (뷰를 직접 반환하는 경우) ModelAndView 객체 반환
   - (Rest API인 경우) ResponseEntity나 도메인 객체 반환(Rest API인 경우)
- (인터셉터의 postHandle 호출)
- 응답 변환
   - (뷰를 직접 반환하는 경우) ModelAndView 객체에서 뷰 이름을 추출하고 ViewResolver를 사용하여 실제 뷰 객체를 찾음
   - (RestAPI인 경우) 핸들러 메서드가 반환한 객체(ResponseEntity)를 HTTP 응답 본문으로 변환하기 위해 HttpMessageConverter를 사용
- 응답 반환
   - (뷰를 직접 반환하는 경우) 뷰 객체를 사용하여 모델 데이터를 바탕으로 응답 생성, 클라이언트에 반환
   - (RestAPI인 경우) 변환된 응답 데이터는 HTTP 응답 본문에 작성되어 클라이언트에게 반환
- (인터셉터의 afterCompletion 호출)

### 여러 요청이 들어온다고 가정할 때, DispatcherServlet은 한번에 여러 요청을 모두 받을 수 있나요?

가능합니다. 서블릿 컨테이너는 각 HTTP 요청을 처리하기 위해 새로운 스레드를 생성하거나, 스레드 풀에서 사용 가능한 스레드를 할당합니다. 각 요청을 독립된 스레드에서 DispatcherServlet을 통해 처리됩니다.
dispatcherServlet은 단일 인스턴스로 생성되며, 할당된 스레드는 DispatcherServlet 인스턴스의 service 메서드를 호출하여 요청을 처리합니다. 각 요청에 대한 상태는 ThreadLocal 또는 메서드 로컬 변수를 통해 관리되며, 다른 요청과 격리됩니다.

### 수많은 @Controller 를 DispatcherServlet은 어떻게 구분 할까요?
HandlerMapping을 조회하여 @RequestMapping 또는 특정 HTTP 메서드 매핑 어노테이션이 붙은 메서드 즉, 적절한 핸들러(컨트롤러)를 찾습니다.

이후 HandlerExecutionChain 객체로 반환합니다. 이 객체에는 선택된 핸들러와 함께 실행할 인터셉터 목록을 포함합니다.

이후 인터셉터의 preHandle을 호출하고 HandlerAdapter에서 실제 핸들러를 호출합니다.



# JPA와 같은 ORM을 사용하는 이유가 무엇인가요?

## ORM(Object-Relational Mapping)
ORM은 데이터베이스와 객체 지향 프로그래밍 언어 간의 불일치를 해결하기 위해 사용하는 기술입니다. ORM을 사용하면 다음과 같은 장점이 있습니다.
1. 생산성 향상<br/>
   SQL 쿼리를 직접 사용할 필요 없이 객체지향 코드로 데이터베이스 작업을 수행할 수 있기 때문에 개발자의 생산성이 향상됩니다.
2. 유지보수성 향상<br/>
   데이터베이스 테이블 구조가 변경되더라도 ORM 매핑을 통해 쉽게 대응할 수 있습니다.


### 영속성은 어떤 기능을 하나요? 이게 진짜 성능 향상에 큰 도움이 되나요?

__영속성__<br/>
영속성이란 데이터를 생성한 프로그램이 종료되어도 사라지지 않는 데이터의 특성을 의미합니다. 영속성을 갖지 않으면 데이터는 메모리에서만 존재하게 되고 프로그램이 종료되면 해당 데이터는 모두 사라지게 됩니다. 따라서 데이터를 파일이나 DB에 영구 저장함으로써 데이터에 영속성을 부여해야 합니다.

__영속성 컨텍스트__<br/>
JPA에서 엔티티를 영구 저장하고 관리하는 작업을 수행하는 공간입니다. 엔티티 매니저가 영속성 컨텍스트를 생성하여 사용합니다. 애플리케이션과 DB 사이에서 동작하는 하나의 추상 계층이라고 볼 수 있습니다.
영속성 컨텍스는 기본적으로 트랜잭션 범위에서 동작합니다. 같은 트랜잭션 내에서는 동일한 엔티티 매니저가 영속성 컨텍스트를 공유하고, 엔티티의 변경을 관리합니다.

![](https://incheol-jung.gitbook.io/~gitbook/image?url=https%3A%2F%2F2649832514-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-M5HOStxvx-Jr0fqZhyW%252F-MHb8Kq2OMRiS2HyXT_K%252F-MHbBcxqMRck0E9YERLA%252FJPA_3_7.png%3Falt%3Dmedia%26token%3Df8c834cf-8ced-4e70-82d2-5ecfec518af6&width=768&dpr=1&quality=100&sign=b551b47e&sv=1)

__영속성 컨텍스트의 장점__<br/>
1. 1차 캐시<br/>
   영속성 컨텍스트는 1차 캐시로 작동합니다. 즉, 영속성 컨텍스트 내에서 조회된 엔티티는 메모리에 저장되어 동일한 트랜잭션 내에서 반복 조회 시 데이터베이스에 다시 접근하지 않고 캐시된 엔티티를 반환합니다. 이는 데이터베이스 접근 횟수를 줄여 성능을 향상시킵니다.
2. 지연 로딩<br/>
   관계형 데이터를 필요할 때까지 로딩하지 않고 지연 로딩을 통해 성능을 최적화할 수 있습니다.
3. 변경 감지(더티 체킹)<br/>
   영속성 컨텍스트는 엔티티 객체의 상태 변화를 자동으로 감지하여 트랜잭션 종료 시점에 DB에 반영(commit)합니다.    
   이때, 엔티티가 처음 영속성 컨텍스트에 추가될 때 엔티티의 현재 상태(필드 값)를 스냅샷으로 저장합니다. 플러시 시점에 스냅샷과 현재 엔티티를 비교해서 변경된 부분을 찾고, 변경된 데이터가 있다면 update 쿼리문을 쓰기 지연 SQL 저장소에 저장해 두었다가 트랜잭션 커밋 시 DB에 반영합니다.
> flush: 엔티티의 변경 사항을 DB에 반영하지만, 트랜잭션이 커밋되지 않았기 때문에 다른 트랜잭션에서는 보이지 않습니다.

__단점__<br/>
1. 지연로딩을 적절히 관리하지 않으면 N+1 쿼리 문제 등 성능 이슈가 발생할 수 있습니다.
2. 복잡한 쿼리나 대규모 배치 작업은 JPQL보다는 native SQL 쿼리가 더 효율적일 수 있습니다.

### N + 1 문제에 대해 설명해 주세요.
JPA의 지연로딩으로 인해 발생할 수 있는 문제입니다. 연관 관계가 설정된 엔티티를 조회할 경우에 조회한 데이터 갯수(n)만큼 연관관계의 조회 쿼리가 발생하는 문제를 말합니다.

이는 관계형 데이터베이스와 객체지향 언어 간의 패러다임 차이로 인해 발생합니다. 객체는 연관관계를 통해 레퍼런스를 가지고 있으면 언제든지 메모리 내에서 접근 가능하지만 DB의 경우 Select 쿼리를 통해서만 조회할 수 있기 때문입니다.

이를 해결하기 위해 즉시 로딩, batch fetch 등의 기법을 사용하여 쿼리 수를 줄이고 성능을 최적화할 수 있습니다.

### 참고
[JPA의 이해와 실제 사용 시 고려사항](https://f-lab.kr/insight/understanding-jpa)    
[영속성 컨텍스트에 대해 질문이 있습니다.](https://www.google.com/search?q=%EC%98%81%EC%86%8D%EC%84%B1+%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8&oq=%EC%98%81%EC%86%8D%EC%84%B1+%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8&gs_lcrp=EgZjaHJvbWUyBggAEEUYOTIGCAEQRRg80gEIMzI0MmowajeoAgCwAgA&sourceid=chrome&ie=UTF-8#ip=1)    
[영속성 컨텍스트(Persistence Context)](https://incheol-jung.gitbook.io/docs/q-and-a/spring/persistence-context)


# @Transactional

## 개념
`@Transactional` 어노테이션은 Spring 프레임워크에서 트랜잭션 관리를 위해 사용하는 어노테이션입니다. 이 어노테이션을 사용하면 메서드나 클래스에 트랜잭션 경계를 설정할 수 있습니다. 이를 통해 데이터베이스 작업을 원자적으로 처리할 수 있으며, 작업 도중 예외가 발생하면 트랜잭션을 롤백하여 데이터 일관성을 유지할 수 있습니다.

## 주요 기능
1. 트랜잭션 경계 설정<br/>
   `@Transactional` 어노테이션을 메서드나 클래스에 붙이면 해당 범위 내에서 트랜잭션이 관리됩니다. 메서드 단위로 적용하면 해당 메서드 내에서 트랜잭션이 관리되며, 클래스 단위로 적용하면 클래스 내 모든 메서드에 적용됩니다.
2. 트랜잭션 전파(Propagation)<br/>
   트랜잭션 전파 방식은 `propagation` 속성으로 설정할 수 있습니다. 트랜잭션이 어떻게 전파될지를 정의합니다.
    ```java
    @Transactional(propagation = Propagation.REQUIRED)  // 기본값
    public void someMethod() {
    // 트랜잭션이 이미 존재하면 그 트랜잭션을 사용하고, 없으면 새로운 트랜잭션을 생성
    }
    
    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void anotherMethod() {
    // 항상 새로운 트랜잭션을 생성
    }
    ```
3. 격리 수준<br/>
   트랜잭션의 격리 수준을 설정할 수 있습니다. 격리 수준은 여러 트랜잭션이 동시에 실행될 때 서로 간섭하지 않도록 하는 방법을 정의합니다.
   ```java
    @Transactional(isolation = Isolation.READ_COMMITTED)  // 기본값
    public void readCommittedMethod() {
    // 다른 트랜잭션이 커밋한 데이터만 읽을 수 있음
    }
    
    @Transactional(isolation = Isolation.SERIALIZABLE)
    public void serializableMethod() {
    // 가장 높은 격리 수준으로, 완벽한 일관성 보장
    }
    
    ```
4. 트랜잭션 타임아웃<br/>
   트랜잭션이 지정된 시간 내에 완료되지 않으면 자동으로 롤백됩니다.
   ```java
    @Transactional(timeout = 5)  // 5초
    public void timeoutMethod() {
    // 트랜잭션이 5초 내에 완료되지 않으면 롤백
    }
    ```
5. 읽기 전용(read-only)<br/>
   읽기 전용 트랜잭션으로 설정할 수 있습니다. 데이터 변경이 없을 때 성능 최적화에 도움을 줍니다.
    ```java
    @Transactional(readOnly = true)
    public void readOnlyMethod() {
    // 읽기 전용 트랜잭션
    }
    
    ```
이와 같은 작업을 통해 데이터베이스 작업의 일관성과 안정성을 보장할 수 있습니다. 
### @Transactional(readonly=true) 는 어떤 기능인가요? 이게 도움이 되나요?
`@Transactional(readonly=true)`는 조회만 트랜잭션을 읽기 전용으로 설정하는 기능을 제공합니다. 이 설정은 성능 최적화와 불필요한 변경을 방지하는 이점을 제공합니다.
성능 최적화가 가능한 것은 JPA의 영속성 컨텍스트가 수행하는 변경 감지(Dirty Checking)과 관련이 있습니다. 
영속성 컨텍스트는 Entity 조회 시 초기 상태에 대한 snapshot을 저장합니다. 트랜잭션이 commit 될 때, 초기 상태의 정보를 가지는 snapshot과 entity의 상태를 비교하여 변경된 내용에 대해 update query를 생성해 쓰기 지연 저장소에 저장합니다. 
이후, 쓰기 지연 저장소에 모인 SQL query를 일괄적으로 내보내고(flush) 데이터베이스의 트랜잭션을 commit 함으로써 직접 update query를 보내지 않고도 자동으로 Entity의 수정이 이루어지는 Dirty Chekcing이 이루어집니다.
이때, @Transactional(readOnly=true) 속성을 설정한다면 스프링은 JPA의 세션 플러시 모드를 `MANUAL`로 설정합니다. MANUAL 모드는 트랜잭션 내에서 사용자가 수동으로 flush()를 호출하지 않으면 flush()가 자동으로 수행되지 않습니다. 이 모드로 인해 트랜잭션 Commit 시 영속성 컨텍스트가 자동으로 flush() 되지 않으므로 조회용으로 가져온 Entity의 예상치 목한 수정을 방지할 수 있습니다.
더해서, readOnly=true를 설정하게 되면 JPA는 해당 트랜잭션 내에서 조회하는 Entity는 조회용임을 인식하고 변경 감지를 위한 Snapshot을 따로 보관하지 않으므로 메모리가 절약되는 성능상 이점도 가집니다. 


### 그런데, 읽기에 트랜잭션을 걸 필요가 있나요? @Transactional을 안 붙이면 되는거 아닐까요?
