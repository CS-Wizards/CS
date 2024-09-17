# Spring Security

## 개념

스프링 애플리케이션에서 인증(Authentication)과 권한 부여(Authorization)를 처리하는 보안 프레임워크입니다. 주로 웹 애플리케이션과 API에서 사용되며 애플리케이션의 리소스를 보호하고 사용자 인증 절차와 권한 관리 등을 쉽게 구현할 수 있도록 도와줍니다.

## 사용 목적

1. **인증(Authentication)**

   사용자가 누구인지 확인하는 과정입니다. 주로 로그인 폼, OAuth, JWT 등을 통해 사용자 인증을 처리합니다.

2. **권한 부여(Authorization)**

   인증된 사용자가 특정 리소스에 접근할 수 있는 권한이 있는지 확인하는 과정입니다. 사용자 권한을 기반으로 리소스 접근을 제한하거나 허용합니다.

3. **CSRF, 세션 고정 공격 등 보안 위협에 대한 보호**

   Spring Security는 이러한 보안 위협을 자동으로 방지하는 기능을 제공합니다.

4. **암호화 및 데이터 보호**

   사용자 비밀번호 암호화, 데이터 전송 중 보안 등의 기능을 제공합니다.


## 사용 방법

### 1. 의존성 추가

build.gradle 또는 pom.xml에 의존성을 추가합니다.

```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

### 2. 기본 보안 설정

Spring Security 의존성을 추가하면 기본적으로 모든 요청이 인증을 요구하게 됩니다. 기본 설정으로는 로그인 페이지가 자동으로 생성되며 사용자는 user라는 기본 사용자명과 자동 생성된 비밀번호로 로그인 할 수 있습니다.

### 3. 사용자 정의 보안 설정

Spring Security는 SecurityConfig 클래스를 만들어 보안 규칙을 커스텀할 수 있습니다. 예를 들어, 특정 URL 경로에 대해 인증을 요구하지 않거나 로그인 페이지를 커스터마이징 할 수 있습니다.

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/public/**").permitAll()  // /public 경로는 인증 없이 접근 가능
                .anyRequest().authenticated()           // 그 외 경로는 인증 필요
            .and()
            .formLogin()  // 기본 로그인 폼 사용
                .loginPage("/login")  // 커스텀 로그인 페이지 설정
                .permitAll()
            .and()
            .logout()  // 로그아웃 설정
                .permitAll();
    }
}
```

### 4. 인증 방식 설정

사용자 인증은 여러 방식으로 구현할 수 있습니다. 예를 들어, 데이터베이스에 저장된 사용자 정보를 기반으로 인증할 수 있으며 아래는 UserDetailservice를 이용한 사용자 인증 예시입니다.

UserDetailService는 Spring Security에서 사용자 정보를 관리하기 위한 인터페이스입니다.

```java
@Bean
@Override
public UserDetailsService userDetailsService() {
    UserDetails user =
         User.withDefaultPasswordEncoder() // 기본적인 비밀번호 인코더를 사용하여 사용자 정보 생성 
             .username("user")
             .password("password")
             .roles("USER")
             .build();

    return new InMemoryUserDetailsManager(user); // 메모리 내에서 사용자 정보를 관리합니다.
}
```

### 5. OAuth 2.0 및 JWT 사용

Spring Security는 OAuth 2.0 인증 및 JWT 방식도 지원하여 소셜 로그인, 토큰 기반 인증을 쉽게 구현할 수 있습니다.

## 동작 원리

<img src="https://github.com/user-attachments/assets/915d8e67-94ad-427c-a980-4655dd787f48" width="50%"/>

기본적으로 스프링 애플리케이션에서 클라이언트의 요청은 was의 필터를 거쳐 스프링 컨테이너의 컨트롤러에 도달합니다.

스프링 시큐리티를 사용하면

1. WAS의 필터에 하나의 필터를 만들어서 넣고 해당 필터에서 요청을 가로챕니다.
2. 해당 요청은 스프링 컨테이너 내부에 구현되어 있는 스프링 시큐리티 감시 로직을 거칩니다.
3. 시큐리티 로직을 마친 후 다시 WAS의 다음 필터로 복귀합니다.

### 시큐리티 로직

<img src="https://github.com/user-attachments/assets/f1157bf3-050b-406c-9b04-aa8bf33bb298" width="50%"/>

