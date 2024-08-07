## AOP

### Aspect-Oriented Programming

[https://velog.velcdn.com/images/kai6666/post/af28f068-5fce-47ef-9632-43f9c32779cb/image.png](https://velog.velcdn.com/images/kai6666/post/af28f068-5fce-47ef-9632-43f9c32779cb/image.png)

> **AOP(Aspect-Oriented Programming)**
관점 지향 프로그래밍으로, 관점을 기준으로 다양한 기능을 분리하여 보는 프로그래밍. 관점이란 부가 기능과 그 적용처를 정의하고 합쳐서 모듈로 만든 것이다.
> 
- 웹 애플리케이션이라면 사업에 핵심적인 비즈니스 로직이 있고, 애플리케이션을 관통하는 부가 기능 로직이 있음
- 여기서 부가 기능 로직을 **횡단 관심사 (cross-cutting)**이라고 함
    - ex) 로깅, 보안, 트랜잭션
- 예를 들어 모든 서비스가 동작하는 시간을 측정하고자 했을 때, AOP가 없다면 모든 서비스 메소드에 시간을 측정하는 코드를 추가해야 하는 대규모의 수정이 발생
- 이러한 경우에 횡단 관심사의 코드를 핵심 비즈니스 로직의 코드와 분리하여 코드의 간결성을 높이고 변경에 유연함과 무한한 확장이 가능하도록 하는 것이 목적

### AOP → Proxy Patterm

- 예를 들어 클라이언트가 주문을 하는 위와 같은 상황이 있다고 가정
- @Aspect를 적용하면 프록시를 만들어 줌
    - 프록시는 대리인이라는 뜻으로 내가 원하는 횡단 관심사의 일을 수행하는 애
    - 이외의 핵심 기능은 원래의 구현체인 OrderService에게 넘김
- Proxy 를 만드는 첫 번째 방식 → 인터페이스 구현
    
    [https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/ds5ZlU/btrkAuAB0Kg/S5UaDKt9tPJcLoqdsSL9RK/img.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/ds5ZlU/btrkAuAB0Kg/S5UaDKt9tPJcLoqdsSL9RK/img.png)
    
    - 클라이언트는 인터페이스만 알고 있고 실제로 주입되는 것이 뭐인지 모르게 함 → DI 활용
    - 서비스와 레포지토리 모두 AOP로 구현되었다면 런타임 시 의존 관계는 아래와 같은 모습
        
        [https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/bxUE4B/btrkAvGisZL/1vup0vS6uxSJyCcPUUTVGK/img.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/bxUE4B/btrkAvGisZL/1vup0vS6uxSJyCcPUUTVGK/img.png)
        
- Proxy 를 만드는 두 번째 방식 → 구체클래스 상속
    
    https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/b73aMd/btrkuIUU9lt/OBHCHKkGEw7Rv7ZPzQDhUK/img.png
    
    - 프록시는 구체 클래스를 상속받으면서 동시에 필드로도 가지고 있어야 함
        - 핵심 기능은 타겟에게 위임하기 때문

### @Aspect

- Target
    - 핵심 기능을 담고 있는 모듈로 부가 기능을 부여할 대상
- Join Point
    - 어드바이스가 적용될 수 있는 위치
    - 타겟 객체가 구현한 인터페이스의 모든 메서드
- Point Cut
    - 어드바이스를 적용할 타겟의 메서드를 선별하는 정규표현식
    - 포인트컷 표현식은 execuation으로 시작하고 메서드의Signature를 비교하는 방법을 주로 이용
- Advice
    - 어드바이스는 타겟의 특정 포인트에 제공할 부가 기능
- Aspect
    - Advice + Pointcut
    - Aspect를 빈으로 등록해서 프록시를 만들고 관리

## Annotation

### Java Annotation

- 주석이라는 뜻
- 프로그램에 추가적인 정보를 제공해 주는 메타데이터
- 컴파일러에게 코드 작성 문법 에러를 체크하도록 정보 제공
- 소프트웨어 개발툴이 빌드나 배치 시 코드를 자동으로 생성할 수 있도록 정보 제공
- 런타임 시 특정 기능을 실행하도록 정보 제공 → 스프링에서 주로 사용하는 방법

