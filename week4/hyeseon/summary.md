# **IP 주소는 무엇이며, 어떤 기능을 하고 있나요?**

## IP 개념

IP(Internet Protocal Address)는 네트워크에 연결된 각 장치(컴퓨터, 스마트폰, 서버 등)가 **서로 통신할 수 있도록 식별하는 고유한 숫자형 주소**입니다. IP 주소는 IPv4와 IPv6 두 가지 형태로 나뉘며, 각각 서로 다른 방식으로 주소를 표현합니다.

## IP 사용 목적 및 기능

1. **장치 식별**

   네트워크 상에서 모든 장치는 고유한 IP 주소를 가집니다. 이를 통해 각 장치는 서로를 인식하고 데이터가 정확한 목적지로 전달될 수 있도록 합니다.

2. **데이터 전송**

   IP 주소는 데이터 패킷이 발신지에서 수신지로 이동할 때 정확한 경로를 지정합니다. 이를 통해 인터넷 상에서 데이터를 안전하고 정확하게 주고받을 수 있습니다.

3. **네트워크 관리**

   네트워크 관리자들은 IP주소를 사용하여 네트워크의 트래픽을 관리하고 문제를 진단하며 특정 장치에 대한 접근을 제어할 수 있습니다.

4. **위치 추적**

   IP 주소는 인터넷에 연결된 장치의 대략적인 지리적 위치를 나타낼 수 있습니다. 이를 통해 특정 지역에서 발생하는 트래픽 패턴을 분석하거나 보안 목적으로 사용됩니다.


## 동작 과정

### 1. 할당

IP 주소는 네트워크에 연결될 때 자동으로 할당되거나 수동으로 설정될 수 있습니다. 공인 IP 주소는 인터넷 서비스 제공자(ISP)가, 사설 IP 주소는 로컬 네트워크 관리자 또는 라우터가 할당합니다.

**1) 공인 IP 주소 할당**

공인 IP 주소는 전 세계적으로 고유한 주소로 인터넷에 직접 연결되는 장치에 할당됩니다. 공인 IP 주소는 인터넷 서비스 제공자(ISP)가 인터넷에 연결된 고객에게 할당합니다. 이 주소는 IANA(Internet Assigned Numbers Authority)와 RIR(Regional Internet Registries)와 같은 기관에 의해 관리됩니다.
이 방식에는 또다시 두 가지 주소가 부여될 수 있는데, 하나는 ISP가 고객에게 영구적으로 할당하는 IP 주소인 **고정 IP 주소**(Static IP Address)이고, 다른 하나는 네트워크에 접속할 때마다 ISP가 고객에게 할당하는 IP 주소인 **동적 IP 주소**(Dynamic IP Address)가 있습니다.

**2) 사설 IP 주소 할당**

사설 IP 주소는 로컬 네트워크 내에서만 유효한 주소입니다. 사설 IP 주소는 라우터와 같은 네트워크 장치에 의해 네트워크 내의 각 장치에 할당됩니다. 사설 IP 주소는 인터넷에 직접 연결되지 않으면 NAT(Network Address Translation)라는 기술을 통해 공인 IP 주소로 변환되어 인터넷에 접근할 수 있습니다.
이 방식에도 또다시 두 가지 주소가 부여될 수 있는데, 네트워크 관리자가 특정 장치에 수동으로 할당하는 주소인 **고정 사설 IP 주소**가 있고, DHCP(Dynamic Host Configuration Protocol) 서버가 네트워크에 연결된 장치에 자동으로 할당하는 IP 주소인 **동적 사설 IP 주소**가 있습니다.

**2-1) 사설 IP를 사용하는 이유**

사설 IP 주소는 주로 로컬 네트워크(ex. 가정, 회사, 학교 등) 내에서 장치 간의 통신을 위해 사용됩니다. 사설 IP 주소를 사용하는 이유는 다음과 같습니다.

1. 공인 IP 주소의 부족 문제 해결

   IPv4의 경우 전 세계의 모든 장치에 공인 IP 주소를 할당하는 것은 불가능하기 때문에 사설 IP 주소를 사용하여 네트워크 내에서 장치를 관리하고 NAT 기술을 통해 인터넷에 접속할 수 있게 합니다.

2. 네트워크 보안 강화

   사설 IP 주소는 외부 인터넷에서 직접 접근할 수 없습니다. 이는 외부 공격자가 사설 IP 주소를 가진 장치에 직접 접근하는 것을 막아 네트워크의 보안을 강화하는 데 도움이 됩니다.

3. IP 주소 재사용

   사설 IP 주소는 동일한 네트워크 내에서만 유효하므로 전 세계 여러 네트워크에 동일한 사설 IP 주소를 동시에 사용할 수 있습니다.

4. 네트워크 관리 용이 및 비용 절감

### 서브넷팅

로컬 네트워크에서 사설 IP 주소를 나누는 방법에서 라우터나 네트워크 관리자가 특정 IP 주소 대역을 설정하고 이를 네트워크 내의 장치들에게 할당하는 방식을 서브넷팅이라 합니다.

1. 사설 IP 주소 대역 선택

   사설 IP 주소는 3개의 대역으로 나뉘어 사용할 수 있습니다.

   `10.0.0.0 ~ 10.255.255.255` : 큰 네트워크에서 주로 사용

   `172.16.0.0 ~ 172.31.255.255` 중간 규모 네트워크에서 주로 사용

   `192.168.0.0 ~ 192.168.255.255` : 가정이나 소규모 네트워크에서 가장 많이 사용

