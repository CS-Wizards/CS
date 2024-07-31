# Spring 애플리케이션 동작 방식

Spring을 사용하여 백엔드 개발을 해 왔는데 내부 동작을 잘 모르고 개발을 했기 때문에 어떤 원리로 스프링이 동작하는지 알아보았습니다.

## 웹 동작 방식

먼저, 웹의 동작 방식에 대해 알아야 합니다.

<img src="https://github.com/user-attachments/assets/2ea48e0c-dcd9-4f8e-9709-86de94326b2e" width="100%">

1. 브라우저는 DNS 서버로 가서 웹 사이트가 있는 서버의 진짜 주소를 찾습니다.
2. 브라우저는 웹 사이트 서버로 HTTP/HTTPS 요청을 보냅니다.
3. 웹 서버에서 먼저 요청을 받습니다.
4. 요청 메서드, URL, 헤더, 본문 등을 분석합니다.
5. URL 과 파일 확장자를 검사한 결과 **정적 자원 요청**이라고 판단하면
   1. 이미지, CSS 파일, HTML 파일과 같은 정적 자원을 찾습니다.
   2. 상태 코드 및 헤더를 설정하고 파일 내용을 본문에 포함시킨 HTTP 응답을 브라우저에 전송합니다.
6. **동적 자원 요청**이라고 판단하면
   1. WAS로 요청을 전달합니다.
   2. WAS의 서블릿(자바의 경우)을 사용하여 요청 메서드, URL, 헤더, 본문 등을 분석합니다.
   3. 비즈니스 로직을 실행합니다.
   4. 요청 처리 결과를 서블릿에서 응답 상태 코드, 헤더, 본문 등을 설정하여 HTTP 응답을 웹 서버에 전송합니다.
   5. 웹 서버에서 서블릿으로부터 전달 받은 응답을 브라우저로 전달합니다.

## 서블릿 Servlet

자바 기반의 웹 애플리케이션에서 클라이언트의 요청을 처리하고 동적 웹 콘텐츠를 생성하는 서버 측 컴포넌트입니다. 서블릿은 주로 HTTP 프로토콜을 통해 웹 서버와 통신하며 비즈니스 로직을 제외한 공통적인 부분을 처리해주는 역할을 합니다.

서블릿 컨테이너가 여러 개의 서블릿을 모아 관리하고 웹 서버와 소켓으로 통신하는 역할을 합니다. 이러한 서블릿 컨테이너에는 대표적으로 톰캣이 있습니다.

### 서블릿 동작 방식

1. 사용자가 URL을 입력하면 HTTP Request가 Servelt Container로 전송됩니다.
2. 요청을 전송 받은 Servlet Container 는 HttpRequest, HttpResponse 객체를 생성합니다.
3. 사용자가 요청한 URL이 어느 서블릿에 대한 요청인지 찾습니다(라우팅).
   1. 서블릿이 메모리에 없다면 `init()` 메서드를 호출하여 적재합니다.
4. 해당 서블릿 인스턴스의 `service()` 메서드를 호출하여 HTTP 요청의 메서드 타입(GET, POST 등)을 확인합니다.
5. `service()` 메서드는 요청 메서드 타입에 따라 doGet(), doPost() 등의 메서드를 호출합니다.
6. 메서드 처리 후 동적 페이지를 생성하여 HttpServletResponse 객체에 응답을 보냅니다.
7. 응답 후 HttpServletRequest, HttpServletResponse 객체를 소멸합니다.
   1. 컨테이너가 서블릿에 종료 요청을 하면 `destroy()` 메서드가 호출됩니다. 종료 시 처리해야 하는 작업은 destroy() 메서드를 오버라이딩하여 구현하면 됩니다.

<img src="https://github.com/user-attachments/assets/97bda173-9a98-43bb-8212-699395d9a591" width="100%">

## 서블릿 컨테이너 Servlet Container

### 역할