스프링 시큐리티 로직은 여러 개의 필터들이 나열된 필터 체인 형태로 구성되어 있습니다. 각각의 필터에서 CSRF, 로그아웃, 로그인, 인가 등 실질적인 시큐리티 검증 작업이 수행됩니다.

<img src="https://github.com/user-attachments/assets/499f8dc5-e0a8-4186-a5c2-cc6edab3e4eb" width="50%"/>

시큐리티 로직에는 여러 개의 필터 체인을 등록할 수 있습니다. 특정한 로직에 특정한 필터 체인을 통과하도록 설정할 수 있습니다.

> **DelegatingFilterProxy**
스프링 Bean을 찾아 요청을 넘겨주는 서블릿 필터
>

> **FilterChainProxy**
스프링 시큐리티의 의존성을 추가하면 DelegatingFilterProxy에 의해 호출되는 SecurityFilterChain들을 들고 있는 Bean
>

> **SecurityFilterChain**
스프링 시큐리티 필터들의 묶음으로 실제 시큐리티 로직이 처리되는 부분. FilterChainProxy가 SecurityFilterChain들을 들고 있다.
>

### DelegatingFilterProxy

요청을 가로채서 스프링 컨테이너에 들어있는 FilterChainProxy라는 빈에게 요청을 던져주는 매개체, 즉 Proxy역할만 수행합니다. 실제 로직을 수행하지는 않습니다.

DelegatingFilterProxy는 어플리케이션을 실행했을 때 SecurityAutoConfiguration이라는 설정 클래스에 의해 자동으로 등록이 됩니다.

### FilterChainProxy

springSecurityFilterChain이라는 이름으로 등록 되어있습니다. 내부적으로 등록되어 있는 FilterChain들을 리스트로 가지고 있습니다.

DelegatingFilterProxy가 넘긴 요청을 어느 FilterChain에 넘길지 판단하여 요청을 전달하는 역할을 가집니다.

### SpringContextHolder

<img src="https://github.com/user-attachments/assets/610fd25c-d71e-467e-9bf4-21c33487c8ca" width="50%" />

SecurityFilterChain에 의해 필터별 작업이 이루어집니다. 각 필터는 기능별로 분업되어 로그인, cors, csrf, 로그아웃 등을 진행합니다. 각각의 상태를 알아야 다음 필터가 작업할 수 있습니다. 이러한 상태를 저장하는 공간을 SecurityContextHolder라고 합니다.

<img src="https://github.com/user-attachments/assets/610fd25c-d71e-467e-9bf4-21c33487c8ca" width="50%" />

예를 들어, 인가 필터의 작업을 위해 유저의 ROLE 정보가 필요한데, 앞단의 필터에서 유저에게 ROLE 값을 부여한 결과를 인가 필터까지 공유해야 확인할 수 있습니다. 이러한 정보는 Authentication이라는 객체에 담깁니다. Authentication 객체는 SecurityContext에 의해 관리되며 이 SecurityContext는 SecurityContextHolder에 의해 관리됩니다.

> SecurityContext는 접속 유저 한 명 당 한 개씩 생성됩니다.
>

## SecurityFilterChain의 필터

> 공통된 구조 설명
>

<img src="https://github.com/user-attachments/assets/d369dfcc-0bd2-4596-bd7d-7dba2dea2c00" width="50%"/>

각각의 필터는 조상이 모두 동일합니다. 필터의 기반이 되는 필터 클래스를 만들어두고 해당 클래스를 상속받아 각 특성에 맞게 구현되어 있습니다.

### OncePerRequestFilter

GenericFilterBean 기반으로 작성된 추상 클래스로 클라이언트의 한 번 요청에 대해 내부적으로 동일한 서블릿 필터를 여러번 거칠 경우 한 번만 반응하도록 설계되어 있습니다.

ex. csrf 필터

### GenericFilterBean

자바 서블릿 필터 기반 추상 클래스로 구현되어 있으며 자바 서블릿 영역에서 스프링 영역에 접근할 수 있도록 작성되어 있습니다.

> 클라이언트의 한 번의 요청에 대해 몇 번의 내부 로직이 실행되느냐에 차이가 있습니다.
>

### 필터 구조