2. 서브넷팅(Subnetting)

   서브넷팅은 큰 네트워크를 더 작은 네트워크로 나누는 과정입니다. 이를 통해 네트워크 내에서 더 세분화된 주소 관리를 할 수 있습니다.

   예를 들어, `192.168.0.0/24` 대역을 두 개의 서브넷으로 나누고 싶다면 `/25` 서브넷 마스크를 사용할 수 있습니다. 이렇게 하면

    - 첫 번째 서브넷 : `192.168.0.0 ~ 192.168.0.127` (서브넷 마스크: `255.255.255.128`)
    - 두 번째 서브넷 : `192.168.0.128 ~ 192.168.0.255` (서브넷 마스크: `255.25.255.128`)

   각각의 서브넷에 128개의 IP주소를 할당할 수 있게 됩니다.

   > 사용 가능한 주소: 126개, 첫번째 주소: 네트워크 주소, 마지막 주소: 브로드 캐스트 주소)

### 2. 도메인 네임 시스템(DNS)

사용자가 웹 사이트 주소(ex. `www.example.com`)를 입력하면 DNS가 이를 해당하는 IP 주소로 변환합니다. 이렇게 변환된 IP 주소를 통해 사용자의 요청이 올바른 서버로 전달됩니다.

### 3. 패킷 전송

IP 주소를 기반으로 데이터가 패킷으로 나뉘어 전송됩니다. 각 패킷에는 발신지와 수신지의 IP 주소가 포함되어 있으며, 인터넷을 통해 여러 라우터를 거쳐 목적지로 전달됩니다.

### 4. 라우팅

네트워크 상에서 여러 경로 중 최적의 경로를 선택하여 데이터 패킷을 전달하는 과정을 라우팅이라고 합니다. 이 과정에서 라우터는 IP 주소를 참조하여 패킷을 적절한 경로로 전송합니다.

### 5. 데이터 재조립

목적지에 도달한 패킷들은 원래의 데이터로 재조립됩니다. 이 때, IP 주소는 패킷들이 올바르게 조립될 수 있도록 도와줍니다.

<details> 
<summary><h3>IPv4와 IPv6의 차이</h3></summary>
<div markdown="1">

### IPv4

32비트 주소 체계를 사용합니다.   
4개의 옥텟으로 나뉘며, 각각은 0에서 255 사이의 10진수로 표현됩니다.   
약 43억개의 고유한 IP 주소를 제공합니다(2^32개)  
ex. `192.168.0.1`

### IPv6

IPv4의 한계를 극복하기 위해 설계된 새로운 버전입니다.    
128비트 주소 체계를 사용합니다.  
8개의 16비트 블록으로 나뉘며 각 블록은 16진수로 표현됩니다.   
사실상 무한에 가까운 주소 공간을 제공합니다(2^128개)    
ex. `2001:0db8:85a3:0000:0000:8a2e:0370:7334`

</div>
</details>

<details> 
<summary><h3>수많은 공유기에서는 고정 주소를 제공하는 기능의 동작 방법</h3></summary>
<div markdown="1">

**DHCP 예약** 기능을 통해 특정 장치가 연결될 때마다 항상 동일한 IP 주소를 할당받도록 설정할 수 있습니다.    
동작 원리는 다음과 같습니다.

- DHCP 서버는 장치가 네트워크에 접속할 때마다 그 장치의 MAC 주소를 확인하고, 예약된 IP 주소가 있으면 해당 IP 주소를 할당합니다.
   - DHCP 예약 설정은 일반적으로 공유기(라우터)의 내부 메모리(비휘발성)에 저장됩니다.
- 장치가 재부팅되거나 네트워크에서 연결이 끊어졌다가 다시 연결될 때도 동일한 IP 주소를 받게 됩니다.

**장점**     
이 기능은 네트워크에서 특정 장치가 항상 같은 IP 주소를 사용하도록 하여 네트워크 관리를 쉽게 하고 IP 주소 충돌을 방지하는 데 도움이 됩니다.

</div>
</details>

<details> 
<summary><h3>IPv4를 사용하는 장비와 IPv6를 사용하는 같은 네트워크 내에서 통신하는 방법</h3></summary>
<div markdown="1">

### 이중 스택(dual stack) 사용

이중 스택은 네트워크 장치가 IPv4와 IPv6 주소를 모두 사용하는 환경을 의미합니다. 이 방법은 가장 일반적이고 추천되는 방법입니다.  
이중 스택을 지원하는 장비(라우터, 컴퓨터 등)는 IPv4와 IPv6 네트워크 모두에 동시에 연결될 수 있습니다.   
같은 네트워크 내에서 IPv4 주소와 IPv6 주소를 모두 가진 장치들은 서로 통신할 때 각 프로토콜에 따라 적절한 주소를 사용합니다.   
터널링이나 번역도 사용할 수 있지만, 이 방법들은 성능이나 설정의 복잡성 측면에서 고려해야 할 부분이 많습니다.    


</div>
</details>

<details> 
<summary><h3>IP가 송신자와 수신자를 정확하게 전송되는 것의 보장 유무</h3></summary>
<div markdown="1">


IP는 송신자와 수신자 간의 데이터를 전달하는 역할을 하지만, **데이터 전송의 신뢰성을 보장하지는 않습니다**. IP는 패킷을 목적지까지 전송하는 기능을 제공하지만, 패킷이 올바르게 도착하는지 손상되지 않았는지 순서가 맞는지 등을 보장하지 않습니다.    
신뢰성이 필요한 경우, TCP와 같은 상위 계층 프로토콜을 사용하여 데이터 전송의 안정성과 신뢰성을 확보할 수 있습니다.


</div>
</details>

<details> 
<summary><h3>IPv4에서 수행하는 Checksum과 TCP에서 수행하는 Checksum의 차이</h3></summary>
<div markdown="1">

### IPv4의 체크섬
IP 헤더의 무결성을 확인하는 데 사용됩니다. 데이터 부분은 체크섬 계산에 포함되지 않습니다. 주로 라우팅 중에 헤더가 손상되었는지 확인하기 위해 사용되며, 각 라우터를 거칠 때마다 TTL 등 헤더 필드가 변경되므로 IP 체크섬은 매번 재계산됩니다.    
IP 패킷이 네트워크를 통해 전송되는 동안 각 라우터에서 IP 체크섬이 검증됩니다. 즉, IPv4 체크섬은 네트워크 계층에서 헤더의 무결성을 보장하는 역할을 합니다.

