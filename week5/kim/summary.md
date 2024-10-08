# **서브넷 마스크와 게이트웨이**

## 서브넷 (Subnet, Subnetwork)

**`요약`**

네트워크 내부의 네트워크로서 네트워크를 보다 효율적으로 만듬

서브넷으로 인해 네트워크 트래픽은 불필요한 라우터를 통과하지 않고, 대상에게 전송될때 보다 짧은 거리를 이동함

**`추가 설명`** 

- IP 주소는 네트워크 부분과 호스트 부분으로 구성됨.
    - 호스트 =  각각의 노드로서 PC, 스마트폰 등의 장치나 기기.
- 로컬 네트워크 = 하나의 라우터를 거쳐가는 여러개의 호스트들이 연결된 브로드캐스트 영역
- 다시 말해서, 한 노드가 브로드캐스트 했을때, 각 노드가 신호를 받음 
⇒ 그 노드의 집합이 하나의 네트워크.
- 다시 말해서, 한 로컬 네트워크 안에서 IP주소는 네트워크 부분은 같고, 호스트 부분은 다름
- 같은 로컬 네트워크에 있다면 게이트웨이를 통과해서 다른 네트워크로 나갈 필요가 없다.
- 이처럼 효율적인 네트워크 통신과, 브로드캐스팅을 위해 장치들을 일정한 규칙에 따라 
하나의 그룹으로 묶은것이 **`서브넷`**의 기본 개념이다.
- 기본적으로 서브넷 내의 호스트는 같은 서브넷의 호스트 끼리만 통신이 가능하다.
- 목적지가 같은 서브넷이 아니라면, 우선 게이트웨이를 통해 다른 네크워크로 나가야 한다.
    - 이후 여러 라우터를 거쳐 목적지(상대 호스트)가 속한 네트워크를 찾아서 간다.
- 어떤 기관이 B클래스 IP를 할당 받아서 대략 65000개의 IP를 사용할수 있는데 그중 1만개만 사용한다고 가정하면, 남은 IP가 너무 아까우니까 클래스를 쪼개서 더 효율적으로 쓰고자 나온 개념!

## 서브넷 마스크(Subnet Mask)

서브넷을 구분하는 방법중 하나.

서브넷 마스크를 어떤 범위로 하냐에 따라서 로컬 네트워크의 범위가 넓어지거나 좁아진다.

> **`192.168.0.1/24`**
> 

> **`192.168.0.1 서브넷 마스크:255.255.255.0`**
> 

IP 주소에서 위와 같은 것을 본 적이 있을 것이다.

여기서 **`/24`** 와 **`255.255.255.0`**은 같은 것을 나타낸다.

255.255.255.0을 2진수로 쓰면 1111 1111.1111 1111.1111 1111.0000 0000이다. 

여기서 앞에서부터 연속된 1의 개수만 나타낸 것이 /24이다. 

이렇게 슬래시로 표시되는 단위를 CIDR 표기법이라고도 한다.

기존에는 **`클래스`**라는 단위가 이 역할을 대신했다.

하지만 클래스 단위가 8비트 단위로 커지는 바람에 공인 아이피 대역을 기관에 할당하는 과정에서 할당량 조절에 실패하는 현상을 개선하고자 생긴 개념이다.

## **게이트웨이 (Gateway)**

**`요약`**

호스트가 자기가 속한 네트워크 외의 다른 네트워크의 호스트와 데이터를 주고 받을때,

데이터가 거쳐가야 하는 지점.

네트워크 — TO — 네트워크

네트워크 — GATEWAY — 네트워크

내부에서 외부로 나가거나, 외부에서 내부로 들어오거나 할때 출입구 역할을 하는 장비.

**`부연설명`** 

다수간의 통신경로는 너무 복잡하다.

따라서, 통신 경로를 단순화하고 연결 회선을 효율적으로 관리하기 위해서 다수의 호스트들을 여러 그룹으로 나누고, 각 그룹의 호스트들을 **`게이트웨이`**라는 한 지점에 공통적으로 접속하게 하고, 각 **`게이트웨이`**  를 서로 연결하면 다수 간의 통신 경로가 아주 단순해진다.

