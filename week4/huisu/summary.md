## IP

### IP

- Internet Protocol의 약자로 기기간 네트워크 통신을 할 때 쓰는 프로토콜
- IPv4
    - 32bit
    - 3자리 숫자가 4마디로 표기되는 방식
    - 각 마디는 옥탯
    - 예를 들어 192.168.123.123은 11000000.10101000.1111011.1111011로 표시
    - IPv4는 한 옥탯당 256개의 수를 나타낼 수 있어서 256^4 = 4,294,967,296개, 약 43억개의 IP 주소를 만들 수 있음
- IPv6
    - IPv4 하나로는 수용할 수 없을 만큼 많은 IP 주소가 필요해짐
    - 128bit
    - 16bit씩 8자리로 표현되며 각 자리는 콜론으로 구분
    - 라우터의 부담을 줄이고 네트워크 부하를 분산시킴
    - 지리적 제한이 없고 보안을 염두에 두고 구축함

### IP MAC

- Media Access Control
- 물리적 네트워크 세그먼트의 통신을 위해 네트워크 인터페이스에 할당된 고유 식별자
- 네트워크에 연결된 장치에 할당되는 고유 식별자
- 고유한 하드웨어 주소
- 데이터 링크 주소
- 변경 불가능한 물리적 주소
- 제조 시 장치에 하드코딩

### Checksum

- IP
    - 헤더에서 체크섬 값을 제외한 모든 값을 더해 체크섬 값과 비교
- TCP
    - IP 헤더에서 뽑아낸 의사 헤더를 만들어 낸 다음, 의사 헤더와 TCP 세그먼트의 값을 16bit로 끊은 뒤 모두 더함
    - 체크섬과 더해 1이라면 정상 아니라면 비정상으로 판단

## OSI 7

