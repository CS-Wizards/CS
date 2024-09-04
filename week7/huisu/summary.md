## Cookie vs Session

### 쿠키와 세션

- 웹 통신간 유지하려는 정보를 저장하기 위해 사용하는 것
- 쿠키는 개인 PC에 저장
- 세션은 접속 중인 웹서버에 저장

![](https://velog.velcdn.com/images/morion002/post/53928c0d-46ef-41b3-bbdd-6d8c2a8391f7/image.png)


- 규모가 커져 세션 관리가 어려울 때 세션을 외부의 DBMS로 뺴기도 한다 (ex Redis)

### HTTP

- HyperText Transfer Protocol의 약자로 주로 HTML과 같은 HyperText 문서를 주고받기 위해 만들어짐
- 비연결성과 무상태성의 특성
- 비연결성
    - 처음 연결을 맺은 후 요청과 한 번의 응답 이후 연결 종료
    - 매 요청마다 다시 연결
- 무상태성
    - 프로토콜에서 Client의 상태를 기억하지 않음
    - Client의 상태를 보관하기 위해 쿠키나 세션, JWT 토큰 등을 이용

## HTTP Method

### Response Code

1. **200번 대** : 문제 없음
    1. **200** : OK 성공임
    2. **201** : Created: 네가 준 데이터를 가지고 적절한 과정을 거쳐 새로운 리소스를 만듦
2. **400번 대** : 클라이언트 측 잘못으로 인한 에러
    1. **400** : Bad Request : 요청 이상하게 함, 필요한 정보 누락됨
    2. **401** : Unauthorized : 인증이 안됨 (로그인이 되어야 하는데 안 된 상황)
    3. **403** : Forbidden : 권한 없음 (로그인은 되었으나 접근이 안됨, 관리자 페이지 등등)
    4. **404** : NotFound :  요청한 정보가 그냥 없음
3. **500번 대** : 서버 측 잘못으로 인한 에러
    1. **500** : Internal Server Error : 서버 터짐
    2. **504** : Gateway Timeout : 서버가 응답을 안 줌 
4. Custom
    1. 직접 응답 코드를 만들어서 서비스에 맞게 활용 가능

### Method

1. GET : 조회
2. POST : 생성
3. PUT : 갱신(전체)
4. PATCH : 갱신(일부)
5. DELETE : 삭제
- 위의 5개 메소드 중 **POST**는 **새로운 자원의 생성**도 있지만, 클라이언트가 **특정 정보를 서버로 넘기고 그에 대한 처리를 요청 하는 것**을 전부 POST로 처리 가능
- GET은 정보를 url에 담아 보내고 POST는 정보를 body에 담아 보냄
    - 현재는 GET에 body를 담을 수 있지만 일부 클라이언트에서는 지원하지 않음 (Spring RestTemplate)

### Idempotent

- 동일한 요청을 한 번 보내는 것과 여러 번 보내는 것이 같은 효과를 지니고 서버도 동일하게 유지되는 경우
- 멱등성을 가지는 메서드: GET, PUT, DELETE
    - GET: 읽어만 오는 거라서 값이 동일하고 서버도 변동 없음
    - PUT: 수정 이후 계속 수정해 봐도 서버 상태 변동 없음
    - DELETE
        - 첫 요청: 200 삭제 성공
        - 두 번째 이후 요청: 404 이미 삭제된 거라 찾을 수 없음
        - 하지만 두 요청 모두 내가 원하는 데이터는 서버에서 삭제된 상태고 서버의 상태 이동 없음
- 멱등성을 가지지 않는 메서드: POST, PATCH
    - POST: 요청을 보낼 때마다 데이터가 하나씩 생성됨
    - PATCH: POST와 동일하게 멱등성을 가지지 않지만 PUT처럼 쓰면 멱등성을 가짐

## Process

### Process

- 프로세스는 메모리에 올라와 실행되고 있는 프로그램의 인스턴스
- 스케줄링의 대상이 되는 작업과 같은 의미로 사용
- 프로세스 내부에는 최소 하나의 스레드를 가지고 있는데, 실제로는 스레드 단위로 스케줄링 진행
- 하드디스크에 있는 프로그램을 실행하면, 실행을 위해 메모리 할당이 이루어지고, 할당된 메모리 공간으로 바이너리 코드를 올리는 순간 프로세스라 명명
- 프로세스의 Context
    - CPU 수행 상태를 나타내는 하드웨어 문맥
    - 프로세스의 메모리
        - Code 영역: 실행할 프로그램의 코드나 명령어들이 기계어로 저장
        - Data 영역: 코드에서 선언한 전역 변수와 정적 변수 저장
        - Stack: 함수 안에서 선언된 지역 변수, 매개 변수, 리턴값 저장
        - Heap 영역: 관리가 가능한 데이터 이외의 다른 형태의 데이터 관리하기 위한 자유 공간
    - 프로세스 관련 커널 자료 구조
        - PCB(Process Control Block)

### Pcoress State

![https://velog.velcdn.com/cloudflare/aeong98/f79ef46b-d5ac-46c9-ac39-732e6f5cf470/image.png](https://velog.velcdn.com/cloudflare/aeong98/f79ef46b-d5ac-46c9-ac39-732e6f5cf470/image.png)

- Running: CPU 점유 후 실행 중
- Ready: CPU 점유 대기 중
- Blocked: CPU를 주어도 수행할 수 없는 상태 (I/O 요청 기다림)
- New: 디스크에서 메모리로 프로그램이 올라가 실행 준비를 하는 상태
- Terminated: 수행이 끝난 상태

### PCB(Process Control Block)

- 운영체제가 프로세스를 표현한 자료 구조
- 특정 프로세스에 대한 정보를 가지고 있음
- 각 프로세스가 생성될 때마다 고유의 PCB가 생성되고 프로세스가 완료되면 제거
- OS가 관리에서 사용하는 정보
    - Process State, Process ID
    - Scheduling information, Priorioty
- CPU 수행 관련 하드웨어 값
    - Program counter, registers
- 메모리 관련
    - Code, Data, Stack, Heap…
- 파일 관련
    - open file descriptor

### Interupt

- 하드웨어 인터럽트: 하드웨어가 발생시키는 인터럽트 (I/O 입력)
    - Polling 방식으로 인터럽트를 보내지 않고 CPU가 직접 입출력을 관리할 수도 있음
    - 하지만 Polling 방식은 입출력이 발생하는 동안 CPU가 묶여 있다는 단점 발생
- 소프트웨어 인터럽트: 소프트웨어가 발생시키는 인터럽트 (예외 상황, System call)

### Context Switch

![https://velog.velcdn.com/cloudflare/aeong98/f639db9c-41ee-445a-9009-618ea8312633/image.png](https://velog.velcdn.com/cloudflare/aeong98/f639db9c-41ee-445a-9009-618ea8312633/image.png)

- 하나의 프로세스가 이미 CPU를 사용 중인 상태에서 다른 프로세스가 CPU를 사용하기 위해 이전 프로세스의 상태를 저장하고 새로운 프로세스의 상태를 적재하는 것
- 운영체제는 타이머 인터럽트 처리 루틴으로 가서 직전까지 수행 중이던 프로세스 A의 문맥을 자신의 PCB에 저장하고 프로세스 B는 예전에 저장했던 자신의 문맥을 PCB로부터 실제 하드웨어로 복원시킴
- CPU가 동시에 여러 개의 프로세스를 실행시키는 것처럼 보이지만 재빠르게 여러 프로세스를 번갈아 가며 실행하고 관리하는 것
- 오버헤드: 문맥 교환에 필요한 시간과 메모리
- 시스템 콜이나 인터럽트가 발생하면 CPU의 제어권이 운영체제로 넘어와 프로그램을 멈추고 운영체제 코드 실행
    - 자바 프로그램을 실행 중 입력값을 받을 시기가 되면 자바 프로그램이 멈추고 운영체제가 인풋을 기다림
    
    ![https://velog.velcdn.com/cloudflare/aeong98/78c6f6b8-e78c-4e79-9bbd-deada5c7a5f5/image.png](https://velog.velcdn.com/cloudflare/aeong98/78c6f6b8-e78c-4e79-9bbd-deada5c7a5f5/image.png)
    

### Thread

![https://velog.velcdn.com/cloudflare/aeong98/411e94f9-758b-40e3-93cb-938cde5fbd15/image.png](https://velog.velcdn.com/cloudflare/aeong98/411e94f9-758b-40e3-93cb-938cde5fbd15/image.png)

- 스레드는 프로세스와 다르게 스레드 간 메모리를 공유하며 작동
- 프로세스가 할당받은 자원을 활용하는 실행 흐름의 단위
- 하나의 프로세스는 하나 이상의 스레드를 가지고 있음
- 스레드가 독립적으로 가지고 있는 부분
    - Program Counter (실행 흐름)
    - Register Set
    - Stack space
- 스레드가 동료 스레드와 공유하는 부분
    - Code Section
    - Data Section
    - OS Resources

### 프로세스와 스레드의 차이점

![https://velog.velcdn.com/cloudflare/aeong98/2ab2b490-e027-4b03-9a54-8104199b6c60/image.png](https://velog.velcdn.com/cloudflare/aeong98/2ab2b490-e027-4b03-9a54-8104199b6c60/image.png)

- 프로세스는 다른 프로세스의 변수나 자료에 접근할 수 없음

![https://velog.velcdn.com/cloudflare/aeong98/ce1b2913-bef7-46ff-9444-00dbd22489ef/image.png](https://velog.velcdn.com/cloudflare/aeong98/ce1b2913-bef7-46ff-9444-00dbd22489ef/image.png)

- 한 프로세스 안에 있는 스레드들은 Code, Data, Heap 영역을 공유하여 사용 가능함

### 데몬 프로세스

- 운영체제에서 부팅 시 자동으로 켜져, 백그라운드에서 계속 실행되는 프로세스
- 백그라운드에서 계속 실행되고 있다는 것은 요청이 오면 즉시 대응할 수 있도록 대기 중
- 프로세스의 종류 중 하나
- `inetd`, `httpd`, `nfsd`, `sshd`, `lpd`, `systemd`, `ftpd`
- 가장 처음 부팅하자마자 시작하는 루트 프로세스이자 데몬 프로세스 `init`

### 고아 프로세스

- 부모 프로세스가 자식 프로세스보다 먼저 종료되는 경우 부모 프로세스가 없는 자식 프로세스
- 운영체제는 이런 고아 프로세스를 허용하지 않으며 고아 프로세스의 부모 프로세스를 `init`으로 설정해 버림