**`게이트 웨이의 주소`**

게이트웨이의 주소는 4개의 옥텟으로 구성됨 ⇒ 호스트(컴퓨터)의 IP주소의 일종.

다시 말해서 게이트웨이도 동일 네트워크에 있는 여러 호스트 중의 하나.

내부에서 외부로 나가거나, 외부에서 내부로 들어오거나 할때 출입구 역할을 하는 장비. (한번 더)

[예시]

네트워크 대역이 **`192.168.0.0`** ~ **`192.168.0.255`** 일때.

192.168.0.0 은 네트워크 주소로 할당된 상태고

192.168.0.255는 브로드캐스트 주소로 할당된 상태이다.

따라서 256개중 위에 두개를 뺀 나머지 주소에서 사용자가 임의로 선택해서 사용할수 있다.

## Q1. NAT

NAT = Network Address Translation.

IP 패킷에 적힌 소켓 주소의 포트 숫자와 소스 및 목적지의 IP주소 등을 재기록하면서 라우터를 통해 네트워크 트래픽을 주고받는 기술이다.

패킷에 변화가 생기기 때문에 IP나 TCP/UDP의 체크섬도 다시 계산해서 재기록해야한다.

따라서 네트워크의 성능에 영향을 준다.

사용하는 사례의 대부분은 여러대의 호스트가 하나의 공인 IP주소를 사용하여 인터넷에 접속하는 것이다.

공유기에 연결된 호스트는 사설 IP를 가진다 그리고 인터넷은 공인 IP로만 연결되어 통신이 가능하다. 따라서 공유기에 연결된 호스트는 외부 망과 통신하기 위해서 자신의 사설IP를 공인IP로 변환할 필요가 있다.

데이터를 외부망으로 보낼때 : 사설 IP ⇒ 공인 IP

데이터를 외부망에서 받을때 : 공인 IP ⇒ 사설 IP

## Q2. 서브넷 마스크의 표현 방식

서브넷 마스크는 IP주소와 유사한 32비트 2진수로 표현된다.

아이피와 조금 다른 점은 연속된 1과 연속된 0으로만 구성된다는 점이다.

**`10011111.11011111.11110011.00000000` [X]**

**`11111111.11111111.11111111.00000000` [O]**

덕분에 서브넷 마스크의 옥텟이 255면 해당 자리까지는 네트워크 아이디를 가리킨다

덕분에 간단하게 IP주소와 서브넷 마스크를 이용해서 해당 IP가 어느 클래스인지 알수있다.

**`11111111.00000000.00000000.00000000`  [A클래스]**

**`11111111.11111111.00000000.00000000`  [B클래스]**

**`11111111.11111111.11111111.00000000`  [C클래스]**

**`prefix 표현`** 

192.168.0.1/24 에서 /24는 32비트 중 앞에서부터 차례대로 센 1의 개수가 24개라는 의미이다. 32자리에서 24자리를 뺀 나머지 8개의 뒷자리에 0을 채워주면 서브넷 마스크가 된다.

기존의 방법은 표현하기 위해서 4Byte가 필요했지만 prefix표현은 6bit를 필요로 해서 네트워크 리소스를 절약할 수 있다.

## 레퍼런스