```java
init(); // 서블릿 컨테이너 실행 시 필터를 생성하고 초기화
doFilter(); // 요청에 대한 작업 수행 및 다음 필터 호출
destroy(); // 서블릿 컨테이너 종료 시 초기화하는 메소드
```

## 각 필터의 동작 로직

### DisableEncodeUrlFilter

DefaultSecurityFilterChain에 기본적으로 등록되는 필터로 가장 첫 번째에 위치합니다.

필터가 등록되는 목적은 URL 파라미터에 세션 ID가 인코딩되어 로그로 유출되는 것을 방지하기 위함입니다.

```java
http
        .sessionManagement((manage) -> manage.disable()); // 비활성
```

### WebAsyncManagerIntegrationFilter

DefaultSecurityFilterChain에 기본적으로 등록되는 필터로 두 번째에 위치합니다.

서블릿단에서 비동기 작업을 수행할 때 서블릿 입출력 쓰레드와 작업 쓰레드가 동일한 SecurityContextHolder의 SecurityContext 영역을 찹조할 수 있도록 도와줍니다.

### SecurityContextHolderFilter

DefaultSecurityFilterChain에 기본적으로 등록되는 필터로 세 번째에 위치합니다.

이전 요청을 통해 이미 인증한 사용자 정보를 현재 요청의 SecurityContextholder의 SecurityContext에 할당하는 역할을 수행하고 현재 요청이 끝나면 SecurityContext를 초기화합니다.

<img src="https://github.com/user-attachments/assets/6184e1b7-d155-440c-a607-7f406b5873c1" width="50%"/>

```java
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {

    http
            .securityContext((context) -> context.disable()); // 사용 안 하기 설정

    return http.build();
}
```

> 웬만하면 disable 하지 않는 것이 좋습니다. 세션방식에서는 가장 중요하게 사용되고, jwt 방식에서도 마지막에 SecurityContext를 초기화해주므로 disable 하면 로직이 꼬일 수도 있기 때문입니다.
>

### HeaderWriterFilter

DefaultSecurityFilterChain에 기본적으로 등록되는 필터로 네 번째에 위치합니다.

HTTP 응답 헤더에 사용자 보호를 위한 시큐리티 관련 헤더를 추가하는 것이 목적입니다.

```java
http
        .headers((headers) -> headers
                        .frameOptions(frameOptions -> frameOptions.sameOrigin())
                        .cacheControl(cache -> cache.disable())
                        .contentTypeOptions(contentTypeOptions -> contentTypeOptions.disable())
        );
```

**HeaderWriterFilter로 디폴트로 추가되는 헤더**

<img src="https://github.com/user-attachments/assets/81ec906e-efc9-4347-b200-6fc54b671098" width="50%"/>

### CorsFilter

> 프론트와 백엔드의 Origin이 다르면 CORS 문제가 발생한다.
>

DefaultSecurityFilterChain에 기본적으로 등록되는 필터로 다섯 번째에 위치합니다.

CorsConfigurationSource에 설정한 값에 따라 필터단에서 응답 헤더를 설정하는 것이 목적인 필터입니다.

커스텀 SecurityFilterChain을 생성해도 등록되며 여러 구문을 통해 비활성화 시킬 수 있습니다.

```java
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
		
		http
								.cors(corsCustomizer -> corsCustomizer.configurationSource(new CorsConfigurationSource() {

                    @Override
                    public CorsConfiguration getCorsConfiguration(HttpServletRequest request) {

                        CorsConfiguration configuration = new CorsConfiguration();

                        configuration.setAllowedOrigins(Collections.singletonList("http://localhost:3000"));
                        configuration.setAllowedMethods(Collections.singletonList("*"));
                        configuration.setAllowCredentials(true);
                        configuration.setAllowedHeaders(Collections.singletonList("*"));
                        configuration.setMaxAge(3600L);

                        configuration.setExposedHeaders(Collections.singletonList("Set-Cookie"));
                        configuration.setExposedHeaders(Collections.singletonList("Authorization"));

                        return configuration;
                    }
                }));

    return http.build();
}
```

### CsrfFilter

> CSRF 공격: 사용자의 의지와 무관하게 해커가 강제로 사용자의 브라우저를 통해 서버측으로 특정한 요청을 보내도록 공격하는 방법입니다. 세션 방식을 사용할 때 다른 인증 절차 없이 세션ID만 쿠키로 요청이 보내지면 서버는 응답합니다. 이것을 공격하는 방법입니다.
>