서블릿의 생명 주기를 관리하고, HTTP 요청을 서블릿으로 라우팅하여 사용합니다. 서블릿의 초기화, 요청 처리, 소멸 등의 작업을 수행합니다.

### 기능

- 서블릿 및 JSP 지원
- HTTP 요청 및 응답 처리
- 서블릿의 생명 주기 관리
- 서블릿 객체를 싱글톤 패턴으로 관리
- 멀티 스레딩 지원

### 예시

Apache Tomcat, Jetty, Resin

## Servlet Container vs. WAS

WAS(Web Application Server)는 서블릿 컨테이너의 기능을 포함하여 더 다양한 엔터프라이즈 기능을 제공하는 서버입니다. 트랜잭션 관리, 보안, 데이터베이스 연결 풀링, 메시징 등을 지원합니다. 즉, WAS는 서블릿 컨테이너보다 더 넓은 개념이라고 말할 수 있습니다.

그렇다면 Java, Spring을 사용하여 개발할 때 주로 사용하는 Apache Tomcat을 왜 WAS라고도 하고 Servlet Container라고도 하는지 알아보았습니다. Tomcat은 WAS로서 데이터베이스 연결 풀링과 같은 기본적인 자원 관리 기능을 제공하지만 **주로 서블릿과 JSP를 실행하는 데 중점**을 두고 있습니다. 웹 애플리케이션을 구동하는 데 가장 기본적인 실행 환경만을 제공하기 때문에 WAS와 Servlet Container라는 용어를 혼용해서 사용하는 것 같습니다.

## Spring MVC

## DispatcherServlet

Spring MVC 에 내장되어 있는 서블릿입니다. Spring MVC 기반의 애플리케이션은 DispatcherServlet을 통해 Tomcat에 들어온 모든 HTTP 요청을 중앙에서 처리하고, 이를 적절한 컨트롤러로 라우팅합니다.

### DispatcherServlet의 동작 방식 - RestAPI

<img src="https://github.com/user-attachments/assets/2c088c8b-3863-4452-a796-6edd89a2c087" width="100%">

1. 클라이언트의 요청을 DispatcherServlet이 수신합니다.
2. DispatcherServlet은 HandlerMapping에서 요청 URL을 적절한 컨트롤러 메서드에 매핑합니다.
   - HandlerMapping은 인터페이스로, `@RequestMapping` 어노테이션을 사용하여 컨트롤러를 구현할 경우 HandlerMapping의 구현체인 `RequestMappingHandlerMapping` 클래스를 사용합니다. 이 클래스에서 애플리케이션 시작 시 모든 컨트롤러 메서드를 스캔하고 `@RequestMapping` 어노테이션이 붙은 메서드들을 매핑 정보(요청 URL, HTTP 메서드, 파라미터 등)로 등록합니다.
     - 이때 내부적으로 Map과 같은 자료구조를 사용하여 매핑 정보를 등록합니다.
   - 매핑된 핸들러 메서드를 찾는다면 이를 HandlerExecutionChain 객체로 반환합니다.
3. DispatcherServlet은 반환된 핸들러를 실행할 수 있는 HandlerAdapter를 선택합니다. `@RequestMapping` 어노테이션을 사용하여 컨트롤러를 구현할 경우 HandlerAdapter의 구현체인 `RequestMappingHandlerAdapter` \*\*\*\*클래스를 사용하여 컨트롤러를 호출합니다.
4. `RequestMappingHandlerAdapter` \*\*\*\*에서 컨트롤러 메서드를 호출합니다.
   1. 호출한 컨트롤러 메서드에서 비즈니스 로직을 처리하고 반환 받은 데이터를 HttpMessageConverter를 거쳐서 Json 형식으로 변환합니다.
      1. `@ResponseBody` 애노테이션이 붙은 메서드의 반환값은 자동으로 HttpMessageConverter를 통해 변환됩니다.
      2. 이 과정에서 Content-type 헤더도 설정됩니다.
   2. 변환된 JSON 데이터를 HandlerAdapter로 보냅니다.