### Spring Annotation

- 스프링에서도 Custom Annotation을 사용해 입력값 검증이나 런타임 시 값 체크 수행 가능
- 어노테이션의 속성을 정의하는 어노테이션을 Meta Annotation이라고 함

```java
@Documented
@Constraint(validatedBy = CategoriesExistValidator.class)
@Target( { ElementType.METHOD, ElementType.FIELD, ElementType.PARAMETER })
@Retention(RetentionPolicy.RUNTIME)
public @interface ExistCategories {

    String message() default "해당하는 카테고리가 존재하지 않습니다.";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}
```

```java
@Component
@RequiredArgsConstructor
public class CategoriesExistValidator implements ConstraintValidator<ExistCategories, List<Long>> {

    private final FoodCategoryRepository foodCategoryRepository;

    @Override
    public void initialize(ExistCategories constraintAnnotation) {
        ConstraintValidator.super.initialize(constraintAnnotation);
    }

    @Override
    public boolean isValid(List<Long> values, ConstraintValidatorContext context) {
        boolean isValid = values.stream()
                .allMatch(value -> foodCategoryRepository.existsById(value));

        if (!isValid) {
            context.disableDefaultConstraintViolation();
            context.buildConstraintViolationWithTemplate(ErrorStatus.FOOD_CATEGORY_NOT_FOUND.toString()).addConstraintViolation();
        }

        return isValid;

    }
}
```

- `@Target( { ElementType.METHOD, ElementType.FIELD, ElementType.PARAMETER })` : 어디에 적용될 어노테이션인지
- `@Retention(RetentionPolicy.RUNTIME)`: 언제 실행될 것인지

### Lombok

- getter, setter, equals와 같은 메소드를 자동으로 만들어 주는 라이브러리
- `@Data` = `@Getter` +  `@Setter` + `@ToString` + `@EqualsAndHashCode` + `@RequiredArgsConstructor`
- boilerplate (재사용 가능한 코드)를 생성해 줌
- 많은 어노테이션을 포함한 만큼 각 어노테이션의 주의점을 모두 가짐
- `@EqualsAndHashCode`: 객체를 Set에 저장한 뒤 필드값을 변경하면 hashCode가 변경되어 이전에 저장한 객체를 찾을 수 없음
- `@RequiredArgsConstructor`: 초기화되지 않은  final 필드, @NonNull과 같이 제약 조건이 설정되어 있는 모든 필드에 대한 생성자를 자동으로 생성
    - 예를 들어 int 멤버가 두 개 있는 클래스에 대한 객체를 만들고 두 멤버의 순서를 바꾸면, lombok이 생성자의 파라미터 순서를 필드 선언 순서에 따라 자동으로 변경해 줘 입력된 값이 바뀌어 들어감

## Tomcat

### 웹 서버 vs HTTP 서버

