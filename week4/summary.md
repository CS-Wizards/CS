IP 주소는 무엇이며, 어떤 기능을 하고 있나요?

1.IPv4와 IPv6의 차이
2.수많은 공유기에서는 고정 주소를 제공하는 기능의 동작 방법
3.IPv4를 사용하는 장비와 IPv6를 사용하는 같은 네트워크 내에서 통신하는 방법
4.IP가 송신자와 수신자를 정확하게 전송되는 것의 보장 유무
5.IPv4에서 수행하는 Checksum과 TCP에서 수행하는 Checksum의 차이
6.TTL(Hop Limit)
7.IP 주소와 MAC 주소의 차이
OSI 7계층

8.Transport Layer와, Network Layer의 차이
9.L3 Switch와 Router의 차이
10.Layer는 패킷 명칭
11.각각의 Header의 Packing Order
12.ARP
3-Way Handshake

13.ACK, SYN 같은 정보 전달 방식
14.2-Way Handshaking 를 하지 않는 이유
15.두 호스트가 동시에 연결을 시도했을 때 통신 연결 수행 방법
16.SYN Flooding 에 대해 설명
17.**www.github.com을 브라우저에 입력하고 엔터를 쳤을 때, 네트워크 상 어떤 일이 일어나는지**

18.DNS 쿼리를 통해 얻어진 IP가 가리키는 곳
19.Web Server와 Web Application Server의 차이
20.URL, URI, URN의 차이
DNS

21.DNS는 몇 계층 프로토콜
7계층 애플리케이션 계층에 속하는 프로토콜이다. 
애플리케이션 계층은 사용자와 직접 상호작용하는 프로토콜들이 위치하는 계층으로, DNS 외에도 HTTP, FTP, SMTP와 같은 다양한 프로토콜들이 있습니다.
22.UDP와 TCP 중 사용하는 프로토콜
DNS는 주로 UDP를 사용하지만, 데이터 크기나 보안의 필요성에 따라 TCP도 사용될 수 있습니다.
23.DNS Recursive Query, Iterative Query
Recursive queries의 경우에는 local Host가 naver.com에 대해 query를 보내면 Local DNS server가 root name server에 query를 보내고, root server는 자신의 server에 등록되어 있는지 검사한 다음 없으면 com 담당 서버에 요청을 한다. recursive하게 실제 domain name을 가지고 있는 server까지 query가 이동하여 IP 주소를 얻는 방법이다. 이러한 방법은 root server에 너무 큰 부담을 준다는 단점이 있다.

Iterative queries의 경우에는 Host가 naver.com에 대해 query를 보내면 Local DNS server가 root name server에 query를 보내 com담당 server의 주소를 return 받고, 다시 com 담당 server에 query를 보내 naver.com이 변환 요청을 보낸다. 이렇게 최종 IP 주소를 받을 때까지 요청 &응답을 계속해서 local name server가 반복하는 방법이다.
![image](https://github.com/user-attachments/assets/4553d766-88b7-40de-92fa-e363b096c85e)
24.DNS 쿼리 과정에서 손실이 발생할 경우 처리 방법
25.캐싱된 DNS 쿼리가 잘못 될 경우 에러 보장 방법
26.DNS 레코드 타입 중 A, CNAME, AAAA의 차이
27.hosts 파일의 역할과 DNS와 비교하였을 때 우선순위
Stateless와 Connectionless

28.HTTP는 Stateless 구조를 채택 이유
29.Connectionless의 성능 해결 방법
30.TCP의 keep-alive와 HTTP의 keep-alive의 차이