5. HandlerAdapter에 반환된 데이터를 DispatcherServlet으로 보냅니다.
6. DispatcherServlet에서 반환된 데이터를 HTTP 응답 본문에 작성하고 필요한 HTTP 헤더를 설정합니다.
7. 설정된 응답 데이터를 클라이언트에게 전송합니다.

## Filter, Interceptor, AOP

애플리케이션에서 횡단 관심사를 처리하는 Filter, Interceptor, AOP에 간단하게 알아보고 DispatcherServlet과 함께 어떻게 동작하는지 알아보겠습니다.

### Servlet Filter

Filter는 J2EE 표준 스펙 기능으로 Dispatcher Servlet에 요청이 전달되기 전/후에 url 패턴에 맞는 모든 요청에 대해 부가 작업을 처리할 수 있는 기능을 제공합니다. 즉, 스프링 컨테이너가 아닌 톰캣과 같은 웹 컨테이너에 의해 관리가 되는 것이므로 스프링 범위 밖에서 처리됩니다. 따라서 웹 애플리케이션 전체에 공통된 처리가 필요할 때에 주로 사용합니다. Filter 는 Interceptor와 달리 Request와 Response를 조작할 수 있습니다.

```jsx
public interface Filter {

    public default void init(FilterConfig filterConfig) throws ServletException {}

    public void doFilter(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException;

    public default void destroy() {}
}
```

- **init()** : 필터 객체를 초기화하고 서비스에 추가하기 위한 메소드입니다.
- **doFilter()** : url 패턴에 맞는 HTTP 요청이 Dispatcher Servlet으로 전달되기 전에 실행되는 메소드입니다. 여러 필터를 chaining 해서 순차적으로 처리할 수 있습니다.
- **destroy()** : 필터 객체를 서비스에서 제거하고 사용하는 자원을 반환하기 위한 메소드입니다.

### Interceptor

Interceptor는 Dispatcher Servlet이 Controller를 호출하기 전/후에 인터셉터가 가로채는 요청과 응답을 참조하거나 가공할 수 있는 기능을 제공합니다. 필터와 달리 Spring 내부 컨텍스트에서 처리됩니다. 따라서 필터와 달리 요청 처리의 세부 단계별로 처리 로직을 적용해야 할 때 주로 사용합니다. 인터셉터는 필터와 다르게 HttpServletRequest나 HttpServletResponse 등과 같은 객체를 제공받으므로 객체 자체를 조작할 수는 없습니다. 그러나 해당 객체가 내부적으로 갖는 값은 조작할 수 있으므로, 컨트롤러로 넘겨주기 위한 정보를 가공하기에 용이합니다.

```jsx
public class MyInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
            throws Exception {
        System.out.println("Pre Handle method is Calling");
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
                           ModelAndView modelAndView) throws Exception {
        System.out.println("Post Handle method is Calling");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)
            throws Exception {
        System.out.println("Request and Response is completed");
    }
}
```

- **preHandle()** : 컨트롤러가 호출되기 전에 실행됩니다. preHandle의 반환 값이 true이면 다음 단계로 진행되지만, false라면 작업을 중단하여 다음으로 넘어가지 않습니다.
- **postHandle()** : 컨트롤러를 호출된 후에 실행됩니다. 컨트롤러 하위 계층에서 작업을 진행하다가 중간에 예외가 발생하면 postHandle()은 호출되지 않습니다.
- **afterCompletion()** : 모든 뷰에서 최종 결과를 생성하는 일을 포함해 모든 작업이 완료된 후에 실행됩니다. 컨트롤러 하위 계층에서 예외가 발생해도 afterCompletion()은 반드시 호출됩니다.

### AOP