[https://velog.velcdn.com/images/kdhyo/post/65c93d6d-13e2-4e68-91c3-feed25e702bb/다운로드.jpg](https://velog.velcdn.com/images/kdhyo/post/65c93d6d-13e2-4e68-91c3-feed25e702bb/다운로드.jpg)

- 웹 서버
    - 웹 서버 소프트웨어와 웹 사이트의 구성 요소 파일을 저장하는 컴퓨터
    - 하드웨어
- HTTP 서버
    - URL 및 HTTP를 이해하는 소프트웨어
    - 웹 사이트의 도메인 이름을 통해 액세스할 수 있으며 호스팅된 웹 사이트의 콘텐츠를 최종 사용자의 장치로 전달
    - 소프트웨어

### 아파치 서버

- 클라이언트에서 요청하는 HTTP 요청을 처리하는 웹서버
- 정적 타입 데이터만을 처리하기 때문에 톰캣이라는 것이 등장

### 톰캣

- WAS (Web application server)
- 아파치 소프트웨어 재단에서 후원하고 있으며 오픈소스로 개발이 되고 있음
- 컨테이너, 웹 컨테이너, 서블릿 컨테이너
- 아파치 서버와는 다르게 DB 연결, 다른 응용프로그램과 상호 작용 등 동적인 기능 사용 가능

### 용어 정리

- 컨테이너: 동적인 데이터들을 가공하여 정적인 파일로 만들어 주는 모듈
- 서블릿: 클라이언트의 요청을 받고 요청을 처리하여 결과를 클라이언트에게 제공하는 자바 인터페이스
- 서블릿 컨테이너: 서블릿들을 모아 관리
- WAS: 동적 타입을 제공하는 소프트웨어 프레임워크

### Netty

- 비동기 이벤트 기반 네트워크 응용프로그램 프레임워크
- 자바 소켓 프로그래밍에 비해 네티의 추상화로 인해 편하고 간결하게 네트워크 프로그래밍 가능
- 소켓의 모드와 상관없이 개발할 수 있도록 추상화된 전송 API를 제공
- 소켓 모드를 바꾸기 위해서 데이터 송수신 부분의 로직을 고치지 않아도 됨
- 안정적이고 빠른 속도를 자랑

## HTTP

### HTTP

- HyperText Transfer Protocol의 약자로 주로 HTML과 같은 HyperText 문서를 주고받기 위해 만들어짐
- 비연결성과 무상태성의 특성
- 비연결성
    - 처음 연결을 맺은 후 요청과 한 번의 응답 이후 연결 종료
    - 매 요청마다 다시 연결
- 무상태성
    - 프로토콜에서 Client의 상태를 기억하지 않음
    - Client의 상태를 보관하기 위해 쿠키나 세션, JWT 토큰 등을 이용

### 대칭키

[https://velog.velcdn.com/images/gs0351/post/e6ba5378-7c0d-4e3b-9106-1ce0055bb1b3/image-20201228143331890.png](https://velog.velcdn.com/images/gs0351/post/e6ba5378-7c0d-4e3b-9106-1ce0055bb1b3/image-20201228143331890.png)

- 암복호화에 사용하는 키 동일
- 암호화 속도가 빠름
- 키를 교환해야 하는 문제, 탈취 관리 걱정
- 기밀성을 제공하거나 무결성/인증/부인방지를 보장하지는 않음
- **SEED**, DES, 3DES, AES, ARIA

### 공개키

[https://velog.velcdn.com/images/gs0351/post/f8e3eb30-2eda-47ac-954e-915515066bbc/image-20201228143511804.png](https://velog.velcdn.com/images/gs0351/post/f8e3eb30-2eda-47ac-954e-915515066bbc/image-20201228143511804.png)

- 암복호화에 사용하는 키 다름
- 속도가 느림
- 기밀성/인증/부인방지 기능 제공
- 방식
    - 암호 모드: 송신자 공개키로 암호화 → 송신자 사설키로 복호화
    - 인증 모드: 송신자 사설키로 암호화 → 송신자 공개키로 복호화
- RSA, DSA, ECC

### SSL과 TLS

|  | Secure Sockets Layer | Transport Layer Security |
| --- | --- | --- |
| 의미 | 보안 소켓 계층 | 전송 보안 계층 |
| 버전 |  TLS로 대체 | SSL의 업그레이드 버전 |
| 알림 | 두 가지의 알림 메시지만 존재하며 암호화 X | 다양한 암호 메시지가 존재하며 암호화 O |
| 메시지 | MAC 사용 | HMAC 사용 |
| 암호 | 구식 암호화 알고리즘 | 고급 암호화 알고리즘 |
| 핸드셰이트 | 핸드셰이트 단계가 많고 느림 | 핸드셰이크 단계가 적고 빠름 |
- 핸드셰이크는 브라우저가 서버의 SSL 또는 TLS 인증서를 인증하는 프로세스
- 양 당사자를 인증한 다음 암호화 키를 교환
- SSL 핸드셰이크는 명시적 연결인 반면 TLS 핸드셰이크는 암시적 연결

## WebSocket vs Socket

### WebSocket

- 푸시나 채팅 알림은 내가 요청을 해야 오는 알림이 아닌 서버에서 변동 사항이 있으면 사용자에게 자동으로 보내는 알림
- HTTP로 일정 시간이 지나면 HTTP  요청을 발생시켜서 정보를 얻을 수 있음 → HTTP polling
- 서버 측에서 먼저 응답을 보내는 요청을 발생시키는 것이 어려움
- WebSocket: 클라이언트와 서버간의 TCP 연결을 길게 유지하고, 서로 양방향으로 데이터를 전달하게 해 주는 통신 규약
- 푸시 알림처럼 서버에서만 보내는 알림은 Server Side Event (SSE) 사용하기도 함
- WebSocket 통신 단계
    1. HTTP에 Upgrade라는 헤더를 포함해서 WebSocket을 사용할 것이라고 명시 → Protocol Handshake
        
        [https://velog.velcdn.com/images/morion002/post/5489a3b4-c75c-437b-80e2-790d46b3c129/image.png](https://velog.velcdn.com/images/morion002/post/5489a3b4-c75c-437b-80e2-790d46b3c129/image.png)
        
    2. 이후 서버에서 사용해도 된다며 100 번대  Handshake 응답을 보내고 웹소켓을 사용하기 위한 준비 단계 돌입
        
        [https://velog.velcdn.com/images/morion002/post/9b3a4b7c-6e7a-4fbf-9ac5-ff46d3508f0e/image.png](https://velog.velcdn.com/images/morion002/post/9b3a4b7c-6e7a-4fbf-9ac5-ff46d3508f0e/image.png)
        
    3. 이후 연결이 종료될 때까지 양방향 통신
        
        [https://velog.velcdn.com/images/morion002/post/4c16b926-02fd-47fc-ac71-ff38a7476211/image.png](https://velog.velcdn.com/images/morion002/post/4c16b926-02fd-47fc-ac71-ff38a7476211/image.png)
        

### Socket

- TCP/ID 모델엠서 어플리케이션 계층의 Application Layer에서 애플리케이션이 네트워크 통신을 할 수 있게 해 주는 인터페이스
- 소켓은 애플리케이션이 네트워크 스택을 직접 다루지 않고 운영체제를 통해 통신하도록 하여 보안과 안정성 유지
- 네트워크 상에서 소켓은 두 프로그램 간 양방향 통신의 엔드포인트
- 소켓은 IP 주소와 포트 번호의 조합 → 특정 애플리케이션을 식별하고 TCP나 UDP를 통해 데이터가 해당 애플리케이션으로 전달될 수 있게 하기 위함

### Socket의 실행 흐름

!https://velog.velcdn.com/images/rhdmstj17/post/323aa1d3-4338-4cfe-9edf-45c1f3113e9d/image.png

**클라이언트 입장**

- 클라이언트 소켓 생성
    - 연결 대상에 대한 정보가 들어 있지 않은 껍데기 소켓 생성
    - TCP → stream 타입 소켓, UDP → 데이터그램 타입 소켓
- 연결 요청
    - 연결하고 싶은 대상에게 연결 요청 → IP 주소와 서비스 포트 번호로 연결하고 싶은 타겟 대상 특정
    - 단순히 요청을 보내서 끝나는 것이 아니고, 그 요청에 대한 결과가 돌아와야지만 Connect의 실행이 끝
- 데이터 송수신
    - 요청을 보낸다고 끝나는 게 아니라 요청에 대한 결과가 들어와야 실행 종료
    - 송신할 때는 데이터를 보내는 것이기 때문에 데이터를 얼마나 보낼 것인지 알 수 있지만, 수신할 때는 상대방이 데이터의 크기를 알 수 없음
    - 수신하는 API 는 별도의  Thread에서 진행
- 소켓 닫기
    - 더 이상 데이터의 송수신이 없다고 판단되면 소켓 닫기

**서버 입장**

- 서버 소켓 생성
    - 클라이언트 소켓과 마찬가지로 연결 대상에 대한 정보가 없는 껍데기 소켓 생성
- 바인딩
    - 네트워크 스택에서 소켓을 식별할 때는 여러 값을 바인드해 줘야 함
    - IP 버전, 포트, 소켓 타입 등을 바인드
    - 바인드 후에 소켓 식별이 가능해짐
    - bind() 콜은 서버에만 존재하고 클라이언트에는 없는 콜
    - 바인드가 없으면 운영체제가 아무 포트 번호만 사용해 버림
    - 서버는 포트 번호가 일정해야 하기 때문에 바인드 필요
    - 하나의 프로세스는 동일한 포트 번호를 가진 여러 소켓을 결합할 수 있음
        - 채팅 앱 등등
        - 일을 분산시킴 fork() 콜
- listen() - TCP만
    - 소켓이 최대로 받을 연결 개수를 지정하는 함수
- accept() -  TCP만
    - 바로 데이터를 송수신하지 않고 객체와 주체간의 논리적 연결 생성
    - 3-way handshake 는 네트워크 스택에서 처리
- recv() -  TCP / recvform() - UDP
    - TCP
        - accept를 통해 세션이 수립된 후 fork()를 호출해 실제 수신, 응답에 필요한 나머지 처리를 자식 프로세스에서 처리
        - 자식 프로세스는 recv()를 사용하고 wait 상태에 들어가 네트워크 스택이 데이터를 소켓에 보내 주기를 기다림
        - 이후 read() 시스템 콜을 통해 소켓 파일에 있는 데이터를 읽음
    - UDP
        - listen() accept()를 거치지 않고 bind()를 하자마자 fork()를 통해 자식 프로세스를 만듦
        - recvform() 시스템 콜을 통해 데이터를 기다림
        - read() 로 UDP 소켓에서 필요한 데이터를 읽어들여 애플리케이션에서 사용
- close()
    - 사용한 소켓 파일 삭제

## TCP/UDP

## Transport Layer

- 전송 계층은 전송을 담당하는 계층
- 전송 계층에는 TCP 뿐만 아니라 사용자 데이터그램  통신 규약(User Datagram Protocol)도 존재
- 데이터 단위는 세그먼트

### TCP

- Transmission Control Protocol
- 전송을 제어하는 규약
- TCP는 패킷을 추적 및 관리하고 IP는 데이터의 배달을 처리
- 단점
    - 데이터로 보내기 전에 반드시 연결되어야 함
    - 1:1 통신만 가능
    - 고정된 통신 선로가 최단선이 아닐 경우 상대적으로 UDP보다 데이터 전송 속도가 느림
- 특징
    - 연결형 서비스로 연결이 성공해야 통신 가능
    - 데이터의 경계를 구분하지 않음
    - 데이터의 전송 순서 보장
        - 데이터의 순서 유지를 위해 각 바이트마다 번호가 부여됨
    - 신뢰성 있는 데이터 전송
        - Sequence Number: TCP 세그먼트의 연속된 데이터 번호
        - Ack Number: 상대방으로부터 받아야 하는 다음 TCP 세그먼트 데이터 번호
    - 데이터 흐름 제어 및 혼잡 제어
    - 연결의 설정 (3-way Handshake)와 해제 (4-way Handshake)

### 3-way Handshake

[https://velog.velcdn.com/images/devharrypmw/post/b51e0e6d-be93-418c-9429-3ac6d9227a40/image.gif](https://velog.velcdn.com/images/devharrypmw/post/b51e0e6d-be93-418c-9429-3ac6d9227a40/image.gif)

- 클라이언트에서 서버에 연결 요청을 하기 위해 SYN 데이터 전송
- 서버에서 해당 포트가 listen() 상태로 SYN 데이터를 받고 SYN_RCV로 상태가 변경
- 요청을 정상적으로 받았다는 대답 ACK과 클라이언트도 포트를 열어 달라는 SYN를 같이 전송
- 클라이언트에서는 ACK + SYN을 같이 받은 후 established로 상태를 변경하고 ACK 전송
- ACK를 받은 서버는 상태가 established로 변경

### UDP

- User Datagream Protocol
- 비연결 지향적 프로토콜
- 데이터를 주고받을 때 연결 절차를 거치지 않고 발신자가 일방적으로 데이터를 발신하는 방식
- 연결 과정이 없어 TCP보다 빠른 전송이 가능하지만 데이터가 유실될 수도 있고, 데이터 간의 전송 순서가 보장되지 않음
- 데이터그램 방식: 데이터 전송 전에 송수신자 사이에 가상 회선이라 불리는 논리적 경로를 설정하지 않고 패킷들이 각기 독립적으로 전송
- 패킷 관리 필요
