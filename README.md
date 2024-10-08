# 🍇 CS-STUDY

❕Chat GPT가 추천한 이름이니 너무 뭐라고 하지 말 것

## 🥦 스터디 진행 방식

**WEEK STUDY**

- 주차마다 모든 팀원들이 주제에 해당하는 부분에 대해 요약.md를 작성해 pr을 보냅니다.
- 돌아가면서 간단하게 발표와 질의응답 시간을 가집니다.
- 익명 투표로 베스트 워크북을 선정합니다.
- 베스트 워크북으로 선정된 팀원의 md는 메인에 링크됩니다.

**SPECIAL STUDY**

- 주차마다 팀원들끼리 돌아가면서 당번을 정합니다.
- 당번은 평소 주차 스터디 주제보다는 심화된 주제에 대해 딥다이브 아티클을 발표합니다.

## 🍑스터디 규칙

- 주마다 하루씩 만나 대면으로 스터디합니다.
- 부득이한 사유가 있다면 1회 불참이 가능합니다. 다만 SPECIAL SRUDY 당번인 경우에는 불참이 불가능합니다.
- 지각이나 결석이 있을 경우 3000원의 벌금이 있습니다.
- 스터디 직전까지 PR을 작성해야 합니다.
- 스터디에 불참하더라도 스터디룸 대여비는 모두 분할하여 정산합니다.

## 🍌 스터디 목차

