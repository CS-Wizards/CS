## Process Address Space

### Process Address Space

- 운영체제가 자원을 할당하는 단위
- 공간을 분할하여 데이터를 최대한 공유해 메모리 중복을 피하기 위한 목적

![https://velog.velcdn.com/images/klm03025/post/cd3ef853-de73-4a07-b062-263ff9d1acdc/image.png](https://velog.velcdn.com/images/klm03025/post/cd3ef853-de73-4a07-b062-263ff9d1acdc/image.png)

- Stack
    - 함수의 호출과 관계되는 지역 변수와 매개변수가 저장되는 영역
    - Stack 영역의 값은 함수 호출과 함께 할당되면, 함수의 호출이 완료되면 소멸
    - 메모리의 높은 주소에서 낮은 주소의 방향으로 할당
    - 재귀 함수가 너무 깊게 호출되거나 함수가 지역 변수를 너무 많이 가지고 있어 stack 영역을 초과하면 stack overflow 에러가 발생
- Heap
    - 런타임에 크기가 결정되는 영역
    - 사용자에 의헤 공간이 동적으로 할당 및 해제
    - 주로 참조형 데이터 등의 데이터가 할당
    - 메모리의 낮은 주소에서 높은 주소의 방향으로 할당
- Data
    - 전역 변수나 static 변수 등 프로그램이 사용할 수 있는 데이터를 저장하는 영역
    - 어떤 프로그램에 전역/static 변수를 참조하는 코드가 존재한다면, 이 프로그램은 컴파일 된 후에 data 영역을 참조하게 됨
    - 프로그램의 시작과 함께 할당되며, 프로그램이 종료되면 소멸
    - 초기화되지 않은 변수가 존재한다면, BSS와 같은 영역에 저장
- Text (Code) 영역
    - 프로그램이 실행될 수 있도록 CPU가 해석 가능한 기계어 코드가 저장된 공간
    - 프로그램이 수정되면 안 되기에 readOnly로 저장
- stack vs heap
    - 할당/해제 속도는 Stack이 빠름
    - 스택에서 할당의 의미는 이미 생성되어 있는 스택에 대해 포인터의 위치만 바꿔 주는 단순한 연산
    - 힙에서 할당은 요청의 크기, 현재 메모리의 fragmentation 상황 등 다양한 요소를 고려하기 때문에 더 많은 연산 필요

### Thread Address Space

- 프로세스가 자원을 할당받지만 스레드도 자신만의 자원이 필요함
- 스레드도 자신만의 주소 공간 가짐

![https://velog.velcdn.com/images/klm03025/post/ebe37998-118d-474d-b740-ef121ffa0f0e/image.png](https://velog.velcdn.com/images/klm03025/post/ebe37998-118d-474d-b740-ef121ffa0f0e/image.png)

- 각 스레드가 가지고 있는 건 stack 영역밖에 없음
- 나머지 공간은 프로세스의 값을 함께 쓰고 있고 다른 스레드와 공유

## Scheduler

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

### preemptive/non-preemptiv

- 선점형 스케줄링 기법
    - 하나의 프로세스가 CPU를 차지하고 있을 때, 우선순위가 높은 다른 프로세스가 현재 프로세스를 중단시키고 CPU를 점유하는 스케줄링 방식
    - 비교적 응답이 빠르다는 장점이 있지만, 처리 시간을 예측하기 힘들고 높은 우선순위 프로세스들이 계속 들어오는 경우 오버헤드 초래
    - 실시간 응답 환경,  Deadline 응답 환경 등 우선 순위가 높은 프로세스를 빠르게 처리해야 할 경우 등에 유용
    - 스케줄링 알고리즘: 라운드 로빈, 다단계 큐, 다단계 피드백 큐
- 비선점형 스케줄링 기법
    - 한 프로세스가 CPU를 할당받으면 작업 종료 후 CPU 반환 시까지 다른 프로세스는 PCU 점유가 불가능한 스케줄링 방식
    - 모든 프로세스에 대한 요구를 공정하게 처리할 수 있지만, 짧은 작업을 수행하는 프로세스가 긴 작업 종료 시까지 대기해야 할 수도 있음
    - 싱글 스레드 CPU 에서 상시로 돌아가야 하는 프로세스가 있을 때 사용하는 스케쥴링 알고리즘
    - 처리 시간 편차가 적은 특정 프로세스 환경에 용이
    - 전체 시스템 입장에서 낮은 처리율
    - 스케줄링 알고리즘: 우선순위, 기한부, SJF, HRN, FIFO
    
    ### Term Scheduler
    
    - Long-term Scheduler
        - 작업 스케줄러
        - 어떤 프로세스를 Ready Queue로 보낼지 결정하는 스케줄러
        - 메모리와 디스크 사이의 스케줄링 담당
        - 프로세스에 메모리 할당
        - 프로세스가 끝날 때 실행되므로 실행 주기가 매우 긺
        - 일괄처리 시스템 아니라면 사용하지 않음
    - Mediun-term Scheduler
        - Swapper
        - 메모리에 적재된 프로세스 수를 관리하는 스케줄러
        - 스와핑: 일부 프로세스를 메모리에서 디스크로 보내고 시간이 지나 메모리에 여유가 생기면 다시 적재
        - 우선 순위가 가장 낮은 프로세스나 일정 시간 동안 활성화되지 않은 프로세스들을 디스크로 내림
        - 거의 사용하지 않음
    - Short-term Scheduler
        - CPU Scheduler라고 불림
        - CPU는 하나의 작업만 처리하기 때문에 Ready Queue에서 CPU를 할당할 하나의 프로세스 선택
        - Ready 상태에 있는 작업 중에서 프로세스를 선택하여 CPU를 할당
        - CPU와 메모리 사이의 스케줄링 담당
        - CPU는 시간을 낭비하지 않기 위해 프로세스를 기다리는 동안 다른 프로세스를 바로 실행해야 하기 때문에 매우 짧은 주기로 수행
        - 여전히 잘 사용 중
    - 메모리가 부족할 경우의 프로세스
        - 대기 중인 프로세스가 할당된 메모리를 반납하고 하드디스크로 스와핑
        - 이후 메모리에 여유가 생기면 다시 메모리로 올라옴

## Context Switch

### Context Switch

![https://velog.velcdn.com/cloudflare/aeong98/f639db9c-41ee-445a-9009-618ea8312633/image.png](https://velog.velcdn.com/cloudflare/aeong98/f639db9c-41ee-445a-9009-618ea8312633/image.png)

- 하나의 프로세스가 이미 CPU를 사용 중인 상태에서 다른 프로세스가 CPU를 사용하기 위해 이전 프로세스의 상태를 저장하고 새로운 프로세스의 상태를 적재하는 것
- 운영체제는 타이머 인터럽트 처리 루틴으로 가서 직전까지 수행 중이던 프로세스 A의 문맥을 자신의 PCB에 저장하고 프로세스 B는 예전에 저장했던 자신의 문맥을 PCB로부터 실제 하드웨어로 복원시킴
- CPU가 동시에 여러 개의 프로세스를 실행시키는 것처럼 보이지만 재빠르게 여러 프로세스를 번갈아 가며 실행하고 관리하는 것
- 오버헤드: 문맥 교환에 필요한 시간과 메모리
- 시스템 콜이나 인터럽트가 발생하면 CPU의 제어권이 운영체제로 넘어와 프로그램을 멈추고 운영체제 코드 실행
    - 자바 프로그램을 실행 중 입력값을 받을 시기가 되면 자바 프로그램이 멈추고 운영체제가 인풋을 기다림
    
    ![https://velog.velcdn.com/cloudflare/aeong98/78c6f6b8-e78c-4e79-9bbd-deada5c7a5f5/image.png](https://velog.velcdn.com/cloudflare/aeong98/78c6f6b8-e78c-4e79-9bbd-deada5c7a5f5/image.png)
    

### 발생 원인

- Time Quato Expiry: 주어진 quantum 의 시간이 다 됨
- Interrupt: 커널 함수를 통해 프로그램 실행 도중에 멈춰 버림
- Preemption: 더 우선 순위가 높은 일을 해야 할 때 선점해 버려서

### Process VS Thread

- Thread Context Switching
    - 커널 모드 전환과 CPU Register 상태 교체가 이루어짐
    - 기본적인 작업만 하면 돼서 빠름
- Process Context Switching
    - 가상 메모리 주소 관련 처리를 추가적으로 수행
    - MMU (Memory Management Unit)이 새로운 프로세스의 주소 체계를 바라보도록 수정
    - 캐시 역할을 하는 TLB(Translation Lookaside Buffer)를 완전히 비워 줘야 함
    - 상대적으로 느린 속도

## Process Scheduling Algorithm

### Round Robin

- 특징
    - 각 프로세스는 동일한 할당 시간을 가짐
    - CPU를 할당받고 할당 시간이 지나면 Ready 상태로 돌아가 Ready Queue의 Tail로 들어감
    - 프로세스들이 작업을 완료할 때까지 계속해서 순회
- 장점
    - Response time이 빨라짐
    - n개의 프로세스가 ready queue에 있고 할당 시간이 q인 경우 프로세스는 q 단위로 CPU 시간의 1/n을 얻음
    - 즉 어떤 프로세스도 (n - 1) * q 이상을 기다리지 않음
    - 모든 프로세스가 공정하게 CPU를 할당받을 수 있음을 보장
- 주의점
    - q가 너무 커지면 실행 시간이 짧은 프로세스들이 실행 시간이 긴 프로세스를 계속 기다리면서 효율성이 저하됨
    - q가 너무 짧아지면 컨텍스트 스위칭이 자주 일어나 오버헤드 발생 가능성

### Multi-level Feedback Queue

- 특징
    - 각각 다른 우선 순위를 배정받은 여러 개의 큐로 구성
    - 큐에는 둘 이상의 작업이 존재할 수 있고, 한 큐 안에는 라운드 로빈 알고리즘이 사용됨
    - MLFQ는 각 작업에 고정된 우선 순위를 부여하는 것이 아니라 각 작업의 특성에 따라 동적으로 우선 순위 부여
        - 어떤 작업이 키보드 입력을 기다리며 반복적으로 양보했다면 더 높은 우선 순위 부여
        - 어떤 작업이 긴 시간 동안 점유했다면 낮은 우선 순위 부여
- MLFQ 규칙
    - Priority(A) > Priority(B) 이면, A 실행 (B는 실행 x)
    - Priority(A) = Priority(B) 이면, A와 B는 RR 방식으로 실행
        - 보통 응답시간 중요한 작업이 우선순위 높음
    - 작업이 시스템에 진입하면, 가장 높은 우선순위의 큐에 놓임
    - 주어진 단계에서 CPU 시간 할당량을 소진하면 우선순위는 낮아짐
    - 일정 기간 `S`가 지나면, 시스템의 모든 작업을 최상위 큐로 이동
- 장점
    - 짧은 작업을 먼저 실행시켜 반환시간 최적화
    - 대화형 사용자를 위해 응답 시간 최적화 → 내부에서 RR 알고리즘 사용
    - 프로세스에 대한 정보 없이 스케줄링 하는 방법

### Concurrency vs Parallelism

| 동시성 (Concurrency) | 병렬성 (Parallelism) |
| --- | --- |
| 동시에 실행되는 것처럼 보이는 것 | 실제로 동시에 실행되는 것 |
| 논리적인 개념 | 물리적인 개념 |
| 싱글코어, 멀티코어에서 가능 | 멀티코어에서만 가능 |

## Docker & Virtual Machine

### Reading Article

<aside>
<img src="https://raw.githubusercontent.com/eirikmadland/notion-icons/master/v5/icon3/ul-comment-message.svg" alt="https://raw.githubusercontent.com/eirikmadland/notion-icons/master/v5/icon3/ul-comment-message.svg" width="40px" /> Article

### What is a Container

[What is a Container? | Docker](https://www.docker.com/resources/what-container/)

</aside>

### Container

- 한 컴퓨터 환경으로부터 다른 컴퓨터 환경에서 빠르고 안정적으로 앱이 실행될 수 있도록 모든 코드와 그의 의존성을 묶은 소프트웨어의 표준화된 규격
- Docker의 container image는 앱을 실행하기 위한 코드, 런타임, 시스템 도구, 시스템 라이브러리나 세팅 등이 포함된 가볍고 안정적이고 실행할 수 있는 소프트웨어
- Container image는 런타임에 containers가 되고, 특히 Docker의 경우 Docker Engine에서 돌아갈 때 image가 container가 됨
- containerized된 소프트웨어는 infrastructure과 무관하게 항상 같은 환경에서 실행될 수 있음
- Container는 소프트웨어를 그것의 환경과 분리시키기 때문에, 소프트웨어가 다양한 컴퓨터 위에서도 uniformly하게 실행될 수 있도록 함
- 기본적으로 각 APP들은 Cgroup(control groups)과 네임스페이스와 독립된 네트워크로 인해 “Wall”이 존재
    - 도커 데몬은 이렇게 각각의 컨테이너가 정해진 논리 공간에 쉽게 배정될 수 있게 도와주는 역할

### VS Virtual Machine
![](https://velog.velcdn.com/images/morion002/post/d38ab3f1-51e3-48fd-a0ac-ed352f9beeda/image.png)


- Container과 Virtual Machine은 자원을 격리시키고 할당하는 장점을 동일하게 가지고 있음
- 하지만 hardware을 가상화하는 virtual machine과 다르게 container는 oprating system을 가상화 → 더 효율적이고 휴대 간편
- Containers
    - Container 여려 개는 각각의 container에서 os kernal을 나눠 가지며 한 컴퓨터 안에서 동시에 실행
    - 각각의 container는 user space에서 격리된 프로세스로 실행
    - container는 VMs 보다 가볍기 때문에 더 많은 앱을 다룰 수 있음
- virtual machine
    - VMs는 하나의 서버에서 여러 서버로 전환하는 물리적인 하드웨어의 추상화
    - 여러 VMs가 하나의 machine 위에서 실행
    - 각각의 VM은 모든 구성 요소를 갖춘 operating system, 앱, 그리고 꼭 필요한 라이브러리들을 복사해 두기 때문에 부팅이 느림
    
    ![](https://velog.velcdn.com/images/morion002/post/23df9728-0891-4898-9dfa-d788cc055373/image.png)

    

### Docker in Docker

- 도커 인스턴스 두 개가 서로 완전히 독립되어 있는 상태에서 이미지를 끌어당기고 구축하거나 다른 컨테이너를 실행할 수 있는지에 대한 문제
- 할 수는 있지만 low level의 기술적 문제를 많이 일으키기 때문에 권장되지는 않음
- 도커가 설치된 컨테이너는 자체적으로 도커 데몬을 실행하지 않고, 호스트 시스템의 도커 데몬에 연결
- 컨테이너와 호스트 시스템에 Docker CLI가 있지만 둘 다 동일한 도커 데몬에 연결됨
- CICD를 위해 종종 도커 데몬을 실행해야 하는 경우가 생김
- Docker in docker (dind) 명령어를 통해 실행 가능
    
    `docker run --privileged -d docker:dind`