Spring AOP는 스프링 프레임워크에서 제공하는 기능 중 하나로 관점 지향 프로그래밍을 지원하는 기술입니다. Spring AOP는 로깅, 트랜잭션 관리 등과 같은 공통적인 관심사를 모듈화하여 중복 코드를 줄이고 유지 보수성을 향상하는 데 도움을 줍니다.

AOP란 Aspected-Oriented Programming 의 약자로, 메소드나 객체의 기능을 핵심 관심사와 공통 관심사로 나누어 프로그래밍을 하는 것을 말합니다. 핵심 관심사는 비즈니스 로직과 관련된 기능이며 공통 관심사는 비즈니스 로직과는 별개지만 많은 코드에서 사용해야 하는 기능을 의미합니다. 각 코드에 같은 동작을 하는 코드를 넣으면 중복이 많아져 유지 보수하기 어렵기 때문에 그 부분만 떼서 모듈화 시켜 원하는 기능에만 붙여넣고 싶을 때 사용할 수 있습니다.

**주요 용어**

| 용어       | 설명                                                                                              |
| ---------- | ------------------------------------------------------------------------------------------------- |
| Aspect     | 공통적인 기능들을 모듈화 한것을 의미합니다.                                                       |
| Target     | Aspect가 적용될 대상을 의미하며 메소드, 클래스 등이 이에 해당 됩니다.                             |
| Join point | Aspect가 적용될 수 있는 시점을 의미하며 메소드 실행 전, 후 등이 될 수 있습니다.                   |
| Advice     | Aspect의 기능을 정의한 것으로 메서드의 실행 전, 후, 예외 처리 발생 시 실행되는 코드를 의미합니다. |
| Point cut  | Advice를 적용할 메소드의 범위를 지정하는 것을 의미합니다.                                         |

**주요 어노테이션**

| 메서드          | 설명                                                                 |
| --------------- | -------------------------------------------------------------------- |
| @Aspect         | 해당 클래스를 Aspect로 사용하겠다는 것을 명시합니다.                 |
| @Before         | 대상 “메서드”가 실행되기 전에 Advice를 실행합니다.                   |
| @AfterReturning | 대상 “메서드”가 정상적으로 실행되고 반환된 후에 Advice를 실행합니다. |
| @AfterThrowing  | 대상 “메서드에서 예외가 발생”했을 때 Advice를 실행합니다.            |
| @After          | 대상 “메서드”가 실행된 후에 Advice를 실행합니다.                     |
| @Around         | 대상 “메서드” 실행 전, 후 또는 예외 발생 시에 Advice를 실행합니다.   |

**사용 예시**

```java
@Aspect
@Component
public class LoggingAspect {

    @Before("execution(* com.example.service.*.*(..))")
    public void logBefore() {
        System.out.println("Method execution started");
    }
}

```

- **Aspect** : LoggingAspect 클래스
- **Target** : `com.example.service` 패키지 내의 모든 메서드
- **Join point** : `com.example.service` 패키지의 메서드 실행 전 시점
- **Advice** : 메서드 실행 전에 logBefore() 수행
- **Point cut** : `"execution(* com.example.service.*.*(..))"` - `com.example.service` 패키지 내의 모든 클래스의 모든 메서드가 대상

**동작 방식**

1. Spring Application Context가 스프링 컨테이너를 생성하면서 빈을 생성할 때 `@Aspect` 어노테이션이 붙은 클래스도 함께 등록됩니다.
2. 스프링 컨테이너가 빈 인스턴스를 생성합니다. 이때 AOP 적용 대상이 되는 빈에 대한 프록시를 생성할 준비를 합니다.
3. 스프링 컨테이너는 생성된 빈에 필요한 의존성을 주입합니다.
4. 초기화 콜백이 실행됩니다.
5. AOP 프록시를 생성합니다.
   - 스프링은 빈 후처리기를 통해 `@Aspect`가 붙은 클래스와 포인트컷 정의를 분석하고, AOP 적용 대상 빈을 식별합니다.
   - 스프링 컨테이너는 `@Aspect`의 **포인트컷에 매칭되는 빈에 대해 프록시를 생성** 합니다. 이 단계에서 JDK 동적 프록시나 CGLIB 프록시가 사용됩니다.