| 주차 | 날짜 | 주제 | 당번 |
| --- | --- | --- | --- |
| 1 | 7/24 | Java & Spring | [조희수](https://github.com/ranunclulus) |
| 2 | 7/31 | Java & Spring | [정혜선](https://github.com/chunghye98) |
| 3 | 8/7 | Network | 김성일 |
| 4 | 8/14 | Network | [문인규](https://github.com/InGyu-Moon) |
| 5 | 8/21 | Network, Database | 진성일 |
| 6 | 8/28 | Network, Operating System | [황인준](https://github.com/InJun2) |
| 7 | 9/4 | Operating System | [조희수](https://github.com/ranunclulus) |
| 8 | 9/11 | Operating System | [정혜선](https://github.com/chunghye98) |

- 1주차
    
    **JVM**
    
    - 그럼, 자바 말고 다른 언어는 JVM 위에 올릴 수 없나요?
    - 반대로 JVM 계열 언어를 일반적으로 컴파일해서 사용할 순 없나요?
    - VM을 사용함으로써 얻을 수 있는 장점과 단점에 대해 설명해 주세요.
    - JVM과 내부에서 실행되고 있는 프로그램은 부모 프로세스 - 자식 프로세스 관계를 갖고 있다고 봐도 무방한가요?
    
    **final 키워드**
    
    - 그렇다면 컴파일 과정에서, final 키워드는 다르게 취급되나요?
    
    **인터페이스와 추상 클래스의 차이**
    
    - 왜 클래스는 단일 상속만 가능한데, 인터페이스는 2개 이상 구현이 가능할까요?
    
    **리플렉션**
    
    - 의미만 들어보면 리플렉션은 보안적인 문제가 있을 가능성이 있어보이는데, 실제로 그렇게 생각하시나요? 만약 그렇다면, 어떻게 방지할 수 있을까요?
    - 리플렉션을 언제 활용할 수 있을까요?
    
    **static class와 static method**
    
    - static 을 사용하면 어떤 이점을 얻을 수 있나요? 어떤 제약이 걸릴까요?
    - 컴파일 과정에서 static 이 어떻게 처리되는지 설명해 주세요.
    
    **Java의 Exception**
    
    - 예외처리를 하는 세 방법에 대해 설명해 주세요.
    - CheckedException, UncheckedException 의 차이에 대해 설명해 주세요.
    - 예외처리가 성능에 큰 영향을 미치나요? 만약 그렇다면, 어떻게 하면 부하를 줄일 수 있을까요?
    
    **Synchronized**
    
    - Synchronized 키워드가 어디에 붙는지에 따라 의미가 약간씩 변화하는데, 각각 어떤 의미를 갖게 되는지 설명해 주세요.
    - 효율적인 코드 작성 측면에서, Synchronized는 좋은 키워드일까요?
    - Synchronized 를 대체할 수 있는 자바의 다른 동기화 기법에 대해 설명해 주세요.
    - Thread Local에 대해 설명해 주세요.
    
    **Java Stream**
    
    - Stream과 for ~ loop의 성능 차이를 비교해 주세요,
    - Stream은 병렬처리 할 수 있나요?
    - Stream에서 사용할 수 있는 함수형 인터페이스에 대해 설명해 주세요.
    - 가끔 외부 변수를 사용할 때, final 키워드를 붙여서 사용하는데 왜 그럴까요? 꼭 그래야 할까요?
- 2주차
    
    **Java의 GC**
    
    - finalize() 를 수동으로 호출하는 것은 왜 문제가 될 수 있을까요?
    - 어떤 변수의 값이 null이 되었다면, 이 값은 GC가 될 가능성이 있을까요?
    
    **equals()와 hashcode()**
    
    - 본인이 hashcode() 를 정의해야 한다면, 어떤 점을 염두에 두고 구현할 것 같으세요?
    - 그렇다면 equals() 를 재정의 해야 할 때, 어떤 점을 염두에 두어야 하는지 설명해 주세요.
    
    **IoC와 DI**
    
    - 후보 없이 특정 기능을 하는 클래스가 딱 한 개하면, 구체 클래스를 그냥 사용해도 되지 않나요? 그럼에도 불구하고 왜 Spring에선 Bean을 사용 할까요?
    - Spring의 Bean 생성 주기에 대해 설명해 주세요.
    - 프로토타입 빈은 무엇인가요?
    
    **DispatcherServlet**
    
    - 여러 요청이 들어온다고 가정할 때, DispatcherServlet은 한번에 여러 요청을 모두 받을 수 있나요?
    - 수많은 @Controller 를 DispatcherServlet은 어떻게 구분 할까요?
    
    **JPA와 같은 ORM을 사용하는 이유**
    
    - 영속성은 어떤 기능을 하나요? 이게 진짜 성능 향상에 큰 도움이 되나요?
    - N + 1 문제에 대해 설명해 주세요.
    
    **@Transactional** 
    
    - @Transactional(readonly=true) 는 어떤 기능인가요? 이게 도움이 되나요?
    - 그런데, 읽기에 트랜잭션을 걸 필요가 있나요? @Transactional을 안 붙이면 되는거 아닐까요?
- 3주차
  
    **AOP**
    
    - @Aspect는 어떻게 동작하나요?
    
    **Java 에서 Annotation** 
    
    - 별 기능이 없는 것 같은데, 어떻게 Spring 에서는 Annotation 이 그렇게 많은 기능을 하는 걸까요?
    - Lombok의 @Data를 잘 사용하지 않는 이유는 무엇일까요?
    
    **Tomcat**
    
    - 혹시 Netty에 대해 들어보셨나요? 왜 이런 것을 사용할까요?

    
    **HTTP에 대해 설명해 주세요.**
    
    - 공개키와 대칭키
    - HTTPS Handshake 과정에서는 인증서 사용 이유
    - SSL과 TLS의 차이
    
    **웹소켓과 소켓 통신의 차이**
    
    - 소켓과 포트의 차이
    - 여러 소켓이 있다고 할 때, 그 소켓의 포트 번호는 모두 다른가
    - 사용자의 요청이 무수히 많아지면, 소켓도 무수히 생성되는지
    
    **TCP와 UDP**
    
    - Checksum
    - TCP와 UDP 중 Checksum을 수행하는 프로토콜
    - Checksum을 통해 오류를 정정 가능 유무
    - TCP가 신뢰성을 보장하는 방법
    - TCP의 혼잡 제어 처리 방법
    - HTTP는 TCP를 사용하는 이유
    - 브라우저가 서버가 TCP를 쓰는지 UDP를 쓰는지 어떻게 알 수 있는 방법
    - 본인이 새로운 통신 프로토콜을 TCP나 UDP를 사용해서 구현한다고 하면, 프로토콜 선택 기준
- 4주차
    
    **IP 주소는 무엇이며, 어떤 기능을 하고 있나요?**
    
    - IPv4와 IPv6의 차이
    - 수많은 공유기에서는 고정 주소를 제공하는 기능의 동작 방법
    - IPv4를 사용하는 장비와 IPv6를 사용하는 같은 네트워크 내에서 통신하는 방법
    - IP가 송신자와 수신자를 정확하게 전송되는 것의 보장 유무
    - IPv4에서 수행하는 Checksum과 TCP에서 수행하는 Checksum의 차이
    - TTL(Hop Limit)
    - IP 주소와 MAC 주소의 차이

    
    **OSI 7계층**
    
    - Transport Layer와, Network Layer의 차이
    - L3 Switch와 Router의 차이
    - Layer는 패킷 명칭
    - 각각의 Header의 Packing Order
    - ARP
    
    **3-Way Handshake**
    
    - ACK, SYN 같은 정보 전달 방식
    - 2-Way Handshaking 를 하지 않는 이유
    - 두 호스트가 동시에 연결을 시도했을 때 통신 연결 수행 방법
    - SYN Flooding 에 대해 설명
    
    [**www.github.com을](http://www.github.xn--com-of0o/) 브라우저에 입력하고 엔터를 쳤을 때, 네트워크 상 어떤 일이 일어나는지** 
    
    - DNS 쿼리를 통해 얻어진 IP가 가리키는 곳
    - Web Server와 Web Application Server의 차이
    - URL, URI, URN의 차이
    
    **DNS**
    
    - DNS는 몇 계층 프로토콜
    - UDP와 TCP 중 사용하는 프로토콜
    - DNS Recursive Query, Iterative Query
    - DNS 쿼리 과정에서 손실이 발생할 경우 처리 방법
    - 캐싱된 DNS 쿼리가 잘못 될 경우 에러 보장 방법
    - DNS 레코드 타입 중 A, CNAME, AAAA의 차이
    - hosts 파일의 역할과  DNS와 비교하였을 때 우선순위
    
    **Stateless와 Connectionless**
    
    - HTTP는 Stateless 구조를 채택 이유
    - Connectionless의 성능 해결 방법
    - TCP의 keep-alive와 HTTP의 keep-alive의 차이
      
- 5주차
  
    **라우터 내의 포워딩 과정**
    
    - 라우팅과 포워딩의 차이
    - 라우팅 알고리즘
    - 포워딩 테이블의 구조

    
    **로드밸런서**
    
    - L4 로드밸런서와, L7 로드밸런서의 차이
    - 로드밸런서 알고리즘
    - 로드밸런싱 대상이 되는 장치중 일부 장치가 문제가 생겨 접속이 불가능할 경우, 로드밸런서가 해당 장비로 요청을 보내지 않도록 하는 방법
    - 로드밸런서 장치를 사용하지 않고, DNS를 활용해서 유사하게 로드밸런싱을 하는 방법
    
    **서브넷 마스크와 게이트웨이**
    
    - NAT
    - 서브넷 마스크의 표현 방식
    
    **트랜잭션과 ACID 원칙**
    
    - ACID 원칙 중, Durability를 DBMS가 보장하는 방법
    - 읽기에는 트랜잭션을 걸지 않아도 되는지
    
    **트랜잭션 격리 레벨**
    
    - 모든 DBMS가 4개의 레벨을 모두 구현하는지
    - 만약 MySQL을 사용하고 있다면, (InnoDB 기준) Undo 영역과 Redo 영역
    - 스토리지 엔진
- 6주차
  
    **인덱스**
    
    - 일반적으로 인덱스는 수정이 잦은 테이블에선 사용하지 않기를 권하는 이유
    - ORDER BY/GROUP BY 연산의 동작 과정을 인덱스의 존재 여부와 연관지어서 설명
    - 기본키는 인덱스라고 할 수 있을까요? 그렇지 않다면, 인덱스와 기본키의 차이
    - 외래키와 인덱스의 차이
    - 인덱스가 데이터의 물리적 저장에 미치는 영향과 데이터가 저장되는 방식
    - RDB와 NoSQL (ex. Redis, MongoDB 등) 인덱스 차이

    **정규화**
    
    - 정규화를 하지 않을 경우, 발생할 수 있는 이상현상에 대해 설명해 주세요.
    - 각 정규화에 대해, 그 정규화가 진행되기 전/후의 테이블의 변화에 대해 설명해 주세요.
    - 정규화가 무조건 좋은가요? 그렇지 않다면, 어떤 상황에서 역정규화를 하는게 좋은지
    
    **DB Locking**
    
    - Optimistic Lock/Pessimistic Lock에 대해 설명해 주세요.
    - 물리적인 Lock을 건다면, 만약 이를 수행중인 요청에 문제가 생겨 비정상 종료되면 Lock이 절대 해제되지 않는 문제가 생길 수도 있을 것 같습니다. DB는 이를 위한 해결책이 있나요? 없다면, 우리가 이 문제를 해결할 수 없을까요?
    
    **DB의 Connection Pool**
    
    - DB와 Client가 Connection을 어떻게 구성하는지 설명해 주세요.
    
    **Table Full Scan, Index Range Scan**
    
    - 가끔은 인덱스를 타는 쿼리임에도 Table Full Scan 방식으로 동작하는 경우가 있습니다. 왜 그럴까요?
    - COUNT (개수를 세는 쿼리) 는 어떻게 동작하나요? COUNT(1), COUNT(*), COUNT(column) 의 동작 과정

- 7주차

  
    **쿠키와 세션의 차이에 대해 설명**
    
    - 세션 방식의 로그인 과정
    - HTTP의 특성인 Stateless
    - Stateless의 의미를 살펴보면, 세션은 적절하지 않은 인증 방법이지 않은지
    - 규모가 커져 서버가 여러 개가 됐을 때 세션 관리 방법
    
    **HTTP 응답코드에 대해 설명해 주세요.**
    
    - 401 (Unauthorized) 와 403 (Forbidden)의 의미적인 차이
    - 200 (ok) 와 201 (created) 의 차이
    - 필요하다면 저희가 직접 응답코드를 정의해서 사용할 수 있는지
    
    **HTTP Method 에 대해 설명해 주세요.**
    
    - HTTP Method의 멱등성
    - GET과 POST의 차이
    - POST와 PUT, PATCH의 차이
    - HTTP 1.1 이후로, GET에도 Body에 데이터를 실을 수 있게 되었음에도 왜 아직도 이런 방식을 지양하는가


    
    **인터럽트**
    
    - 하드웨어 인터럽트 vs 소프트웨어 인터럽트
    - 인터럽트 처리 방식
    - Polling 방식
    - 동시에 두 개의 인터럽트가 발생한다면?

    
    **프로세스**
    
    - 프로세스  vs 스레드
    - PCB
    - 리눅스에서 프로세스와 스레드의 생성 방법
    - 데몬 프로세스
    - 자식 프로세스가 상태를 알리지 않고 죽거나 부모 프로세스가 먼저 소멸된다면?
    - 리눅스에서 프로세스 트리의 루트 노드에 위치하는 프로세스
    
- 8주차
  
    **프로세스 주소 공간**
    
    - 초기화하지 않은 변수가 저장되는 곳
    - Stack과 Heap의 크기와 크기가 결정되는 순간
        - 개발자가 아닌 사용자가 이 크기를 결정할 수 있는지
    - Stack과 Heap 중 접근이 더 빠른 곳
    - 공간을 분할하는 이유
    - 스레드의 주소 공간
    - "스택"영역과 "힙"영역은 정말 자료구조의 스택/힙과 연관?
    - IPC의 Shared Memory 기법은 프로세스 주소 공간 중 들어가는 곳

    
    **스케쥴러**
    
    - 단기, 중기, 장기 스케쥴러와 현재 사용하는 스케쥴러
    - 프로세스의 스케쥴링 상태
    - preemptive/non-preemptive 에서 존재할 수 없는 상태
    - Memory가 부족할 경우, Process의 상태 변화
    
    **컨텍스트 스위칭**
    
    - 컨텍스트 스위칭이 발생했을 때 프로세스와 스레드의 차이
    - 컨텍스트 스위칭이 발생할 때, 기존의 프로세스 정보가 커널스택에 저장되는 형식
    - 컨텍스트 스위칭이 발생하는 시점
    
    **프로세스 스케쥴링 알고리즘**
    
    - RR을 사용할 때, Time Slice에 따른 trade-off
    - 싱글 스레드 CPU 에서 상시로 돌아가야 하는 프로세스가 있을 때 사용하는 스케쥴링 알고리즘
    - 동시성 vs 병렬성
    - 타 스케쥴러와 비교하여, Multi-level Feedback Queue가 해결하는 문제점
    - 스레드의 스케줄링 알고리즘

    
    **가상화가 무엇이고, 이것이 가상머신과 어떠한 차이가 있는지 설명해 주세요.**
    
    - 그렇다면 Docker는 둘 중 어디에 속하나요? 왜 사람들이 Docker를 많이 채택할까요?
    - 하나의 Host OS에서 돌아간다면 충분히 한 컨테이너가 다른 컨테이너에 간섭할 수 있는 위험이 있지 않을까요? 이를 어떻게 방어할 수 있을까요?
    - Docker 위에 Docker를 올릴 순 없을까요?


## 🍈 명예의 전당

| 주차 | 베스트 워크북 | 딥다이브 |
| --- | --- | --- |
| 1 | [조희수](https://github.com/CS-Wizards/CS/blob/main/week1/huisu/summary.md) | [JVM](https://github.com/CS-Wizards/CS/blob/main/Special/WEEK1/huisu.md) |
| 2 | [정혜선](https://github.com/CS-Wizards/CS/blob/main/week2/hyeseon/summary.md) | [Spring 동작 원리](https://github.com/CS-Wizards/CS/blob/main/Special/WEEK2/hyeseon.md) |
| 3 |   [정혜선](https://github.com/CS-Wizards/CS/blob/main/week3/hyeseon/summary.md)| [QUIC](https://github.com/CS-Wizards/CS/blob/main/Special/WEEK3/kim.md) |
| 4 |[김성일](https://github.com/CS-Wizards/CS/blob/main/week4/kim/summary.md) & [문인규](https://github.com/CS-Wizards/CS/blob/main/week4/ingyu/summary.md)| [Virtual Thread](https://github.com/CS-Wizards/CS/blob/main/Special/WEEK4/ingyu.md) |
| 5 |[김성일](https://github.com/CS-Wizards/CS/blob/main/week5/kim/summary.md)  | [InnoDB](https://github.com/CS-Wizards/CS/blob/main/Special/WEEK5/jin.md) |
| 6 |[황인준](https://github.com/CS-Wizards/CS/blob/main/week6/injun/summary.md)  | [DB Rock](https://github.com/InJun2/TIL/blob/main/CS-topic/DB/Lock.md) |
| 7 |[정혜선](https://github.com/CS-Wizards/CS/blob/main/week7/hyeseon/summary.md) & [황인준](https://github.com/CS-Wizards/CS/blob/main/week7/injun/summary.md)  | [JPA](https://github.com/CS-Wizards/CS/blob/main/Special/WEEK7/huisu.md) |
| 8 | [조희수](https://github.com/CS-Wizards/CS/blob/main/week8/huisu/summary.md) |  [Spring Security](https://github.com/CS-Wizards/CS/blob/main/Special/WEEK8/hyeseon.md)|

## 🥥 브랜치와 커밋

- 브랜치
    - `week${n}/name`
- 커밋
    
    
    | 태그이름 | 내용 |
    | --- | --- |
    | Feat | 새로운 기능 (파일 추가도 포함)을 추가할 경우 |
    | Update | 코드 수정을 한 경우 |
    | Docs | 문서를 수정한 경우 |
    | Rename | 파일 혹은 폴더명을 수정하거나 옮기는 작업만인 경우 |
    | Remove | 파일을 삭제하는 작업만 수행한 경우 |
- 폴더
    - `week${n}/name/summmary.md`  작성
    - 이미지는 `week${n}/name/img/` 밑에 작성
