# Java와 Spring 관련 주요 개념 정리

## 1. `finalize()` 메서드를 수동으로 호출하는 것은 왜 문제가 될까요?
`finalize()` 메서드는 객체가 더 이상 참조되지 않아 가비지 컬렉션이 수행될 때 호출됩니다. 이를 수동으로 호출하면 객체가 여전히 유효한 상태임에도 불구하고 자원 해제 등의 작업이 수행되어 객체 상태가 불일치하게 될 수 있습니다.

## 2. 어떤 변수의 값이 `null`이 되었다면, 이 값은 GC가 될 가능성이 있을까요?
이 객체에 대한 다른 활성 참조가 없어야 합니다. 다른 스코프, 스태틱 필드, 캐시 등에서 여전히 참조 중이라면 해당 객체는 가비지 컬렉션의 대상이 되지 않습니다.

## 3. 본인이 `hashCode()`를 정의해야 한다면, 어떤 점을 염두에 두고 구현할 것 같으세요?
`hashCode()` 메서드는 객체의 상태가 변하지 않는 한 동일한 객체에 대해 여러 번 호출되어도 항상 동일한 값을 반환해야 합니다.

## 4. `equals()`를 재정의 해야 할 때, 어떤 점을 염두에 두어야 하는지 설명해 주세요.
`equals()` 메서드가 두 객체를 같다고 판단하면, 두 객체의 `hashCode()` 값도 동일해야 합니다. 반대로 `hashCode()` 값이 다르면, `equals()` 메서드가 두 객체를 다르다고 판단할 수 있습니다.

## 5. 후보 없이 특정 기능을 하는 클래스가 딱 한 개라면, 구체 클래스를 그냥 사용해도 되지 않나요? 그럼에도 불구하고 왜 Spring에선 Bean을 사용 할까요?
현재 특정 기능을 하는 클래스가 하나일지라도, 미래에 해당 기능을 확장하거나 대체할 필요가 있을 수 있습니다. Spring의 Bean을 사용하면 코드 수정 없이 설정만 변경하여 새로운 구현체를 주입할 수 있습니다.

## 6. Spring의 Bean 생성 주기에 대해 설명해 주세요.
1. 스프링 IoC 컨테이너 생성
2. 스프링 빈 생성
3. 의존관계 주입
4. 초기화 콜백 메소드 호출
5. 사용
6. 소멸 전 콜백 메소드 호출
7. 스프링 종료

## 7. 프로토타입 빈은 무엇인가요?
프로토타입 빈은 Spring 컨테이너에 의해 요청될 때마다 새로운 인스턴스를 반환하는 빈 스코프입니다. 이를 통해 각 요청마다 새로운 객체를 생성할 수 있습니다.

### 용어 정리
- **IoC**: 제어의 역전 또는 제어의 반전으로, 메소드나 객체의 호출작업이 개발자에 의해 결정되는 것이 아닌 외부(스프링)에서 결정되는 것을 의미합니다.
- **DI**: Dependency Injection의 약자로 IoC의 핵심 요소이며, 제어의 반전이 일어날 때 스프링이 내부에 있는 객체들 간의 관계를 관리할 때 사용하는 기법입니다.
- **Bean**: 스프링 컨테이너에 의해 관리되는 재사용 가능한 소프트웨어 컴포넌트입니다. 스프링 컨테이너가 관리하는 자바 객체를 의미하며, 하나 이상의 빈을 관리합니다. 빈은 인스턴스화된 객체를 의미하며, 스프링 컨테이너에 등록된 객체를 스프링 빈이라고 합니다. 쉽게 이해하자면 new 키워드 대신 사용한다고 보면 됩니다.

