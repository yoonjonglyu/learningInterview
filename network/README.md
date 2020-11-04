# Network

- [HTTP의 GET 과 POST](#http의-get-과-post)
- [TCP와 UDP의 비교](#tcp와-udp의-비교)
- [TCP 3-way-handshake](#tcp-3-way-handshake)
- [HTTP와 HTTPS](#http와-https)
- [DNS Round Robin 방식](#dns-round-robin-방식)

## HTTP의 GET 과 POST

> HTTP 프로토콜을 이용한 서버와의 통신 방식에는 크게 두가지 GET, POST방식이 있다.
> 일반적으로 GET 방식은 데이터를 가져오는데 사용되고, POST는 데이터를 변경하는데 사용된다. GET방식은 브라우저에서 caching된다.

1. GET : 요청 데이터가 HTTP REQUEST HEADER 부분의 URL에 담겨서 전송된다. 쿼리스트링(쿼리파라메터) URL 끝에 ?로 시작하여 =와 &으로 구분한다. 데이터 크기가 제한적이며, 보안상 URL에 다 노출되기에 적절하지않다.
2. POST : 요청 데이터가 HTTP REQUEST BODY 부분에 담겨서 전송된다. 데이터 크기가 상대적으로 크며 상대적으로 보안에 유리하다.(암호화 없이는 별차이없다.)

## TCP와 UDP의 비교

> UDP(사용자 데이터그램 프로토콜), TCP(전송제어 프로토콜)는 네트워크 전송 계층에서 사용하는 두가지 프로토콜  
> UDP는 TCP와 다르게 비연결형 프로토콜이다.

1. UDP(User Datagram Protocol) : 비연결형 프로토콜, IP 데이터그램을 캡슐화 해서 전송하는 방법과 연결설정 없이 보내는 방법을 제공, 흐름제어, 오류 제어 손상된 새그먼트 수신에 대한 재전송을 하지 않는다. 사전 설정이 필요없고 추후 해제가 필요 없는 서비스에 주로 사용된다.
2. UDP는 단순히 포트들을 이용해서 IP프로토콜에 대한 인터페이스를 제공하는 목적으로 DNS에서 사용된다.
3. TCP(Transmission Control Protocol) : UDP가 가진 신뢰성과 순차적인 전달을 보완한 프로토콜, 종단간 소켓이라는 종단점을 생성하여 바이트 스트림을 전송하는 방식, TCP의 연결 설정(Connection establishment)은 3-Way Handshake를 통해 행해진다. 모든 연결은 전이중(full-duplex), 점대점(point to point)방식이다.
4. 전이중 : 전송이 양방향으로 동시에 일어날 수 있다.
5. 점대점 : 각 연결이 정확히 2 개의 종단점을 가지고 있음을 의미한다. 
6. TCP 는 멀티캐스팅이나 브로드캐스팅을 지원하지 않는다.

## TCP 3-way-handshake

> TCP의 연결(Connection Establishment)은 3-way-handshake 방식으로 이루어지며, 연결해제(Connection Termination)는 4-way-handshake 방식으로 이루어진다.  
> SYN(Synchronize Sequence Number), ACK(Acknowledgement) 두가지 패킷이 연결에 쓰이고 거기다 FIN이라는 패킷이 더해져서 연결해제에 쓰인다.

1. 연결 : 클라이언트가 서버에 SYN(a) => 서버가 클라이언트에 ACK(a + 1) + SYN(B) => 클라이언트가 서버에 ACK(B + 1) 연결 성립
2. 해제 : 클라이언트가 FIN => 서버가 ACKTIMEOUT(데이터 전송 대기) => 또 서버가 FIN => 클라이언트가 ACK 연결 해제
3. TCP Header은 Code Bit(Flg big)라는 부분이 존재하며 해당 부분은 6Bit로 이루어져 있고 Urg-Ack-Psh-Rst-Syn-Fin 순서로 이진 방식으로 해당 패킹의 종류를 구분한다.
4. SYN 그중에서도 초기 SYN을 ISN은 0부터가 아닌 랜덤한 난수부터 시작 되며 그 이유는 서버측에서 패킷의 SYN을 보고 이전에 사용된 포트쌍을 사용시 이전 커넥션으로 인식하는 것을 방지하기 위해서이다.

## HTTP와 HTTPS

> HTTP는 평문 통신이며, 통신 상대를 확인하지 않고, 완전성을 증명하지 못허기에 보안에 취약하다.  
> HTTP 암호화에는 SSL(Secure Socket Layer) or TLS(Tranport Layer Security) 프로토콜을 조합해서 가능하다.  
> HTTPS는 HTTP의 통신 자체를 SSL or TLS 를 통해서 암호화 한 것이다. HTTPS(HTTP Secure) or HTTP over SSL이라 부른다.

1. HTTP는 평문 통신이기에 도청이 가능하다. TCP/IP 구조의 통신은 전부 통신 경로 상에서 엿볼 수 있어 패킷만으로 사용자의 인터넷 사용 내역을 체크할 수 있다.
2. HTTP는 통신 상대를 확인하지 않기에 위장이 가능하다. 어디서든 요청을 보낼 수 있고 그 요청에 응답을 보낸다. 요청과 응답을 주고 받는 대상이 누군지 확인하지 못하고, 상대가 허가 되었는지 확인 할 수 없고, 누가 요청을 보냈는지, 의미 없는 요청도 응답한다.(DoS 공격)
3. HTTP는 완전성을 증명 못하기에 변조가 가능하다. 완정성 : 정보의 정확성, 리퀘스트나 리스폰스가 발신된 후에 상대가 수신하는 사이에 누군가에 의해 변조되더라도 이 사실을 알 수 없다. 중간자 공격(Main-in-the-Middle)
4. 보안 방법으로 암호화를 쓸 수 있지만 확실히 보장되는게 아니다 그렇기에 SSL방식을 사용한다.
5. SSL은 인증, 암호화, 다이제스트 기능을 제공한다.
6. HTTP는 HTTP/1.1은 plaintext transfer protocol이었지만 HTTP/2는 binary transfer protocol로 변경되었고, 기존의 HTTPS가 가진 단점인 CPU, 메모리등의 리소스 소모가 비교적 많은 것이 어느 정도 해결되어서 사실상 단순 HTTP는 지양해야할 방법이 되었다.

### HTTPS

1. HTTP에 암호화, 인증, 완정성 보호를 더한 프로토콜
2. HTTPS 는 새로운 애플리케이션 계층의 프로토콜이 아니라는 것이다. HTTP 통신하는 소켓 부분을 SSL(Secure Socket Layer) or TLS(Transport Layer Security)로 대체하여서 HTTP가 TCP에 직접했다면 HTTPS는 SSL을 매개체로 TCP와 통신한다.
3. HTTPS 의 SSL 에서는 공통키 암호화 방식과 공개키 암호화 방식을 혼합한 하이브리드 암호 시스템을 사용한다. 공통키를 공개키 암호화 방식으로 교환한 다음에 다음부터의 통신은 공통키 암호를 사용하는 방식이다.
4. 통신할 때마다 암호화를 하면 많은 리소스를 소비하기 때문에 서버 한 대당 처리할 수 있는 리퀘스트의 수가 줄어들게 된다. 그렇기 때문에 민감한 정보를 다룰 때만 HTTPS 에 의한 암호화 통신을 사용했지만 HTTP가 2.0으로 버전업이 되면서 거의 의미가 없어진 단점이다.

## DNS Round Robin 방식

> 주소 요청에 대한 DNS 응답을 관리하여서 로드 분산 또는 로드 밸런싱 하는  기법.
> 복수개의 서버로 사용자 요청을 분산시킨다. 분산이 균등하지 않고 캐싱에 의한 문제, 서버에 장애가 발생했을시에 대한 대응 방법이 마땅하지않다.

1. DNS응답으로 로드 밸런싱 하는 기법의 문제로는 여러 서버가 필요하고 각 서버 마다 공인 IP가 필요하다.
2. 부하가 균등하게 분산되지 않아서 모바일(캐리어 게이트웨이 라는 프록시 서버를 경유)접속이나 pc접속이나 질의 결과가 일정시간 캐싱되므로 같은 서버로 접속된다.
3. 서버에 장애가 발생해도 확인 할 수 없고, 그저 단순히 DNS 연결해주는게 다이기에 유저들은 간혹 다운된 서버로 연결이 되기도 한다. DNS 라운드 로빈은 어디까지나 부하분산 을 위한 방법이지 다중화 방법은 아니므로 다른 S/W 와 조합해서 관리할 필요가 있다.
4. 이런 문제의 단점을 해소하기 위해서 DNS 스케쥴링 알고리즘이 존재한다.

### DNS 스케쥴링 알고리즘

1. WRR(Weighted Round Robin) : 각각의 서버에 가중치를 가미해서 분산 비율을 변경한다. (선택에 따른 스케쥴링) 성능이 높은 서버의 가중치를 높게 설정
2. Least Connection :  접속 클라이언트 수가 가장 적은 서버를 선택하는 방식, 로드 밸런서에서 커넥션수를 관리하거나 알려주는 것이 필요하다.