### TCP의 체크섬
TCP 세그먼트 전체의 무결성을 확인하는 데 사용됩니다. 여기에는 TCP 헤더와 데이터가 포함됩니다. 데이터가 도착한 후 수신 측에서 다시 체크섬을 계산하여 비교합니다. 만약 값이 일치하지 않으면 데이터가 손상된 것으로 판단하여 해당 세그먼트를 폐기하고 재전송을 요청합니다.   
데이터가 송신자에서 수신자로 전송 된 후 수신자가 데이터 무결성을 확인할 때 검증됩니다. 즉, TCP 체크섬은 전송 계층에서 전체 세그먼트의 무결을 보장하는 역할을 합니다.

</div>
</details>

<details> 
<summary><h3>TTL(Hop Limit)</h3></summary>
<div markdown="1">

### 개념

TTL(Time to Live)은 네트워크에서 데이터 패킷이 전송될 때 그 패킷이 네트워크를 통해 얼마나 멀리 전송될 수 있는지를 제한하는 값입니다. TTL은 패킷이 네트워크 상에서 무한히 순환하는 것을 방지하고 네트워크 자원을 효율적으로 관리하기 위해 사용됩니다.

### 사용 목적
1. 네트워크 루프 방지

   라우팅 테이블이 잘못 설정되었거나, 네트워크에서 루프가 발생하면 TTL이 없을 경우 패킷은 무한히 순환할 수 있습니다. TTL을 통해 이를 방지할 수 있습니다.

2. 네트워크 자원 보호

   불필요하게 네트워크 자원을 소모하는 것을 방지합니다.

3. 트러블슈팅 도구

   네트워크 문제를 진단하는 데 사용됩니다. 네트워크 경로를 추적하거나 특정 노드까지의 지연 시간을 측정하여 트러블 슈팅을 할 수 있습니다.


### 동작 방식
1. 초기 설정

   TTL은 IP 패킷의 헤더에 포함된 필드로 패킷이 생성될 때 송신자가 초기값을 설정합니다.

2. 라우터 통과 시 감소

   패킷이 네트워크를 통해 전송될 때마다 각 라우터는 TTL 값을 1씩 감소시킵니다. 만약 TTL 값이 0이 되면 해당 패킷은 더 이상 전송되지 않고 해당 라우터에서 폐기됩니다.

3. 패킷 폐기 및 ICMP 메시지

   TTL 값이 0이 되어 패킷이 폐기 될 경우 해당 라우터는 원래 송신자에게 ICMP(Time Exceeded) 메시지를 보냅니다.


</div>
</details>

<details> 
<summary><h3>IP 주소와 MAC 주소의 차이</h3></summary>
<div markdown="1">

**IP 주소**   
IP 주소는 네트워크 상에서 장치의 위치를 식별하기 위한 논리적 주소입니다.   
네트워크 연결에 따라 변경될 수 있으며 네트워크 간 통신에서 사용됩니다.

**MAC 주소**  
MAC 주소는 네트워크 장치의 고유 식별자로서, 장치 자체를 식별합니다.  
기본적으로 변경되지 않으며 동일 네트워크 내에서(LAN) 데이터의 전송 경로를 결정하는 데 사용합니다.


</div>
</details>

# **OSI 7계층**

### 개념

OSI 7계층(Open Systems Interconnection 7계층)은 네트워크 통신을 체계적으로 이해하고 설계하기 위해 국제표준기구(ISO)에서 정의한 표준 모델입니다. 각 계층은 특정한 네트워크 기능을 담당하며, 계층 간의 상호작용을 통해 데이터를 전송합니다.

### 구조

### 7계층, 응용 계층(Application Layer)
**사용자와 네트워크 애플리케이션 간의 인터페이스를 제공**하는 계층입니다.   
사용자에게 네트워크 서비스를 직접 제공하며, 애플리케이션이 네트워크를 통해 데이터를 송수신할 수 있도록 합니다.    
애플리케이션 프로토콜(ex. HTTP, FTP 등)을 통해 사용자의 요구를 처리합니다.   
응용 프로그램에서 수행됩니다.  

### 6계층, 표현 계층(Presentation Layer)
데이터의 형식 변환 및 인코딩/디코딩을 통해 애플리케이션 간의 데이터 호환성을 보장할 수 있는 계층입니다.    
응용 프로그램 및 운영체제에서 수행됩니다.

### 5계층, 세션 계층(Session Layer)
애플리케이션 간의 세션을 설정, 유지, 종료하며 데이터 교환을 관리하는 계층입니다.  
예를 들어, 파일 전송 세션을 설정하거나 데이터베이스 연결을 관리합니다.  
응용 프로그램 및 세션 계층에서 수행됩니다.

### 4계층, 전송 계층(Transport Layer)
데이터의 전송 품질을 관리하는 계층으로 데이터 전송의 신뢰성, 흐름 제어, 오류 복구 등을 담당하는 계층입니다. 대표적인 프로토콜로 TCP와 UDP가 있습니다.    
주로 컴퓨터의 운영 체제에서 수행됩니다.

### 3계층, 네트워크 계층(Network Layer)
데이터를 목적지까지 가장 효율적으로 전달하기 위한 경로를 설정하는 계층입니다. IP 주소를 이용하여 패킷을 라우팅하며 라우터가 이 계층에서 작동합니다.   
컴퓨터의 운영 체제에서 수행됩니다. 또는 라우터나 L3 스위치에서도 수행됩니다.

### 2계층, 데이터 링크 계층(Data Link Layer)
물리 계층을 통해 전송된 데이터를 신뢰성 있게 전달하기 위한 계층입니다. 데이터 프레임의 생성, 오류 감지 및 수정, 흐름 제어 등을 수행하며 MAC 주소를 이용해 물리적 주소 지정을 합니다. 스위치와 브리지가 이 계층에서 작동합니다.    
네트워크 인터페이스 카드(NIC)와 스위치에서 작동합니다.