6. 클라이언트가 빈의 메서드를 호출하면, 실제 빈 인스턴스가 아닌 프록시 객체가 호출을 가로챕니다. 프록시 객체는 설정된 포인트 컷에 따라 어드바이스를 실행합니다.

### 정리

<img src="https://github.com/user-attachments/assets/794af238-8737-464c-a3d5-340bc55db0d3" width="100%">

1. **클라이언트 요청 수신**
   - 클라이언트가 HTTP 요청을 보냅니다.
   - 웹 서버(Tomcat 등)는 이 요청을 수신하여 `DispatcherServlet`으로 전달합니다.
2. **Filter 동작**
   - `Filter`는 `DispatcherServlet` 앞단에서 요청을 전처리합니다.
   - 예를 들어, 인증, 로깅, 요청 데이터 검증 등의 작업을 수행합니다.
   - `FilterChain`을 통해 다음 필터 또는 `DispatcherServlet`으로 요청을 전달합니다.
3. **DispatcherServlet 요청 처리**
   - `DispatcherServlet`이 요청을 수신합니다.
   - `HandlerMapping`을 사용하여 요청 URL을 적절한 컨트롤러 메서드에 매핑합니다.
   - 매핑된 핸들러와 인터셉터를 포함한 `HandlerExecutionChain` 객체를 반환받습니다.
4. **Interceptor 전처리**
   - `HandlerExecutionChain`에 포함된 인터셉터의 `preHandle` 메서드가 호출됩니다.
   - 요청 전처리를 수행합니다. 예를 들어, 권한 검사, 추가 로깅 등을 수행할 수 있습니다.
5. **HandlerAdapter 선택 및 컨트롤러 호출**
   - `DispatcherServlet`은 반환된 핸들러를 실행할 수 있는 적절한 `HandlerAdapter`를 선택합니다.
   - `HandlerAdapter`는 컨트롤러 메서드를 호출합니다.
   - 컨트롤러 메서드 호출 전후로 설정한 AOP 모듈이 동작합니다.
   - 컨트롤러 메서드는 비즈니스 로직을 처리하고, 데이터를 반환합니다.
6. **HttpMessageConverter를 통한 데이터 변환**
   - 컨트롤러 메서드가 반환한 데이터를 `HttpMessageConverter`를 통해 JSON, XML 등의 형식으로 변환합니다.
   - 이 과정에서 `Content-Type` 헤더도 설정됩니다.
7. **Interceptor 후처리**
   - 인터셉터의 `postHandle` 메서드가 호출되어 응답 전처리를 수행합니다.
8. **헤더 설정 및 응답 반환**
   - `DispatcherServlet`은 변환된 데이터를 HTTP 응답 본문에 작성하고, 필요한 HTTP 헤더를 설정합니다.
   - 설정된 응답 데이터를 클라이언트에게 전송합니다.
9. **Filter 후처리**
   - 응답이 생성된 후, `Filter`에서 후처리를 수행할 수 있습니다.
   - 예를 들어, 응답 로깅, 응답 데이터 수정 등의 작업을 수행할 수 있습니다.

## 참고

[웹의 동작 방식 - Web 개발 학습하기 | MDN](https://developer.mozilla.org/ko/docs/Learn/Getting_started_with_the_web/How_the_Web_works)<br/>
[Servlet 과 ServletContainer](https://tecoble.techcourse.co.kr/post/2021-05-23-servlet-servletcontainer/)<br/>
[[Spring] Dispatcher-Servlet(디스패처 서블릿)이란? 디스패처 서블릿의 개념과 동작 과정](https://mangkyu.tistory.com/18)<br/>
[[Java] Spring Boot AOP(Aspect-Oriented Programming) 이해하고 설정하기](https://adjh54.tistory.com/133)