## 8. 여러 요청이 들어온다고 가정할 때, DispatcherServlet은 한번에 여러 요청을 모두 받을 수 있나요?
- **멀티스레딩**: DispatcherServlet은 멀티스레드 환경에서 동작합니다. 즉, 여러 요청이 들어오면 각 요청은 별도의 스레드에서 처리됩니다. Spring MVC는 서블릿 컨테이너(Tomcat, Jetty 등)가 제공하는 서블릿 스레드 풀을 활용하여 동시 요청을 처리합니다.
- **스레드 풀**: 서블릿 컨테이너는 스레드 풀을 관리하며, 들어오는 요청마다 새로운 스레드를 생성하거나 풀에서 기존 스레드를 재사용하여 요청을 처리합니다.

## 9. 수많은 `@Controller`를 DispatcherServlet은 어떻게 구분할까요?
1. **클라이언트 요청 수신**: 클라이언트가 웹 애플리케이션에 HTTP 요청을 보냅니다.
2. **요청 매핑**: DispatcherServlet은 요청 URL을 기반으로 핸들러 매핑을 조회하여 적절한 컨트롤러 메서드를 찾습니다.
3. **핸들러 실행**: 찾은 컨트롤러 메서드를 호출하여 요청을 처리합니다.
4. **뷰 해석 및 렌더링**: 컨트롤러 메서드가 반환한 모델과 뷰 이름을 기반으로 ViewResolver를 사용해 적절한 뷰를 찾고, 이를 렌더링합니다.
5. **응답 반환**: 렌더링된 뷰를 클라이언트에게 응답으로 반환합니다.

결론적으로 DispatcherServlet은 다양한 핸들러 매핑과 핸들러 어댑터를 사용하여 `@Controller`와 그 안의 `@RequestMapping` 메서드를 적절하게 구분하고 호출합니다.

## 10. 영속성은 어떤 기능을 하나요? 이게 진짜 성능 향상에 큰 도움이 되나요?
영속성 컨텍스트(Persistence Context)는 1차 캐시 역할을 합니다. 동일한 트랜잭션 내에서 동일한 엔티티를 조회하면 데이터베이스에 다시 접근하지 않고 1차 캐시에서 반환합니다.

## 11. N + 1 문제에 대해 설명해 주세요.
N + 1 문제는 한 번의 조회로 여러 개의 연관된 엔티티를 가져와야 할 때, 연관된 엔티티들을 개별적으로 조회하여 발생하는 성능 문제를 말합니다. 주로 지연 로딩이 기본 설정인 경우 발생하며, 메인 엔티티를 조회한 후 연관된 엔티티를 각각 별도의 쿼리로 조회하기 때문에 발생합니다.

### 용어 정리
- **JPA**: Java 진영에서 ORM(Object-Relational Mapping) 기술 표준으로 사용하는 인터페이스 모음.
- **ORM**: 우리가 일반적으로 알고 있는 애플리케이션 Class와 RDB(Relational DataBase)의 테이블을 매핑(연결)한다는 뜻.
- **영속성**: 데이터를 생성한 프로그램이 종료되어도 사라지지 않는 데이터의 특성을 말합니다.

## 12. `@Transactional(readOnly = true)`는 어떤 기능인가요? 이게 도움이 되나요?
트랜잭션 내에서 엔티티를 변경하려고 할 때, JPA는 이 변경을 무시합니다. 이는 엔티티가 변경되지 않도록 보장합니다. 트랜잭션이 읽기 전용임을 명시적으로 표현하여 코드의 가독성과 의도를 명확히 할 수 있습니다.

## 13. 그런데, 읽기에 트랜잭션을 걸 필요가 있나요? `@Transactional`을 안 붙이면 되는 거 아닐까요?
데이터베이스 트랜잭션은 일관성 있는 데이터를 읽도록 보장합니다. 여러 테이블에 걸친 읽기 작업에서 데이터 일관성을 유지하려면 트랜잭션이 필요합니다. 단순 조회가 아닌, 여러 연산이나 조건이 결합된 읽기 작업에서는 트랜잭션을 사용하여 작업 도중 데이터 일관성을 유지할 수 있습니다.

### 용어 정리
- **트랜잭션**: 하나 이상의 SQL 문장들로 이루어진 논리적인 작업 단위를 말합니다.