### 1계층, 물리 계층(Physical Layer)
전송 매체를 통해 실제로 데이터를 전송하는 계층입니다. 전기적 신호, 광신호, 전송 매체의 물리적 특성 등을 다룹니다.   
하드웨어, 케이블, 리피터, NIC 등에서 수행됩니다.

### 예시
사용자 A가 자신의 컴퓨터에서 사용자 B의 컴퓨터로 이메일을 보내는 상황을 가정해봅시다.

**사용자 A의 컴퓨터**

1. 응용 계층

   사용자A는 이메일 클라이언트 프로그램(Gmail)을 열고 메시지를 작성한 후 사용자B의 이메일 주소로 이메일을 전송합니다.

2. 표현 계층

   작성된 이메일은 문자, 이미지 등의 데이터를 포함할 수 있습니다. 이 데이터가 네트워크를 통해 전송할 수 있는 형식으로 변환합니다.

3. 세션 계층

   사용자 A의 이메일 클라이언트와 사용자 B의 이메일 서버 간의 통신 세션이 설정됩니다.

4. 전송 계층

   이메일이 잘 전송될 수 있도록 데이터를 작은 세그먼트로 나누고 각 세그먼트에 순서 번호를 부여합니다. 신뢰성 있는 전송을 위해 오류 검사와 흐름 제어를 수행합니다.

5. 네트워크 계층

   데이터를 사용자B의 이메일 서버까지 가장 효율적인 경로를 통해 전달합니다. IP 주소를 이용해 데이터 패킷을 라우팅합니다.

6. 데이터 링크 계층

   네트워크 계층에서 받은 패킷을 프레임으로 캡슐화하고 물리 계층을 통해 전송할 수 있도록 준비합니다.

7. 물리 계층

   데이터를 전기 신호, 광신호 등으로 변환하여 실제 네트워크 케이블이나 무선 통신을 통해 전송합니다. 이 단계에서 데이터는 사용자 A의 컴퓨터에서 사용자B의 이메일 서버로 전송됩니다.


**사용자 B의 이메일 서버**

1. 물리 계층

   네트워크를 통해 전달 받은 신호를 데이터 링크 계층으로 보냅니다.

2. 데이터 링크 계층

   물리 계층에서 받은 비트 스트림을 프레임으로 변환합니다. 이때 오류가 없다면 프레임은 네트워크 계층으로 전달됩니다.

3. 네트워크 계층

   데이터 링크 계층에서 받은 프레임에서 패킷을 꺼내고, IP 주소를 기반으로 데이터의 목적지와 출발지 정보를 확인합니다. 만약 수신된 패킷이 여러 조각이라면 올바르게 정렬하는 역할을 합니다.

4. 전송 계층

   네트워크 계층에서 전달된 패킷에서 세그먼트를 꺼내고 데이터의 신뢰성을 보장하기 위해 오류 검사를 수행합니다. 특히 TCP 프로토콜을 사용할 경우, 데이터가 올바른 순서로 도착했는지 확인하고 손실된 데이터가 있다면 재전송을 요청합니다.

5. 세션 계층

   이메일 데이터 전송을 위한 세션을 설정하고, 전송된 데이터를 응용 계층으로 전달합니다.

6. 표현 계층

   이메일 내용이 암호화되어 있었다면 표현 계층에서 이를 복호화하여 사람이 읽을 수 있는 형식으로 변환합니다.

7. 애플리케이션 계층

   사용자 B가 이메일 클라이언트를 통해 이메일을 확인할 수 있습니다.


<details> 
<summary><h3>Transport Layer와, Network Layer의 차이</h3></summary>
<div markdown="1">

**전송 계층**

데이터의 종단 간 통신을 관리하고 신뢰성 있는 데이터 전송을 제공하는 것이 목적입니다. 데이터를 **세그먼트** 단위로 분할하고, 오류 제어와 프름 제어를 수행하여 신뢰성 있는 데이터 전달을 보장합니다(TCP).  
포트 번호를 사용하여 특정 프로세스나 애플리케이션을 식별합니다.

**네트워크 계층**

데이터 패킷의 전달 경로를 설정하고 여러 네트워크를 거쳐 목적지까지 **패킷**을 전송하는 것이 주된 목적입니다. 패킷 라우팅과 흐름 제어를 담당하여 최적의 경로를 선택하고 데이터를 전송합니다.   
IP 주소를 사용하여 호스트를 식별하고 패킷을 목적지까지 전달합니다. 패킷의 헤더에는 출발지 IP 주소와 목적지 IP 주소가 포함되어 있어 각 호스트를 고유하게 식별하고 패킷을 올바른 목적지로 전달할 수 있게 합니다.

</div>
</details>

<details> 
<summary><h3>L3 Switch와 Router의 차이</h3></summary>
<div markdown="1">

**L3 스위치**

2계층과 3계층의 기능을 모두 갖춘 장비로, 보통의 스위치처럼 MAC 주소를 기반으로 데이터를 가장 적합한 포트로 전달합니다(2계층). 동시에 IP 주소를 기반으로 패킷을 전달할 수 있는 기능도 있습니다(3계층). 즉, L3 스위치는 IP 주소를 확인하여 패킷이 어디로 가야 할 지 경로를 결정합니다. 이 과정도 하드웨어 기반(ASIC, 라우팅 테이블을 참조하여 패킷이 가야 할 포트를 결정하는 칩)으로 매우 빠르게 수행됩니다.    
L3 스위치는 빠른 속도로 라우팅을 처리하면서도 스위칭 기능을 제공하기 때문에 대규모 네트워크에서 효율적으로 사용됩니다.

**라우터**