![https://cf-assets.www.cloudflare.com/slt3lc6tev37/6ZH2Etm3LlFHTgmkjLmkxp/59ff240fb3ebdc7794ffaa6e1d69b7c2/osi_model_7_layers.png](https://cf-assets.www.cloudflare.com/slt3lc6tev37/6ZH2Etm3LlFHTgmkjLmkxp/59ff240fb3ebdc7794ffaa6e1d69b7c2/osi_model_7_layers.png)

### Application Layer (data)

![https://cf-assets.www.cloudflare.com/slt3lc6tev37/2rcDKpr4WLqoyAZ7GDKkyJ/7cab96402de7ac5465b86e617da3da4e/osi_model_application_layer_7.png](https://cf-assets.www.cloudflare.com/slt3lc6tev37/2rcDKpr4WLqoyAZ7GDKkyJ/7cab96402de7ac5465b86e617da3da4e/osi_model_application_layer_7.png)

- 사용자의 데이터와 직접 상호작용 하는 계층
- 소프트웨어가 사용자에게 의미 있는 데이터를 제공하기 위해 프로토콜과 데이터를 조작하는 역할
- HTTP, SMTP

### Presentation Layer (data)

![https://cf-assets.www.cloudflare.com/slt3lc6tev37/19L86neKKT8srUkOSe4rf7/ff4c91c94a1790651df7b48433913f59/osi_model_presentation_layer_6.png](https://cf-assets.www.cloudflare.com/slt3lc6tev37/19L86neKKT8srUkOSe4rf7/ff4c91c94a1790651df7b48433913f59/osi_model_presentation_layer_6.png)

- 데이터를 준비해 애플리케이션 계층이 사용할 수 있도록
- 데이터의 변환, 암호화, 압축

### Session Layer (data)

![https://cf-assets.www.cloudflare.com/slt3lc6tev37/29mRrgK22AqJVlg2MMlD86/34d8f4071b6cc0d3b03c93f55e4d89b7/osi_model_session_layer_5.png](https://cf-assets.www.cloudflare.com/slt3lc6tev37/29mRrgK22AqJVlg2MMlD86/34d8f4071b6cc0d3b03c93f55e4d89b7/osi_model_session_layer_5.png)

- 두 기기 사이의 통신을 시작하고 종료하는 일
- 통신이 시작될 때부터 종료될 때까지의 시간을 세션
- 데이터 전송을 체크포인트와 동기화
    - 도중 연결이 끊어지면 마지막 체크포인트부터 재개

### Transport Layer (segment)

![https://cf-assets.www.cloudflare.com/slt3lc6tev37/3OlO75NcADGL3SmEADFDqd/723b8c7639c4e2e6b4febcbe7fd36e0e/osi_model_transport_layer_4.png](https://cf-assets.www.cloudflare.com/slt3lc6tev37/3OlO75NcADGL3SmEADFDqd/723b8c7639c4e2e6b4febcbe7fd36e0e/osi_model_transport_layer_4.png)

- 두 기기 간의 종단 간 통신
- 세그먼트라는 데이터 조각으로 분할하는 일 담당
- 흐름 제어 및 오류 제어의 기능
- TCP/UDP

### Network Layer (packet)

![https://cf-assets.www.cloudflare.com/slt3lc6tev37/3g2Hv0frHsql5SFauJL5EG/d8cede7b6a780e63413bd86de9eee7f9/osi_model_network_layer_3.png](https://cf-assets.www.cloudflare.com/slt3lc6tev37/3g2Hv0frHsql5SFauJL5EG/d8cede7b6a780e63413bd86de9eee7f9/osi_model_network_layer_3.png)

- 서로 다른 두 네트워크 간 데이터 전송을 용이하게 함
- 전송 계층의 세그먼트를 송신자의 장치에서 패킷이라는 단위로 더욱 세분화하고 조립
- 데이터가 표적에 도달하기 위한 최상의 물리적 경로 찾기 → 라우팅
- IP, ICMP, IGMP

### Data Link Layer (frame)

![https://cf-assets.www.cloudflare.com/slt3lc6tev37/3TLHavXiotb9ayyZFKECf3/9456d1c431cd71ceea7f4b407f076f11/data_link_layer_osi_model.png](https://cf-assets.www.cloudflare.com/slt3lc6tev37/3TLHavXiotb9ayyZFKECf3/9456d1c431cd71ceea7f4b407f076f11/data_link_layer_osi_model.png)

- 동일한 네트워크에 있는 두 개의 장치 간 데이터 전송
- 네트워크 계층에서 패킷을 가져와 프레임이라고 불리는 더 작은 조각으로 세분화

### Physical Layer (bit)

![https://cf-assets.www.cloudflare.com/slt3lc6tev37/1HQ1W5P4XAinIdM37DTu4U/900ccdceda346baf03ce8b9f977d2974/osi_model_physical_layer_1.png](https://cf-assets.www.cloudflare.com/slt3lc6tev37/1HQ1W5P4XAinIdM37DTu4U/900ccdceda346baf03ce8b9f977d2974/osi_model_physical_layer_1.png)

- 케이블이나 스위치 등 데이터 전송과 관련된 물리적 장비 포함
- 1과 0의 문자열인 비트스트림으로 변환되는 계층

### ARP

- Address Resolution Protocol
- 네트워크 상에서 IP주소를 MAC 주소로 대응시킴
- IP는 고정적이지 않기 때문에 항상 해당 단말을 가리키는 고유한 값인 MAC 주소까지 알아내고 소통

## 3-Way Handshake

### 3-way Handshake

![https://velog.velcdn.com/images/devharrypmw/post/b51e0e6d-be93-418c-9429-3ac6d9227a40/image.gif](https://velog.velcdn.com/images/devharrypmw/post/b51e0e6d-be93-418c-9429-3ac6d9227a40/image.gif)

- 클라이언트에서 서버에 연결 요청을 하기 위해 SYN 데이터 전송
- 서버에서 해당 포트가 listen() 상태로 SYN 데이터를 받고 SYN_RCV로 상태가 변경
- 요청을 정상적으로 받았다는 대답 ACK과 클라이언트도 포트를 열어 달라는 SYN를 같이 전송
- 클라이언트에서는 ACK + SYN을 같이 받은 후 established로 상태를 변경하고 ACK 전송
- ACK를 받은 서버는 상태가 established로 변경

### SYN Flooding

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://t1.daumcdn.net/cfile/tistory/9933693359901D660A](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://t1.daumcdn.net/cfile/tistory/9933693359901D660A)

- SYN (서버 열려 있나요?) → SYN + ACK (열려 있습니다 응답 후 클라이언트를 받을 메모리 확보) → ACK (서버에 연결할게요)의 과정
- 1번 과정만 반복되고 3번 과정이 없다면 서버는 클라이언트의 연결을 받아들이기 위해 메모리 공간을 점점 더 많이 확보해 둔 상태에서 대기
- 방어 방법 1 SYN Cookie
    - 방화벽 단에서 SYN을 먼저 받고 SYN Cookie를 포함한 SYN + ACK를 보내는 방법
    - 일정 시간 동안 SYN Cookie에 대한 정상적인 응답 패킷이 들어오지 않으면 방화벽에서 차단
- 방어 방법 2 SYN Proxy
    - 방화벽 단에서 정상적인 3-Way-Handshake 과정이 이루어지면 그 연결을 서버에게 재현시켜 주는 방식

### Web Server & Web Application & Web Application Server

- 최초의 웹에는 데이터에 따라 바뀌는 응답이라는 개념이 없음
- **Web Server:** 요청에 해당하는 파일을 그저 돌려주는 역할
    - 물리적 서버에서 어떤 파일을 돌려줘야 하는지 판단하고 검증해 주는 프로그램이자 프로세스
    - 대표적으로 Apache, NginX
- **Web Application:** 사용자가 요청하는 파일의 형식에 따라서 전달한 **요**청에 따라 알맞은 응답을 생성해내는 응용 소프트웨어
    - 사용자가 요청을 보냈을 때 Presentaion, Business Logic, Persistence Logic 등등을 수행하고 원하는 대답을 생성해내는 것
    - Web Application 자체에는 직접 http 요청을 들을 수는 없었기 때문에 Web Server에 종속적이었고, Web Server가 있어야지만 작동
- **Web Application Server:** Web Application에 Web Server 기능을 내장시켜서 바로 실행
    - 내가 자체적으로 요청을 듣고 그에 맞는 응답을 생성한 뒤 답변을 보내는 것

### 웹 동작 방식

- 사용자가 웹브라우저 검색창에 [www.google.com](http://www.google.com) 입력
- 웹 브라우저는 캐싱된 DNS 기록들을 통해 해당 도메인 주소와 대응하는 IP 주소 확인
- 주소가 없다면 웹브라우저가 HTTP를 사용하여 DNS에게 입력된 도메인 주소를 요청
- DNS가 웹브라우저에게 찾는 사이트의 IP 주소를 응답
- 웹브라우저가 웹서버에게 IP 주소를 이용하여  Html 문서를 요청
- 웹어플리케이션서버와 데이터베이스에서 웹페이지 작업을 우선적 처리
- 작업 처리 결과를 웹서버로 전송
- 웹서버는 웹브라우저에게 html 문서 결과를 응답

### URL, URI, URN

![https://velog.velcdn.com/images/younoah/post/b476852f-ffbf-44ed-b407-0449c69c65d3/image.png](https://velog.velcdn.com/images/younoah/post/b476852f-ffbf-44ed-b407-0449c69c65d3/image.png)

- URI (Uniform Resource Identifier)
    - [https://inpa.tistory.com/entry/WEB-🌐-URL-URI-차이](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-URL-URI-%EC%B0%A8%EC%9D%B4)

## DNS

### Domain Same System

- DNS는 7계층 UDP (신뢰성보다 속도가 중요)
- 192.168.10.2 라는 IP 주소는 사람이 일상적으로 사용하기에는 편하지 않음
- 도메인이라는 새로운 주소 체계가 나오게 되었고, 도메인과 그에 해당하는 IP 주소 매핑의 필요
- hosts 파일
    
    ```c
    # Comments start with a hash symbol
    
    # Localhost
    127.0.0.1   localhost
    ::1         localhost
    
    # Example mappings
    192.168.0.1 example.com
    ```
    
    - /etc/hosts
    - 위의 모습이 실제 hosts 파일의 모습
    - hosts 은 지금도 쓰고 있지만 과거 DNS가 없던 시절 오로지 저 파일 하나로만 도메인과 IP 주소 매핑
    - 점차 도메인이 많아지고 복잡해지면서 파일 하나로만 관리를 하기 어려워짐 → 사람을 대신해 도메인을 아이피에 매칭시켜 줄 서비스 등장
    - hosts 파일의 우선 순위가 더 높은 것이 디폴트값
- 도메인의 계층
    
    ```c
    www.google.com
    ```
    ![](https://velog.velcdn.com/images/morion002/post/4ef046e8-5b1b-44e5-aeb6-c626b89160f8/image.png)

    
    - 도메인은 .(Root) → com → google → www 이렇게 네 개의 계층으로 나뉘고 루트가 최상단

### DNS의 작동 과정

- DNS도 서버 - 클라이언트로 동작
- 클라이언트 (아이피를 알아내려는 호스트)가 서버한테 아이피 주소를 알려 달라고 함
- DNS의 작동에는 보통 두 가지의 네임서버가 협업
- Recursive Name Server
  ![](https://velog.velcdn.com/images/morion002/post/80e3247f-aff8-43c7-a7f1-582d5e990976/image.png)

    
    - 보통 ISP 업체에서 Recursive 네임서버 서비스 제공
- Authoritative Name Server
 ![](https://velog.velcdn.com/images/morion002/post/cbf6cb2b-7632-4f2c-86ad-b75eee31f911/image.png)

    
    - 실제 도메인에 대한 아이피 주소를 가지고 있는 서버

### DNS 질의 형태

- 재귀적 질의 (Recursive Query)
    - Recursive 네임 서버한테 질의를 날리고 그 질의를 받은 네임 서버가 다른 네임 서버한테 질의
    - 클라이언트는 재귀적 질의를 보내고 더 이상 질의를 보내지 않고 응답이 올 때까지 기다림
- 반복적 질의 (Iterative Query)
    - 원하는 답이 나올 때까지 계속 질의를 보내는 것
    - 질의에 대한 답이 왔는데 원하는 답이 아니라면 또 질의를 보냄

### DNS 작동 흐름

1. 클라이언트가 [www.google.com](http://www.sampledomain.com) 아이피 주소를 알려 달라고 recursive 네임 서버에게 질의
2. recursive 네임 서버는 최상위 계층인 .(루트) 네임 서버한테 com 네임서버의 아이피 주소가 뭔지 물어보고 루트 authoritative 네임서버가 응답
3. recursive가 받은 응답은 com 도메인을 관리하는 네임서버의 아이피 주소이고, 이 주소를 가진 서버와 소켓을 통해 이번엔 google 도메인을 관리하는 네임서버의 아이피 주소 질의
4. 역시 recursive 네임서버는 또 www 에 대한 서브 도메인을 관리하는 네임서버의 아이피 주소를 알려달라고 sampledomain 네임서버한테 요청
5. 드디어 www.sampledomain.com에 대한 아이피 주소를 알아내서 클라이언트에게 응답
6. 클라이언트는 알아낸 아이피 주소를 가지고 통신

### DNS

- DNS 는 도메인에 대한 아이피 주소를 알아내기 위해 사용하기 시작했지만, 기능이 점차 확장되어 도메인에 대한 아이피 주소 말고 추가 정보 제공
- 도메인 이름에 대한 텍스트 정보
- 대규모 트래픽을 처리하기 위해서 네임 서버 간에 주고받아야 하는 정보
- DNS에서 질의를 보낼 때 요청하는 데이터 타입을 레코드라고 함

| 레코드 | 내용 |
| --- | --- |
| CNAME | IP 주소가 아닌 다른 도메인 주소를 가리키는 레코드 |
| A | IPv4 주소 |
| AAAA | IPv6 주소 |
| MX | 해당 도메인의 서브 도메인으로 관리되는 서버 중 메일 서버의 주소(IPv4인지 IPv6인지는 서버의 설정에 따라서 알아서 온다고 함) |
| NS | 해당 도메인을 관리하는 네임서버의 아이피 주소(IPv4인지 IPv6인지는 서버의 설정에 따라서 알아서 온다고 함) |
| TXT | 해당 도메인과 연결된 텍스트 정보 (도메인에 대한 문자로 된 설명) |
| IXFR | 뒤에서 설명 |
| AXFR | 뒤에서 설명 |
| ANY | 모든 레코드 정보 다 주세여 |

## **Stateless와 Connectionless**

**Stateless와 Connectionless**

- HTTP는 Stateless 구조를 채택 이유
- Connectionless의 성능 해결 방법
- TCP의 keep-alive와 HTTP의 keep-alive의 차이

### **Stateless와 Connectionless**

- Stateless: 서버가 클라이언트의 이전 상태를 보존하지 않음
- Connectionless: 서버에 요청을 하고 응답을 받으면 바로 TCP/IP 연결을 끊음
- HTTP 는 두 상태를 채택함으로 인해 서버 과부하를 줄일 수 있음
- Stateless가 시스템 확장에 더 유리함
- Stateless한 경우 사용 가능한 모든 인스턴스 활용 가능

### Keep-Alive

- Keep-Alive를 활성화하면 하나의 TCP 연결을 여러 번 재사용하며 응답과 요청을 수행 가능
- 클라이언트와 서버 간 여러 요청을 단일 TCP 연결을 재사용하는 방식으로 처리하는 기능
- 단일 TCP 연결에서 여러 요청과 응답이 이루어지기 때문에 네트워크 지연 시간이 줄고 웹사이트 성능이 좋아짐

### HTTP Keep-Alive VS TCP Keep-Alive

- HTTP Keep-Alive: 최대 얼마 동안 connection을 유지할 것인가
- TCP Keep-Alive: connection을 지속적으로 얼마나 유지할 것인가