DefaultSecurityFilterChain에 기본적으로 등록되는 필터로 여섯 번째에 위치합니다.

CSRF 공격 방어를 위해 HTTP 메소드 중 GET, HEAD, TRACE, OPTIONS 메소드를 제외한 요청에 대해서 검증을 진행하는 것이 목적인 필터입니다.

스프링 시큐리티의 CSRF 검증 방식은 토큰 방식이며 요청 시 토큰을 서버 저장소에 저장 후 클라이언트에게도 전송합니다. 그 후 해당하는 요청에 대해서 서버에 저장된 토큰과 비교 검증을 진행합니다.

**기본 동작은 SSR 세션 방식으로 설정되어 있습니다**. 따라서 RestAPI에서는 사용할 일이 거의 없기 때문에 보통 disable 시킵니다.

```java
http
        .csrf((csrf) -> csrf.disable());
```

만약 Controller 단에서 view단 응답 시 HTML form 영역에 서버에 저장되어 있는 _csrf 토큰 값을 넣어주면 됩니다.

### LogoutFilter

DefaultSecurityFilterChain에 기본적으로 등록되는 필터로 일곱 번째에 위치합니다.

인증 후 생성되는 사용자 식별 정보에 대해 로그아웃 핸들러를 돌며 로그아웃을 수행하는 것이 목적인 필터입니다. 기본적으로 세션 방식에 대한 로그아웃 설정이 되어 있기 때문에 JWT 방식이나 추가할 로직이 많으면 커스텀해서 사용해야 합니다.

커스텀 해서 만든 핸들러는 아래 방법으로 등록합니다.

```java
CookieClearingLogoutHandler cookies = new CookieClearingLogoutHandler("our-custom-cookie");
http
        .logout((logout) -> logout.addLogoutHandler(cookies));
```

### UsernamePasswordAuthenticationFilter

> username, password 기반의 로그인을 수행하는 필터, 이 필터에서 jwt 로그인 등 여러가지 로그인 방식이 구현됩니다.
>

DefaultSecurityFilterChain에 기본적으로 등록되는 필터로 여덟 번째에 위치합니다.

`POST /login` 경로에서 form 기반 인증을 진행할 수 있도록 multipart/form-data 형태의 username/password 데이터를 받아 인증 클래스에게 값을 넘겨주는 역할을 수행하는 필터입니다.

커스텀 SecurityFilterChain을 생성하면 자동 등록이 되지 않기 때문에 아래 구문을 통해서 필터를 활성화 시켜야 합니다.

```java
http
        .formLogin(Customizer.withDefaults());
```

인증 과정은 다음과 같습니다.

1. 사용자에게 데이터를 받아 인증
2. 인증 결과가 존재하면 세션 전략에 따라 SecurityContext에 저장
3. 로그인 성공/실패 핸들러 실행

form 형태가 아닌 JSON 형태의 데이터라도 위의 과정은 같습니다.

### DefaultLoginPageGeneratingFilter

이 필터는 DefaultSecurityFilterChain에 기본적으로 등록되는 필터로 아홉 번째에 위치합니다.

로그인 설정에 대해 `GET  /login` 경로에 기본 로그인 페이지를 응답하는 역할을 수행하는 것이 목적인 필터입니다.

이 필터는 여러 로그인 설정에 의존되며 가장 많이 사용하는 formLogin에서는 커스텀 SecurityFilterChain 등록시 아래와 같은 설정을 통해 사용할 수 있으며, 커스텀 로그인 페이지를 사용할 경우 제외됩니다.

```java
// 기본 사용
http
        .formLogin(Customizer.withDefaults());
        
// 커스텀 하더라도 아래와 같이 loginPage() 메소드를 다루지 않으면 기본 로그인 페이지 활성
http
        .formLogin((login) -> login.loginPage("/커스텀경로"));
```

### DefaultLogoutPageGeneratingFilter

이 필터는 DefaultSecurityFilterChain에 기본적으로 등록되는 필터로 열 번째에 위치합니다.

`GET /logout` 경로에 대해 기본 로그아웃 페이지를 응답하는 역할을 수행하는 것이 목적인 필터입니다.