네트워크 계층의 기능을 갖춘 장비로 소프트웨어 기반으로 라우팅을 수행합니다. 패킷의 헤더 정보를 분석하여 라우팅 테이블을 참조하고 패킷을 전달합니다. 네트워크 간의 데이터를 전송하는 데 중점을 둔 장비입니다.


</div>
</details>

<details> 
<summary><h3>각 Layer에서 패킷을 명칭하는 방법</h3></summary>
<div markdown="1">

물리 계층에서는 데이터 비트(bit) 단위로 전송됩니다.  
데이터 링크 계층에서는 **프레임**(frame) 단위로 전송됩니다.    
네트워크 계층에서는 **패킷**(packet) 단위로 전송된다. 헤더와 페이로드(data)가 포함됩니다.    
전송 계층에서는 **세그먼트**(segment) 단위로 전송됩니다.전송 계층 프로토콜(TCP, UDP)에 따라 다양한 헤더와 페이로드로 구성됩니다.     
세션 계층과 표현 계층 에서는 데이터 단위에 대한 명확한 정의가 없습니다.    
응용 계층은 메시지(message) 또는 데이터 단위로 전송됩니다.

</div>
</details>

<details> 
<summary><h3>각각의 Header의 Packing Order</h3></summary>
<div markdown="1">

1. 응용 계층
   - 데이터: 사용자가 직접 상호작용하는 애플리케이션에서 생성된 데이터
   - 헤더: 데이터를 처리하는 프로그램에 따라 헤더가 붙여짐(HTTP의 요청메서드, URL, 헤더 정보 등)
2. 표현 계층
   - 데이터 : 응용 계층에서 전달된 데이터
   - 헤더: 추가적인 헤더는 없거나 특정 변환이 필요할 경우 부가 정보를 붙인다.
3. 세션 계층
   - 데이터: 표현 계층에서 넘어온 데이터
   - 헤더: 세션 관리와 관련된 정보(ex. 통신 세션의 ID)
4. 전송 계층
   - 데이터: 세션 계층에서 받은 데이터를 세그먼트로 나누고, 전송 계층 헤더를 추가
   - 헤더: 출발지 및 목적지 포트 번호, 세그먼트 순서 번호, 오류 검출을 위한 체크섬 정보
5. 네트워크 계층
   - 데이터 : 전송 계층에서 전달된 세그먼트가 패킷으로 감싸진다.
   - 헤더: 출발지 IP 주소, 목적지 IP 주소, 패킷의 생존 시간(TTL), 라우팅 정보 등
6. 데이터 링크 계층
   - 데이터 : 네트워크 계층에서 전달된 패킷이 프레임으로 감싸진다.
   - 헤더: 출발지 및 목적지의 mac 주소, 프레임 시작과 끝을 표시하는 정보, 오류 검출을 위한 FCS 등
7. 물리 계층
   - 데이터: 데이터 링크 계층에서 전달된 프레임이 물리적 신호로 변환되어 전송
   - 헤더: 없음

</div>
</details>

<details> 
<summary><h3>ARP</h3></summary>
<div markdown="1">

네트워크에서 **IP 주소를 이용해 통신하려 할 때 해당 장치의 MAC 주소를 알아내기 위한 방법**입니다. 송신자는 수신자의 IP 주소는 알고 있지만 실제 데이터를 전송하기 위해 필요한 수신자의 MAC 주소를 모릅니다. 이때 ARP가 사용됩니다. 송신자와 수신자가 처음 통신하거나 캐시가 비워졌을 때 ARP가 사용됩니다.

### ARP 작동 과정

1. 송신자가 수신자의 MAC 주소를 알아내기 위해 네트워크에 연결된 모든 컴퓨터에게 **ARP 요청**을 합니다(**브로트 캐스트**)
2. 네트워크에 있는 모든 컴퓨터가 이 요청을 받고, 수신자의 컴퓨터만 송신자에게 **ARP 응답**을 보냅니다. 이때 MAC 주소를 포함한 응답을 송신자에게 보냅니다.
3. 송신자는 수신자로부터 받은 MAC 주소를 캐시해두고 이후에 수신자에게 데이터를 보낼 때 이 MAC 주소를 사용하여 데이터를 전달합니다.
   - 이때 운영 체제 내에서 관리되며 IP 주소와 해당 MAC 주소의 쌍을 일시적으로 저장해두는 테이블인 ARP 캐시라는 메모리 공간에 저장됩니다.

</div>
</details>

# **3-Way Handshake**

네트워크 프로토콜에서 연결을 위해 사용하는 절차이며 TCP 프로토콜에서 사용합니다. 즉, 클라이언트와 서버 간에 신뢰성 있는 연결을 설정하기 위해 사용합니다.

<img src="https://github.com/user-attachments/assets/0e73daaa-e2bc-4544-b20d-f9032a993155" width="50%" />

### 1단계

클라이언트가 서버에 연결 요청을 보내는 단계입니다.  
클라이언트는 SYN 패킷을 서버에 보냅니다. 이 패킷에는 클라이언트의 초기 순서 번호(ISN)가 포함되어 있습니다. SYN 패킷은 클라이언트의 연결 요청을 나타냅니다.

### 2단계

서버가 연결 요청을 수락하고 클라이언트에 응답합니다.    
서버는 SYN-ACK 패킷을 클라이언트에 보냅니다. 이 패킷에는 서버의 초기 순서 번호와 클라이언트의 ISN에 1을 더한 값이 포함되어 있습니다. SYN-ACK 패킷은 클라이언트의 연결 요청을 수락하고 서버의 초기 순서 번호를 알려줍니다.

### 3단계

클라이언트는 ACK 패킷을 서버에 보냅니다. 이 패킷에는 서버의 초기 순서 번호에 1을 더한 값이 들어있습니다. ACK 패킷은 서버의 응답을 확인하고 클라이언트에서 서버로의 연결이 수립되었음을 알려줍니다.