- https://nordvpn.com/ko/blog/what-is-subnet-mask/?srsltid=AfmBOoohjSoPYS4i0GGMwkE1fQlt7k5jDbSe68S8FC0WYb5Jw8XNecLm
- https://www.cloudflare.com/ko-kr/learning/network-layer/what-is-a-subnet/
- https://inpa.tistory.com/entry/WEB-IP-클래스-서브넷-마스크-서브넷팅-총정리
- [https://namu.wiki/w/서브넷 마스크](https://namu.wiki/w/%EC%84%9C%EB%B8%8C%EB%84%B7%20%EB%A7%88%EC%8A%A4%ED%81%AC)
- [https://velog.io/@rladmlduq47/서브넷팅-서브넷-마스크란#:~:text=서브넷 마스크는 IP 주소,을 서브넷으로 나누게 됩니다](https://velog.io/@rladmlduq47/%EC%84%9C%EB%B8%8C%EB%84%B7%ED%8C%85-%EC%84%9C%EB%B8%8C%EB%84%B7-%EB%A7%88%EC%8A%A4%ED%81%AC%EB%9E%80#:~:text=%EC%84%9C%EB%B8%8C%EB%84%B7%20%EB%A7%88%EC%8A%A4%ED%81%AC%EB%8A%94%20IP%20%EC%A3%BC%EC%86%8C,%EC%9D%84%20%EC%84%9C%EB%B8%8C%EB%84%B7%EC%9C%BC%EB%A1%9C%20%EB%82%98%EB%88%84%EA%B2%8C%20%EB%90%A9%EB%8B%88%EB%8B%A4).
- [https://namu.wiki/w/게이트웨이](https://namu.wiki/w/%EA%B2%8C%EC%9D%B4%ED%8A%B8%EC%9B%A8%EC%9D%B4)
- https://m.blog.naver.com/kangyh5/223175392071
- https://inpa.tistory.com/entry/WEB-🌐-IP-기초-사설IP-공인IP-NAT-개념-정말-쉽게-정리#nat_사설망_↔_외부_통신_방법
- https://velog.io/@emawlrdl/서브넷-마스크-게이트웨이
- https://hstory0208.tistory.com/entry/Gateway게이트웨이란-Router라우터란-각-개념과-차이점에-대해-알아보자
- https://inpa.tistory.com/entry/WEB-🌐-NAT-란-무엇인가
- https://namu.wiki/w/NAT

---

# 로드밸런서

## 로드밸런서 (**Load Balancer)**

**`요약`**

로드밸런서는 여러 서버에 클라이언트의 요청을 분산하여, 시스템의 성능과 안정성을 높이는 장치입니다. 로드밸런서는 주로 웹 애플리케이션에서 많이 사용되며, 네트워크 레벨에서의 L4 로드밸런서와 애플리케이션 레벨에서의 L7 로드밸런서가 대표적입니다.

## Q1. L4 로드밸런서와 L7 로드밸런서의 차이

**L4 로드밸런서 (Layer 4 Load Balancer)**:

- **작동 계층**: OSI 모델의 전송 계층 (Layer 4)에서 동작합니다.
- **기능**: IP 주소와 포트 번호를 기반으로 트래픽을 분산합니다. TCP/UDP 연결에 대해 로드밸런싱을 수행하며, 세션 상태나 패킷의 내용에는 접근하지 않습니다.
- **성능**: 패킷 단위로 로드밸런싱을 하기 때문에 빠르고, 낮은 레이턴시를 제공합니다.
- **적용 사례**: 대규모 트래픽 처리, 클라우드 네트워크, 간단한 트래픽 분산이 필요한 경우 사용됩니다.

**L7 로드밸런서 (Layer 7 Load Balancer)**:

- **작동 계층**: OSI 모델의 응용 계층 (Layer 7)에서 동작합니다.
- **기능**: HTTP 헤더, URL, 쿠키, 콘텐츠 타입 등 애플리케이션 레벨의 데이터를 분석하여 트래픽을 분산합니다. 특정 URI나 요청의 내용을 기반으로 라우팅할 수 있습니다.
- **성능**: 데이터를 깊이 분석하므로, L4 로드밸런서보다 처리 시간이 길고 리소스 소모가 큽니다.
- **적용 사례**: 웹 애플리케이션에서 특정 URL이나 API 요청을 특정 서버로 라우팅하거나, A/B 테스트, 콘텐츠 기반 라우팅이 필요할 때 사용됩니다.

## Q2. 로드밸런서 알고리즘

로드밸런서는 다양한 알고리즘을 통해 클라이언트의 요청을 여러 서버로 분산합니다. 주요 알고리즘은 다음과 같습니다:

**Round Robin (라운드 로빈)**:

- 서버 리스트를 순환하며 요청을 순차적으로 분배합니다. 간단하고 균형 잡힌 방법이지만, 서버의 성능 차이나 부하 상태를 고려하지 않습니다.

**Weighted Round Robin (가중치 라운드 로빈)**:

- 각 서버에 가중치를 부여하고, 가중치에 비례하여 요청을 분배합니다. 성능이 더 높은 서버에 더 많은 요청을 보낼 수 있습니다.

**Least Connections (최소 연결)**:

- 현재 연결 수가 가장 적은 서버로 요청을 보냅니다. 각 서버의 부하 상태를 고려하여, 연결 수가 많은 서버를 피합니다.

**IP Hash**:

- 클라이언트의 IP 주소를 해싱하여, 특정 서버로 라우팅합니다. 같은 클라이언트가 항상 동일한 서버로 연결되도록 보장할 수 있습니다.

**Least Response Time (최소 응답 시간)**:

- 응답 시간이 가장 짧은 서버로 요청을 보냅니다. 서버의 현재 부하와 성능을 반영하여 트래픽을 분산합니다.

## Q3. 로드밸런싱 대상이 되는 장치 중 일부 장치가 문제가 생겨 접속이 불가능할 경우, 로드밸런서가 해당 장비로 요청을 보내지 않도록 하는 방법

로드밸런서에서 장애가 발생한 서버로 트래픽을 보내지 않도록 하기 위해, **헬스 체크 (Health Check)** 기능을 사용합니다.

**헬스 체크 (Health Check)**:

- 로드밸런서는 주기적으로 각 서버의 상태를 확인하는 헬스 체크를 수행합니다. 이 과정에서 서버의 응답 여부, 특정 포트의 열림 상태, 특정 URL에 대한 응답 상태 등을 검사합니다.
- 헬스 체크를 통해 서버가 정상적으로 동작하지 않는다고 판단되면, 로드밸런서는 해당 서버로의 트래픽 분배를 중지합니다. 서버가 다시 정상화되면, 로드밸런서는 해당 서버로 트래픽을 재분배하기 시작합니다.
- 헬스 체크는 로드밸런서 설정에서 정의할 수 있으며, 간단한 핑(Ping) 검사부터 HTTP 요청 검사, 특정 애플리케이션의 응답 상태 확인까지 다양한 방법이 있습니다.

## Q4. **로드밸런서 장치를 사용하지 않고, DNS를 활용해서 유사하게 로드밸런싱을 하는 방법**

DNS를 활용한 로드밸런싱 방법은 **DNS 라운드 로빈 (DNS Round Robin)** 방식을 사용하여, 로드밸런서 장치 없이도 유사한 효과를 얻을 수 있습니다.

- **DNS 라운드 로빈 (DNS Round Robin)**:
    - 도메인 이름에 대해 여러 IP 주소를 등록하여, DNS 서버가 요청을 받을 때마다 다른 IP 주소를 반환합니다. 이를 통해, 클라이언트는 서로 다른 서버에 연결되며 트래픽이 분산됩니다.
    - 예를 들어, `example.com` 도메인에 대해 3개의 IP 주소(A 레코드)를 등록하면, DNS 요청이 들어올 때마다 서로 다른 IP를 순차적으로 반환하게 됩니다.
    - **장점**: 설정이 간단하고 추가적인 장치 없이도 분산 처리가 가능합니다.
    - **단점**: 실제 서버 상태를 반영하지 않기 때문에, 서버가 다운되거나 성능에 문제가 있어도 해당 서버로의 트래픽이 계속 전송될 수 있습니다. 이를 보완하기 위해 TTL(Time-To-Live) 값을 짧게 설정하여, DNS 레코드 갱신 주기를 빠르게 할 수 있지만, 이는 DNS 서버와 네트워크 트래픽 증가로 이어질 수 있습니다.

## 레퍼런스

https://www.smileshark.kr/post/what-is-a-load-balancer-a-comprehensive-guide-to-aws-load-balancer

---

# 라우터 내의 포워딩 과정

라우터는 네트워크에서 패킷을 목적지로 전달하는 중요한 장치입니다. 라우터는 패킷이 올바른 경로를 통해 전달되도록 **라우팅**과 **포워딩** 작업을 수행합니다.

포워딩 과정은 라우터가 패킷을 수신한 순간부터, 그 패킷을 다음 네트워크 장치로 전달할 때까지의 일련의 작업으로 구성됩니다. 이 과정은 다음과 같은 단계로 이루어집니다.

1. **패킷 수신**:
    - 라우터는 네트워크 인터페이스(이더넷, 광섬유 등)를 통해 패킷을 수신합니다.
    - 수신된 패킷은 라우터의 입력 인터페이스로 들어오게 됩니다.
2. **패킷 해독 및 분석**:
    - 라우터는 수신된 패킷의 IP 헤더를 분석합니다. 여기서 주요하게 확인하는 부분은 **목적지 IP 주소**입니다.
    - 또한, 패킷에 포함된 다른 필드(IP 버전, TTL 등)도 분석하여 적절한 처리 여부를 판단합니다.
3. **라우팅 테이블 조회**:
    - 목적지 IP 주소를 기반으로 라우터는 라우팅 테이블을 조회합니다. 라우팅 테이블에는 각 목적지 네트워크에 대한 최적 경로 정보가 저장되어 있습니다.
    - 라우터는 목적지 주소와 가장 일치하는 경로(네트워크 프리픽스가 가장 긴 것)를 찾아내고, 해당 경로에 대응하는 **출력 인터페이스**와 **다음 홉(Next Hop) 라우터의 IP 주소**를 확인합니다.
4. **TTL(시간 초과) 처리**:
    - IP 패킷의 헤더에는 **TTL(Time to Live)**라는 필드가 있습니다. TTL은 라우터를 거칠 때마다 1씩 감소하며, TTL이 0이 되면 패킷은 폐기됩니다. 이는 무한 루프 방지용입니다.
    - 라우터는 패킷의 TTL 값을 1 감소시키고, TTL이 0이 되지 않았는지 확인합니다. 만약 0이 되었다면, 라우터는 패킷을 폐기하고 ICMP 시간 초과 메시지를 송신자에게 보냅니다.
5. **MAC 주소 재작성**:
    - 라우터는 패킷을 다음 홉으로 전달할 때, 이더넷 프레임의 **출발지 MAC 주소**를 자신의 MAC 주소로 설정하고, **목적지 MAC 주소**를 다음 홉 라우터의 MAC 주소로 설정합니다.
    - 이를 위해, 라우터는 **ARP(주소 결정 프로토콜)**를 사용하여 다음 홉 라우터의 IP 주소에 대응하는 MAC 주소를 알아냅니다.
6. **패킷 포워딩**:
    - 라우터는 포워딩 테이블을 참조하여, 패킷을 적절한 출력 인터페이스로 전송합니다.
    - 출력 인터페이스를 통해 패킷이 다음 홉 라우터로 전송됩니다. 이 과정은 목적지에 도달할 때까지 여러 번 반복될 수 있습니다.

## Q1. 라우팅과 포워딩의 차이

- **라우팅 (Routing)**:
    - **정의**: 라우팅은 네트워크 내에서 데이터 패킷이 최적의 경로를 통해 목적지로 도달할 수 있도록 경로를 결정하는 과정입니다.
    - **작동 방식**: 라우터는 네트워크 전체의 경로 정보를 수집하고, 이를 바탕으로 라우팅 테이블을 작성합니다. 라우팅 프로토콜 (예: OSPF, BGP, RIP 등)을 사용하여 경로 정보를 교환하고, 네트워크 토폴로지 변화에 대응합니다.
    - **결과**: 라우팅의 결과로 생성된 라우팅 테이블은 포워딩 과정에서 참조됩니다. 즉, 라우팅은 "어디로 갈 것인가?"를 결정하는 과정입니다.
- **포워딩 (Forwarding)**:
    - **정의**: 포워딩은 이미 결정된 경로를 따라 데이터 패킷을 실제로 전송하는 과정입니다. 패킷이 들어오면, 라우터는 포워딩 테이블을 참조하여 목적지로 패킷을 전달합니다.
    - **작동 방식**: 라우터는 각 패킷의 목적지 IP 주소를 확인하고, 포워딩 테이블을 조회하여 해당 패킷을 어느 인터페이스로 전송할지 결정합니다.
    - **결과**: 포워딩은 "이미 결정된 경로를 따라 데이터를 전달하는 과정"입니다.

## Q2. 라우팅 알고리즘

라우팅 알고리즘은 라우터가 네트워크 내에서 최적의 경로를 찾기 위해 사용하는 규칙이나 방법입니다. 주요 라우팅 알고리즘은 다음과 같습니다:

**Distance Vector (거리 벡터)**:

- 라우터는 인접한 라우터들과 거리 정보를 교환하며, 최적의 경로를 계산합니다. 각 경로의 비용을 계산하여, 비용이 가장 적은 경로를 선택합니다.
- **프로토콜**: RIP (Routing Information Protocol) 등이 사용됩니다.
- **장점**: 구현이 간단하며, 작은 네트워크에 적합합니다.
- **단점**: 네트워크가 커질수록 라우팅 업데이트에 시간이 많이 걸리며, 큰 네트워크에서는 불안정할 수 있습니다.

**Link State (링크 상태)**:

- 각 라우터는 네트워크의 전체 구조를 알고 있으며, Dijkstra 알고리즘을 사용해 최단 경로를 계산합니다. 링크 상태 정보를 주기적으로 모든 라우터에 전송하여 최신 경로를 유지합니다.
- **프로토콜**: OSPF (Open Shortest Path First) 등이 사용됩니다.
- **장점**: 큰 네트워크에서도 효율적이며, 빠른 경로 수정을 지원합니다.
- **단점**: 링크 상태 정보의 전파와 계산이 복잡하여, 더 많은 메모리와 처리 능력이 요구됩니다.

**Hybrid (혼합형)**:

- Distance Vector와 Link State의 혼합된 형태로, 둘의 장점을 결합한 알고리즘입니다.
- **프로토콜**: EIGRP (Enhanced Interior Gateway Routing Protocol)가 대표적입니다.
- **장점**: 안정성 및 확장성이 뛰어나며, 네트워크 전반에서 효율적인 라우팅이 가능합니다.
- **단점**: 상대적으로 복잡한 설정과 운영이 요구될 수 있습니다.

## 포워딩 테이블의 구조

포워딩 테이블(Forwarding Table, 혹은 FIB: Forwarding Information Base)은 라우터가 패킷을 올바른 인터페이스로 전달하기 위해 사용하는 데이터 구조입니다. 포워딩 테이블은 다음과 같은 정보를 포함합니다:

**목적지 주소 (Destination Address)**:

- 목적지 IP 주소 또는 네트워크 주소입니다. 라우터는 수신된 패킷의 목적지 주소를 이 필드와 비교하여, 가장 일치하는 항목을 찾습니다.

**넷마스크 (Netmask)**:

- 네트워크와 호스트 부분을 구분하는 데 사용되는 서브넷 마스크입니다. 목적지 주소와 넷마스크를 조합하여 네트워크 주소를 계산합니다.

**차선 인터페이스 (Next Hop Interface)**:

- 패킷을 전송할 라우터의 인터페이스입니다. 라우터는 패킷을 이 인터페이스를 통해 다음 네트워크로 전달합니다.

**차선 IP 주소 (Next Hop IP Address)**:

- 패킷을 전달할 다음 라우터의 IP 주소입니다. 라우터는 이 주소로 패킷을 전송합니다.

**메트릭 (Metric)**:

- 경로의 비용 또는 우선순위를 나타내는 값입니다. 여러 경로가 있을 때, 메트릭이 낮은 경로가 우선 선택됩니다. 메트릭은 거리, 대역폭, 트래픽, 지연 시간 등을 기준으로 계산됩니다.

**타이머 및 기타 정보**:

- 경로의 유효성을 확인하기 위한 타이머, 정책 기반 라우팅 등의 부가 정보가 포함될 수 있습니다.

## 레퍼런스

---

# 트랜잭션과 ACID 원칙

트랜잭션(Transaction)은 데이터베이스의 상태를 변화시키는 일련의 작업들을 하나로 묶은 논리적인 작업 단위입니다. 트랜잭션은 일관된 데이터 상태를 유지하기 위해 중요한 역할을 하며, 특히 데이터베이스의 무결성을 보장하는 데 필수적입니다.

ACID 원칙은 트랜잭션이 안전하게 처리되도록 보장하는 4가지 핵심 속성을 나타냅니다. ACID는 **Atomicity(원자성)**, **Consistency(일관성)**, **Isolation(고립성)**, **Durability(지속성)**의 약자입니다.

## Q1. **ACID 원칙 중 Durability를 DBMS가 보장하는 방법**

**Durability (지속성)**:

- **정의**: 지속성은 트랜잭션이 성공적으로 완료된 후에, 그 결과가 영구적으로 데이터베이스에 저장되는 것을 보장하는 원칙입니다. 즉, 트랜잭션이 완료되면 시스템 오류가 발생해도 그 결과는 데이터베이스에서 사라지지 않아야 합니다.
- **DBMS가 보장하는 방법**:
    - **로그 기반 회복(Log-Based Recovery)**: 트랜잭션이 완료되기 전에 모든 변경 사항을 로그(Write-Ahead Log, WAL)에 기록합니다. 로그에는 트랜잭션의 시작, 커밋, 롤백 등의 정보를 포함하며, 시스템이 충돌해도 로그를 기반으로 데이터베이스를 복구할 수 있습니다.
    - **백업 및 체크포인트(Backup and Checkpointing)**: DBMS는 주기적으로 데이터베이스 상태를 백업하고, 체크포인트를 생성합니다. 체크포인트는 시스템 충돌 시 데이터베이스 복구에 필요한 지점을 나타내며, 복구 시간을 단축시킵니다.

## Q2. **읽기에는 트랜잭션을 걸지 않아도 되는지**

**읽기에 트랜잭션이 필요한 이유**:

읽기 작업에서 트랜잭션이 필요하지 않다고 생각할 수 있지만, 실제로는 데이터 일관성과 정확성을 보장하기 위해 트랜잭션을 사용하는 것이 중요할 수 있습니다. 트랜잭션이 없는 읽기 작업은 다음과 같은 문제를 일으킬 수 있습니다:

- **Dirty Read (더티 리드)**: 다른 트랜잭션이 아직 커밋되지 않은 데이터를 읽는 경우입니다. 나중에 해당 트랜잭션이 롤백되면, 읽은 데이터는 유효하지 않게 됩니다.
- **Non-Repeatable Read (비반복적 읽기)**: 하나의 트랜잭션 내에서 같은 데이터를 두 번 읽을 때, 그 사이에 다른 트랜잭션이 데이터를 수정하면, 두 번의 읽기 결과가 달라질 수 있습니다.
- **Phantom Read (팬텀 리드)**: 트랜잭션 내에서 같은 조건으로 여러 번 쿼리를 실행할 때, 다른 트랜잭션이 데이터를 삽입하거나 삭제함으로써 읽은 데이터의 개수가 달라질 수 있습니다.

**읽기에도 트랜잭션을 사용하는 경우**:

- 중요한 읽기 작업에서는 이러한 문제를 방지하기 위해 트랜잭션을 사용하는 것이 좋습니다. 특히, 일관된 데이터를 보장해야 하는 상황에서는 읽기 작업도 트랜잭션 내에서 수행하는 것이 안전합니다.
- DBMS는 읽기 작업을 위한 다양한 격리 수준(Isolation Level)을 제공하며, 이를 통해 읽기 작업 중에 발생할 수 있는 데이터 일관성 문제를 제어할 수 있습니다. 예를 들어, "Repeatable Read" 격리 수준에서는 트랜잭션 내에서 동일한 데이터를 읽을 때 항상 동일한 결과를 보장합니다.

## 레퍼런스

- https://f-lab.kr/insight/transaction-management-and-acid-principles
- https://wantodo.tistory.com/41
- https://mangkyu.tistory.com/300

---

# 트랜잭션 격리 레벨

트랜잭션 격리 레벨은 동시에 여러 트랜잭션이 수행될 때 각 트랜잭션이 서로 간섭하지 않도록 격리하는 정도를 나타냅니다. 이는 데이터의 일관성을 유지하면서 성능을 최적화하기 위해 다양한 수준의 격리를 제공합니다. 주요한 격리 수준은 **Read Uncommitted**, **Read Committed**, **Repeatable Read**, **Serializable**의 4가지입니다.

## Q1. 모든 DBMS가 4개의 레벨을 모두 구현하는지

- 모든 DBMS가 4개의 격리 레벨을 모두 구현하는 것은 아닙니다. 대부분의 상용 DBMS는 이 4개의 표준 격리 레벨을 지원하지만, 각 DBMS의 구현 방식이나 기본 설정은 다를 수 있습니다.
- **Oracle**: Oracle은 두 가지 주요 격리 수준만을 지원합니다: **Read Committed**와 **Serializable**. Oracle에서는 기본적으로 **Read Committed**가 사용되며, 이는 대부분의 경우에서 충분히 안전한 수준의 격리를 제공합니다.
- **MySQL**: MySQL(InnoDB)은 4개의 격리 수준을 모두 지원합니다. 기본 격리 수준은 **Repeatable Read**로 설정되어 있습니다.
- **SQL Server**: SQL Server도 4개의 표준 격리 수준을 지원하며, 추가적으로 **Snapshot Isolation** 같은 특별한 격리 수준도 제공합니다.

## Q2. 만약 MySQL을 사용하고 있다면, 
(InnoDB 기준) Undo 영역과 Redo 영역

InnoDB는 MySQL에서 가장 많이 사용되는 스토리지 엔진이며, 트랜잭션 무결성을 유지하기 위해 Undo 로그와 Redo 로그를 사용합니다.

- **Undo 영역**:
    - **역할**: Undo 로그는 트랜잭션이 수행되기 전의 데이터 상태를 저장하는 공간입니다. 트랜잭션이 완료되기 전에 오류가 발생하거나 롤백되는 경우, Undo 로그를 사용하여 데이터베이스를 이전 상태로 복원할 수 있습니다.
    - **작동 방식**: 트랜잭션이 시작될 때, 데이터가 수정되기 전의 값을 Undo 로그에 기록합니다. 트랜잭션이 롤백되면 Undo 로그를 참조하여 데이터를 이전 상태로 되돌립니다. 이로 인해 사용자가 일관된 데이터 상태를 볼 수 있게 됩니다.
- **Redo 영역**:
    - **역할**: Redo 로그는 트랜잭션의 변경 사항을 기록하며, 시스템 장애 후 데이터를 복구하는 데 사용됩니다. Redo 로그를 통해 시스템 충돌 후에도 최근의 변경 사항을 다시 적용하여 데이터베이스를 복구할 수 있습니다.
    - **작동 방식**: 트랜잭션이 변경 작업을 수행할 때, 해당 변경 사항이 디스크에 영구적으로 기록되기 전에 Redo 로그에 기록됩니다. 이후, Redo 로그의 내용을 디스크에 쓰는 과정이 완료되면 트랜잭션이 커밋됩니다. 시스템 장애가 발생하면 Redo 로그를 읽어, 완료되지 않은 트랜잭션의 변경 사항을 복구하거나 완료된 트랜잭션의 변경 사항을 재적용할 수 있습니다.

## Q3. 스토리지 엔진

- **정의**: 스토리지 엔진은 데이터베이스 관리 시스템(DBMS)에서 데이터를 저장, 관리, 조회하는 방법을 담당하는 소프트웨어 모듈입니다. 각 스토리지 엔진은 데이터의 저장 구조, 인덱싱 방법, 트랜잭션 처리 방식, 동시성 제어, 복구 메커니즘 등에서 차이를 보입니다.
- **InnoDB**: MySQL의 기본 스토리지 엔진으로, 트랜잭션 지원, 외래 키 제약, MVCC, ACID 속성 보장을 특징으로 합니다. 특히, InnoDB는 높은 동시성과 데이터 무결성을 요구하는 애플리케이션에 적합합니다.

## 레퍼런스

- https://mangkyu.tistory.com/299
- https://velog.io/@pk3669/Mysql-Redo-Undo-Log
- https://velog.io/@sweet_sumin/DB-스토리지-엔진-별-차이

---