이 필터는 여러 로그인 설정에 의존되며 가장 많이 사용하는 formLogin에서는 커스텀 SecurityFilterChain 등록시 아래와 같은 설정을 통해 사용할 수 있습니다.

```java
// 기본 사용
http
        .formLogin(Customizer.withDefaults());
```

### BasicAuthenticationFilter

이 필터는 DefaultSecurityFilterChain에 기본적으로 등록되는 필터로 열한 번째에 위치합니다.

Basic 기반의 인증을 수행하는 것이 목적인 필터입니다.

커스텀 SecurityFilterChain을 생성하면 자동 등록이 안되기 때문에 아래 구문을 통해서 필터를 활성화시켜야 합니다.

```java
http
        .httpBasic(Customizer.withDefaults());
```

> Basic 기반의 인증 : form 인증 방식과 달리 브라우저에서 제공하는 입력기에 username/password를 입력하면 브라우저가 매 요청 시 BASE64로 인코딩하여 Authorization 헤더에 넣어서 전송하는 방식입니다.
>

### RequestCacheAwareFilter

이 필터는 DefaultSecurityFilterChain에 기본적으로 등록되는 필터로 열두 번째에 위치합니다.

이전 HTTP 요청에서 처리할 작업이 있고 현재 요청에서 그 작업을 수행하기 위해 등록됩니다.

커스텀 SecurityFilterChain에도 기본적으로 등록되기 때문에 아래와 같은 구문을 통해 비활성화 시킬 수 있습니다.

```java
http
        .requestCache((cache) -> cache.disable());
```

예를 들어, 다음과 같은 상황이 발생할 수 있습니다.

1. 로그인하지 않은 사용자가 권한이 필요한 `/my` 경로에 접근합니다.
2. 권한 없음 예외가 발생하고 핸들러에서 `/my` 경로를 기억 후 핸들 작업을 수행합니다.
3. 스프링 시큐리티가 `/login` 창을 띄웁니다.
4. username/password 입력 후 인증을 진행합니다.
5. 로그인 이후 저장(캐싱)되어 있는 `/my` 경로를 불러내서 실행합니다.

### SecurityContextHolderAwareRequestFilter

이 필터는 DefaultSecurityFilterChain에 기본적으로 등록되는 필터로 열세 번째에 위치합니다.

ServletRequest 요청에 스프링 시큐리티 API를 다룰 수 있는 메소드를 추가하기 위해 사용하는 필터입니다.

커스텀 SecurityFilterChain에도 기본적으로 등록된다.

**추가되는 스프링 시큐리티 API 메소드**

- authenticate() : 사용자가 인증 여부를 확인하는 메소드
- login() : 사용자가 AuthenticationManager를 활용하여 인증을 진행하는 메소드
- logout() : 사용자가 로그아웃 핸들러를 호출할 수 있는 메소드
- AsyncContext.start() : Callable를 사용하여 비동기 처리를 진행할 때 SecurityContext를 복사하도록 설정하는 메소드

### AnonymousAuthenticationFilter

이 필터는 DefaultSecurityFilterChain에 기본적으로 등록되는 필터로 열네 번째에 위치합니다.

여러 필터를 거치면서 현재 지점 까지 SecurityContext값이 null 인 경우 Anonymous 값을 넣어주기 위해 사용되는 필터입니다.

커스텀 SecurityFilterChain에도 기본적으로 등록됩니다.

### ExceptionTranslationFilter

이 필터는 DefaultSecurityFilterChain에 기본적으로 등록되는 필터로 열다섯 번째에 위치합니다.

이 필터 `이후`에 발생하는 인증, 인가 예외를 핸들링하기 위해 사용됩니다.

커스텀 SecurityFilterChain에도 기본적으로 등록됩니다.

### AuthorizationFilter

이 필터는 DefaultSecurityFilterChain에 기본적으로 등록되는 필터로 마지막에 위치합니다.

SecurityFilterChain의 authorizeHttpRequests()를 통해 인가 작업을 진행한 값에 따라 최종적으로 인가를 수행합니다.

커스텀 SecurityFilterChain에도 기본적으로 등록되며 인가를 설정하는 방법은 아래와 같습니다.

```java
http
        .authorizeHttpRequests((auth) -> auth
                .requestMatchers("/").permitAll()
                .anyRequest().permitAll());
```