<details> 
<summary><h3>ACK, SYN 같은 정보 전달 방식</h3></summary>
<div markdown="1">

패킷의 헤더에 담겨 전송됩니다.

</div>
</details>

<details> 
<summary><h3>2-Way Handshaking 를 하지 않는 이유</h3></summary>
<div markdown="1">

호스트 입장에서 연결을 신뢰하려명 아래 사항이 모두 확인 되어야 합니다.

1. 상대방에게 패킷을 전달할 수 있다.
2. 상대방으로부터 패킷을 전달 받을 수 있다.

2-way-handshaking을 한다면 일부만 확인 됩니다. 클라이언트는 서버의 ACK를 통해 1과 2를 통해 2는 확인할 수 있어도 자신의 ACK로는 1을 확인할 수 없습니다. 따라서 이는 신뢰할 수 없는 연결이라고 할 수 있습니다.


</div>
</details>

<details> 
<summary><h3>두 호스트가 동시에 연결을 시도했을 때 통신 연결 수행 방법</h3></summary>
<div markdown="1">

두 호스트가 동시에 서로에게 TCP 연결을 시도하는 상황을 **동시 개방**이라고 합니다.

**동시 개방 과정**

1. 양쪽에서 SYN 패킷 전송
2. 양쪽에서 SYN-ACK 패킷 수신 및 전송
3. 양쪽에서 ACK 패킷 전송

두 호스트가 동시에 연결을 시도해도 TCP는 이를 처리할 수 있는 메커니즘을 가지고 있기 때문에 두 호스트 간에 정상적으로 연결이 수립됩니다. 양쪽 모두 상대방의 SYN-ACK 패킷을 수신하고 응답함으로써 서로의 연결 요청을 인식하고 연결을 설정할 수 있습니다.

</div>
</details>

<details> 
<summary><h3>SYN Flooding 에 대해 설명</h3></summary>
<div markdown="1">

네트워크 공격 기법 중 하나로 공격자가 대상 서버에 대량의 SYN 패킷을 보내고 ACK 패킷을 수신하지 않기 때문에 서버는 점차적으로 자원을 고갈시키게 되며, 실제 유효한 연결 요청을 처리할 수 없는 상태가 되게 만드는 방법입니다.   
대상 서버가 클라이언트에서 SYN+ACK 패킷을 전달하고 서버는 클라이언트의 접속을 허용하기 위해 RAM에 일정 공간을 확보해 두는데, ACK 패킷을 안 보내게 되면 서버는 클라이언트의 연결을 받아들이기 위해 RAM 공간을 점점 더 많이 확보해 둔 상태에서 대기합니다. 이 때문에 서버의 자원을 점차적으로 고갈시키게 됩니다.

</div>
</details>

<details> 
<summary><h3>www.github.com을 브라우저에 입력하고 엔터를 쳤을 때, 네트워크 상 어떤 일이 일어나는지</h3></summary>
<div markdown="1">

1. DNS 조회

   브라우저는 먼저 입력한 도메인 이름인 [`www.github.com`](http://www.github.com) 을 IP 주소로 변환하기 위해 DNS 서버에 DNS 조회 요청을 보냅니다. 이 요청은 도메인 이름과 매핑된 IP 주소를 찾기 위한 작업입니다.

2. DNS 응답

   DNS 서버는 도메인 이름에 대한 IP 주소를 포함한 응답을 브라우저에게 보냅니다.

3. TCP 연결 설정

   브라우저는 얻은 IP 주소를 사용하여 해당 서버와의 TCP 연결을 설정합니다. 이 단계에서 3-Way-Handshake 과정이 발생합니다.

4. HTTP 요청

   TCP 요청이 수립되면 브라우저는 HTTP 요청을 생성하여 해당 서버로 보냅니다.

5. 서버 응답

   서버는 브라우저의 HTTP 요청을 받고 요청에 따라 필요한 정보를 찾거나 처리한 후 HTTP 응답을 보냅니다. 이 응답은 웹 페이지의 HTML, CSS, JavaScript 등의 리소스와 함께 전송됩니다.

6. 리소스 다운로드

   브라우저는 서버로부터 받은 HTTP 응답을 해석하고 필요한 리소스를 다운로드합니다.

7. 웹 페이지 렌더링

   다운로드 된 리소스를 기반으로 브라우저는 웹 페이지를 렌더링하고 사용자에게 보여줍니다.


</div>
</details>

<details> 
<summary><h3>DNS 쿼리를 통해 얻어진 IP가 가리키는 곳</h3></summary>
<div markdown="1">

사용자가 요청한 도메인 이름이 실제로 호스팅 되고 있는 서버의 위치를 가리킵니다.


</div>
</details>

<details> 
<summary><h3>Web Server와 Web Application Server의 차이</h3></summary>
<div markdown="1">

웹 서버는 클라이언트로부터 HTTP 요청을 받아 정적인 웹 페이지와 파일을 제공하는 역할을 합니다. 반면 웹 애플리케이션 서버는 동적인 웹 애플리케이션을 실행하고 관리하는 역할을 합니다. 웹 서버와 웹 애플리케이션 서버는 보통 함께 사용되며 웹 애플리케이션 서버는 웹 서버와 통신하여 필요한 동적인 처리를 수행합니다.


</div>
</details>

<details> 
<summary><h3>URL, URI, URN의 차이</h3></summary>
<div markdown="1">

URL은 리소스의 위치를 식별하는 데 사용되는 개념이며 URI는 리소스를 고유하게 식별하기 위한 일반적인 개념입니다. URN은 리소스의 이름을 식별하는 데 사용됩니다. URL은 URI의 하위 집합으로 볼 수 있으며 URN은 URI의 한 유형입니다.

</div>
</details>

# **DNS**

### 개념

도메인 이름을 숫자로 된 IP 주소로 변환해 주는 시스템입니다. 인터넷 상의 모든 장치는 IP 주소를 통해 통신하지만 사람이 기억하기 쉬운 도메인 이름을 사용하기 위해 DNS가 필요합니다. DNS는 전 세계적으로 분산된 데이터베이스 시스템으로 구성되어 있으며 각 도메인 이름에 해당하는 IP 주소를 저장하고 있습니다.

### 목적

1. 도메인 이름의 IP 주소 변환
2. 인터넷 자원의 관리
3. 분산 시스템의 신뢰성

   DNS는 전 세계적으로 분산된 시스템이기 때문에 특정 서버에 문제가 생겨도 다른 서버가 요청을 처리할 수 있습니다.

### 동작 방식

1. **도메인 이름 요청**

   사용자가 웹 브라우저에 도메인 이름을 입력하면 이 도메인 이름을 IP 주소로 변환해야 합니다. 이 과정에서 컴퓨터는 먼저 자신의 로컬 DNS 캐시에서 해당 도메인의 IP 주소를 찾습니다. 만약 캐시에 없다면 DNS 쿼리를 통해 DNS 서버에 요청을 보냅니다.

2. **DNS 쿼리 과정**

   로컬 DNS 서버는 요청된 도메인 이름의 IP 주소를 찾기 위해 여러 단계의 DNS 서버를 거쳐가며 쿼리를 진행합니다.

    1. **루트 DNS 서버**

       도메인 이름의 최상위 수준(ex. `.com` `.org`)에 대한 정보를 알고 있는 서버입니다. 로컬 DNS 서버가 처음으로 이 서버에 요청을 보냅니다.

    2. **TLD(Top-Level Domain) 서버**

       특정 최상위 도메인(`.com`)에 대한 정보를 관리하는 서버입니다. 루트 서버에서 받은 요청을 TLD 서버로 전달합니다.

    3. **권한 있는 DNS 서버**

       실제 도메인 이름에 대한 IP 주소를 가지고 있는 서버입니다. TLD 서버는 최종적으로 이 권한 있는 DNS 서버로 요청을 안내합니다.

3. **IP 주소 반환**

   권한 있는 DNS 서버가 요청된 도메인 이름에 대한 IP 주소를 로컬 DNS 서버에 반환합니다. 로컬 DNS 서버는 이 정보를 요청한 사용자 컴퓨터에 전달합니다.

4. **캐싱**

   로컬 DNS 서버와 사용자 컴퓨터는 받은 IP 주소를 캐시에 저장합니다. 이후 동일한 도메인 이름에 대한 요청이 있을 때 캐시에서 바로 IP 주소를 가져와 더 빠르게 응답할 수 있습니다.

5. **서버 접속**

   사용자 컴퓨터는 DNS 쿼리를 통해 얻은 IP 주소를 사용해 해당 서버에 접속하여 웹 페이지를 불러오거나 네트워크 서비스를 이용합니다.


> 로컬 DNS 서버: 인터넷 서비스 제공업체(ISP) 나 회사 또는 조직 내에 위치
>

> 캐싱한 IP 주소: 사용자 컴퓨터의 메모리에 저장됩니다.
>

<details> 
<summary><h3>DNS는 몇 계층 프로토콜</h3></summary>
<div markdown="1">

애플리케이션 계층 프로토콜입니다. DNS는 도메인 이름을 IP 주소로 변환하는 애플리케이션 계층의 기능을 수행합니다. DNS는 사용자와 직접 상호작용하는 프로그램에서 사용됩니다.

</div>
</details>

<details> 
<summary><h3>UDP와 TCP 중 사용하는 프로토콜</h3></summary>
<div markdown="1">

UDP를 주로 사용합니다. DNS가 UDP를 사용하는 이유는 속도가 빠르고 네트워크 오버헤드가 적으며 DNS 쿼리와 응답의 데이터 크기가 작기 때문입니다.

</div>
</details>

<details> 
<summary><h3>DNS Recursive Query, Iterative Query</h3></summary>
<div markdown="1">

DNS 질의 방식을 나타내는 용어로 이 두 가지 방식은 DNS 클라이언트가 DNS 서버에게 도메인 이름에 대한 IP 주소를 요청할 때 사용되는 방법을 설명합니다.

**Recursive Query**

클라이언트가 도메인 이름에 대한 IP 주소를 요청하면 DNS 서버가 클라이언트를 대신하여 모든 과정을 처리하고 최종 결과를 제공하는 방식입니다. 클라이언트는 DNS 서버로부터 직접 IP 주소를 받으므로 복잡한 질의 과정을 알 필요가 없습니다.

**Iterative Query**

DNS 서버가 클라이언트의 질의를 받아 해당 도메인에 대한 권한을 가진 상위 DNS 서버에게 반복적으로 질의하여 IP 주소를 찾는 방식입니다. 클라이언트는 이 반복적인 질의를 통해 IP 주소를 찾을 수 있게 됩니다.


</div>
</details>

<details> 
<summary><h3>DNS 쿼리 과정에서 손실이 발생할 경우 처리 방법</h3></summary>
<div markdown="1">

일정 시간 동안 응답을 받지 못한다면 타임아웃이 발생합니다. 타임아웃이 발생하면 클라이언트 측에서 쿼리를 재전송하거나 다른 DNS 서버에 요청을 보낼 수 있습니다.

</div>
</details>

<details> 
<summary><h3>캐싱된 DNS 쿼리가 잘못 될 경우 에러 보장 방법</h3></summary>
<div markdown="1">

DNS 캐시에 저장된 데이터는 TTL 값을 가지고 있습니다. TTL 이 만료되면 캐시된 데이터는 자동으로 삭제되고 새로운 DNS 쿼리가 수행되어 최신 정보를 가져오게 됩니다.     
네트워크 관리자가 캐시된 DNS 정보를 수동으로 삭제할 수 있습니다. 이후 DNS 요청 시 최신 정보를 얻을 수 있습니다.    
만약 캐시된 IP 주소를 사용해 접속을 시도했으나 실패할 경우 시스템은 캐시된 데이터를 무시하고 DNS 서버에 실시간 쿼리를 다시 보낼 수 있습니다.

</div>
</details>

<details> 
<summary><h3>DNS 레코드 타입 중 A, CNAME, AAAA의 차이</h3></summary>
<div markdown="1">

**A 레코드**   
도메인 이름을 IPv4 주소로 매핑하는 데 사용합니다.

**CNAME 레코드**  
도메인의 별칭을 지정하는 데 사용합니다. CNAME 레코드를 통해 도메인 이름을 다른 도메인 이름으로 매핑할 수 있습니다.

**AAAA 레코드**   
도메인 이름을 IPv6 주소로 매핑하는 데 사용합니다.

</div>
</details>

<details> 
<summary><h3>hosts 파일의 역할과 DNS와 비교하였을 때 우선순위</h3></summary>
<div markdown="1">

호스트 파일은 로컬 시스템에 직접 도메인 이름과 IP 주소를 매핑하는 데 사용되는 정적인 파일입니다. DNS는 전 세계적으로 분산된 네임 서버 시스템을 사용하여 도메인 이름을 IP 주소로 해석하는 동적인 매커니즘입니다. 호스트 파일은 작은 규모의 로컬 환경에서 사용되는 반면, DNS는 인터넷과 네트워크 환경에서 광범위하게 사용됩니다.  
호스트 파일은 운영체제의 특정 디렉터리에 위치하며 DNS 쿼리가 발생하기 전에 먼저 참조됩니다.

</div>
</details>


# **Stateless와 Connectionless**

## Stateless

### 개념

시스템이나 프로토콜이 상태 정보를 유지하지 않는 것을 의미합니다. 즉, 클라이언트와의 이전 상호 작용에 대한 정보를 서버나 시스템이 저장하지 않는다는 뜻입니다.

### 특징

- 각 요청은 다른 요청과 독립적으로 처리됩니다.
- 상태 정보를 저장하지 않으므로 서버의 부하를 줄일 수 있습니다.
- 서버가 상태 정보를 유지하지 않기 때문에 여러 서버에 부하를 분산하기가 쉽습니다.

## Connectionless

### 개념

데이터 전송 방식에서 연결을 설정하지 않고 데이터를 전송하는 것을 의미합니다. 즉, 송신자와 수신자 간에 사전 연결 설정 없이 데이터를 보내고 받는다는 뜻입니다. 이때의 Connectionless는 UDP의 의미보다는 HTTP가 Stateless 구조를 채택하고 있다는 점을 연장되어 설명하는 의미로 받아들일 수 있습니다. 매번 TCP 연결을 맺고 끊어 연결을 유지하지 않는 것을 Connectionless입니다.

### 특징

- 사전에 연결을 설정하지 않아 각 데이터 패킷은 독립적으로 전송되며 목적지에 도달할 때까지 경로가 결정됩니다.
- 사전 연결 설정이 필요 없기 때문에 빠르게 전송할 수 있습니다.
- UDP 기반이라면 데이터가 손실되거나 순서가 뒤바뀔 수 있습니다.

<details> 
<summary><h3>HTTP는 Stateless 구조를 채택 이유</h3></summary>
<div markdown="1">

HTTP는 웹의 기본 설계 철학인 단순성, 유연성, 확장성을 따르고 있습니다. 무상태 구조는 이러한 원칙에 부합하며 웹의 빠른 성장과 대규모 확장을 가능하게 했습니다.

</div>
</details>

<details> 
<summary><h3>Connectionless의 성능 해결 방법</h3></summary>
<div markdown="1">

서버가 클라이언트의 상태를 유지하지 않고 각 요청을 독립적으로 처리할 때 발생할 수 있는 성능 문제를 해결하기 위해 다음과 같은 방법이 사용됩니다.

1. HTTP Keep-Alive

   기본적으로 HTTP는 하나의 요청-응답 후에 TCP 연결을 종료합니다. 그러나 HTTP-Alive 옵션을 사용하면 클라이언트와 서버가 TCP 연결을 유지하여 여러 요청-응답을 처리할 수 있습니다. 이를 통해 연결 설정과 해제 과정에서 발생하는 오버헤드를 줄여 성능을 향상시킬 수 있습니다.

   웹 페이지를 로드할 때 여러 리소스를 효율적으로 가져올 수 있습니다. 특히 이미지, CSS, JS 파일 등 여러 개의 리소르를 한 번에 불러와야 할 때 유용합니다.

2. HTTP/2 와 HTTP/3

   HTTP2는 단일 연결을 통해 여러 요청과 응답을 동시에 처리할 수 있습니다.

   HTTP/3은 UDP 기반의 QUIC 프로토콜을 사용하여 연결 설정 시간을 줄이고 패킷 손실 시에도 빠르게 데이터를 복구할 수 있습니다.

3. 캐싱

   HTTP 응답을 캐시하여 동일한 리소스에 대한 반복 요청 시 서버에 다시 요청하지 않고 로컬 캐시나 중간 캐시 서버에서 데이터를 가져오도록 할 수 있습니다.


</div>
</details>

<details> 
<summary><h3>TCP의 keep-alive와 HTTP의 keep-alive의 차이</h3></summary>
<div markdown="1">

**TCP의 Keep-Alive**

TCP 연결이 여전히 유효한지 확인하기 위해 주기적으로 소량의 데이터를 전송하는 메커니즘입니다. 장기적인 비활성 연결을 유지하거나 네트워크 오류로 인해 끊어진 연결을 감지하는 데 사용합니다.

**HTTP의 Keep-Alive**

HTTP 요청과 응답을 하나의 TCP 연결을 통해 여러 번 주고받을 수 있도록 하는 메커니즘입니다. 연결 설정과 해제에 따른 오버헤드를 줄이고 성능을 향상시킬 수 있습니다.

</div>
</details>