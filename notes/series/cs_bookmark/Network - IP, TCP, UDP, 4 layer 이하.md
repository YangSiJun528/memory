
## TCP Handshake

- 용어
	- SYNchronize(동기화)
	- ACKnowledgement(승인) - ISN에 1을 더한 값을 포함
	- ISN - Initial Sequence Number (초기 시퀀스 번호)
	- FIN: 더 이상 보낼 데이터가 없음
	- 기타: RST, PSH, URG

#### 3 Way Handshake - TCP 연결

1. Client: SYN - 클라이언트는 서버에게 동기화 요청을 보냄
2. Server: ACK + SYN - 서버는 동기화 요청의 응답과 본인의 동기화 요청을 보냄
3. Client: ACK - 클라이언트는 서버의 동기화 요청의 응답을 보냄

- TCP의 신뢰성을 위한 3 Way Handshake 특징
	- ISN을 사용해서 특정 요청이 어떤 SYN에 대한 ACK인지 확인 - 서로 (유효한) 요청을 한 번씩만 주고받음을 보장
	- 연결 도중 TIME OUT 시간동안 응답이 안오면 패킷 버리고 재시도, 해도 안되면 연결 끊음(포기)

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F781159a5-f60e-4c94-b815-c8abd8d73b12_1600x1442.png)


#### 데이터 송수신

- 데이터 송수신 과정
	- Data: 한 쪽에서 Data를 보냄
	- ACK: 받은 데이터에 대한 응답

- 특정 기간동안 ACK가 오지 않으면, 재전송 - 신뢰성있는 통신을 보장하기 위함
- TCP 버퍼 (Buffer): 송신/수신자가 보낼/받은 데이터를 일시적으로 저장하는 공간
- 흐름 제어 (Flow Control): 수신자의 버퍼 상태에 따라 송신자가 보내는 전송 속도를 조절
	- 버퍼가 다 차면 처리를 못함.
- 혼잡 제어 (Congestion Control): 네트워크 상태(라우터 병목이나 그런거) 를 감지하고 전송 속도 조절


#### 4 Way Handshake - TCP 해제

1. Client: FIN - 클라이언트가 더 이상 보낼 데이터가 없다는 의미의 FIN을 서버로 보냄.
2. Server: ACK - 서버는 FIN에 대한 응답(ACK)를 보냄. Half-Close 기법을 사용해서, 클라이언트는 이 이후부터 요청을 보내지 않고, 받기만 한다.
3. Server: FIN - 서버는 나머지 데이터를 (있다면) 전부 보내고 FIN을 클라이언트로 보냄
4. Client: ACK - 클라이언트는 FIN에 대한 응합(ACK)를 보냄.
	- 클라이언트는 서버로부터 받지 못한 데이터가 있을 수 있으므로 일정 기간 대기하고, 연결을 종료한다.
	- 서버는 연결을 바로 종료한다.
![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1e386fb3-7fc6-477a-aa50-c314a462a349_1600x1591.png)


- https://blog.bytebytego.com/p/everything-you-always-wanted-to-know
- https://www.geeksforgeeks.org/why-tcp-connect-termination-need-4-way-handshake/
- https://developer.mozilla.org/ko/docs/Glossary/TCP_handshake
- https://learn.microsoft.com/ko-kr/troubleshoot/windows-server/networking/three-way-handshake-via-tcpip
- https://evan-moon.github.io/2019/10/08/what-is-http3/?fbclid=IwAR1V1-yWjkzWEAqm_1OZfe_gtG05EuVo7WXXyVdEz_J0UHZBpGruU8PU0FY#3-way-handshake
- https://hojunking.tistory.com/107#Half-Close%20%EA%B8%B0%EB%B2%95-1
- https://www.youtube.com/watch?v=DC9FfKSgisg&list=PLXvgR_grOs1BFH-TuqFsfHqbh-gpMbFoy&index=27
- https://www.youtube.com/watch?v=K9L9YZhEjC0&list=PLXvgR_grOs1BFH-TuqFsfHqbh-gpMbFoy&index=26


## 세션과 소켓의 관계

어플리케이션 개발(7계층)에서 말하는 세션이랑은 다른 개념임. 

- 소켓: 네트워크 통신의 끝점, IP 주소와 포트 번호로 구성됨.
	- TCP/UDP 둘 다 사용함
- 세션: 두 소켓(하나의 소켓 쌍) 간의 연결 상태를 관리하는 논리적 연결. 
	- TCP만 사용, UDP는 커넥션을 맺지 않기 때문

- 단 쉬운코드 유튜브에서 TCP와 함꼐 설명하지 않는데, 내 생각에 저 강의자 분 준비하는거 보면 세션이라는 단어가 TCP 표준에 있었으면 분명 말 했을거 같음. 
	- GPT 피셜로는 TCP 연결이라는 표현이 더 알맞고, TCP 세션이라고 부르기도 한다고 함. 근데 정확한 건 잘 모르겠음.
	- 대충 사람들이 그렇게 부른다고 이해만 하고, 표준인지는 정확하지 않다고 알고 있으면 될 듯.

- https://community.cisco.com/t5/application-networking/difference-between-session-connections-socket/td-p/2417074
- https://www.quora.com/What-are-the-differences-between-a-session-and-a-socket
- https://www.youtube.com/watch?v=X73Jl2nsqiE&list=PLcXyemr8ZeoSGlzhlw4gmpNGicIL4kMcX&index=4
- https://www.youtube.com/watch?v=WwseO8l8rZc&list=PLcXyemr8ZeoSGlzhlw4gmpNGicIL4kMcX&index=5

## OSI 7 Layer와 TCP/IP Model

- OSI 7 Layer
	- 네트워크 기술을 계층 별로 모듈화한 표준.
	- 7개의 계층으로 이루어짐
- TCP/IP Model
	- 현대 많은 네트워크는 TCP/IP와 이더넷으로 구현되어 있다.
	- 사실 상 TCP/IP에 정의된 프로토콜들로 실제 네트워크가 동작한다고 봐도 됨.
	- 4개의 계층으로 이루어진다.

- 왜 계층을 분리하는가?
	- 네트워크 통신을 위해서 너무 많은 관심사(기능)이 필요함.
	- 역할을 분리하여 모듈화 하면, 유지보수나 문제를 해결하기 좋다.

- OSI 7 Layer는 아무것도 해주지 않는다.
	- 이론적인 부분이고, 실제로 네트워크가 동작하는 방식을 설명해주지는 못한다.
		- 쉬운코드 시리즈에서도 나오듯이, (OSI와는 별개지만) 표준대로 개발되는 것도 아니다.
	- 실제로 네트워크가 동작하는 건, 프로토콜들이고, 모든 프로토콜이 항상 어떤 계층에 존재한다고 명확하게 구분되는게 아니다.
		- 상위 계층이 하위 계층의 개념을 포함한다는 걸 이해하면 좋음.

- https://www.youtube.com/watch?v=XFYRbeqGEtg
	- "OSI 7계층은 사실 아무것도 해주지 않습니다"
- https://www.youtube.com/watch?v=oFKYzp6gGfc&list=PLcXyemr8ZeoSGlzhlw4gmpNGicIL4kMcX
	- 쉬운코드 시리즈
- https://www.youtube.com/watch?v=k1gyh9BlOT8&list=PLXvgR_grOs1BFH-TuqFsfHqbh-gpMbFoy
	- 널널한 개발자 네트워크 입문 시리즈 
- [네트워크 기초 개념](../../Computer%20Science/네트워크%20기초%20개념.md)

## IP와 TCP/UDP 간단하게

- IP 프로토콜: 다른 Host로 데이터(패킷) 전송. 본인의 IP에서 목표 IP로 이동.
	- 전송하는 것에만 집중하고 나머지(신뢰성, 검증)은 거의 없다고 봐야 함.
- TCP/UDP 프로토콜: IP 프로토콜을 기반으로 동작하는 프로토콜
	- Host의 (통신하는) 프로그램을 식별하기 위해 Port를 사용함.
	- TCP: 신뢰성 있는 통신을 위한 프로토콜.
		- 여러가지 프로토콜 규칙 (헤더, 송수신, handshake 등)이 있다.
		- 신뢰성 있게 하기 위해서 연결이 필요. 그래서 1:1 만 가능
	- UDP: IP 프로토콜 위에 별 규칙 없이 존재하는 프로토콜.
		- 자유롭게 커스텀 가능 (QUIC)
		- 연결이 필요하지 않으므로 1:N, N:M 가능.
		- 주로 통화나 스트리밍 같이 전송량이 많고 다수면서 신뢰성이 떨어져도 상관 없으면 사용.
			- 다만, QUIC 처럼 UDP 기반이지만, ACK를 보내서 신뢰성을 검증하는 경우도 있음.
			- 별 규칙이 없어서 IP와 TCP의 관계처럼 UDP를 상위 계층에서 커스텀 할 수 있다고 보면 됨.

- https://www.youtube.com/watch?v=oFKYzp6gGfc&list=PLcXyemr8ZeoSGlzhlw4gmpNGicIL4kMcX
	- 쉬운코드 시리즈
- https://www.youtube.com/watch?v=k1gyh9BlOT8&list=PLXvgR_grOs1BFH-TuqFsfHqbh-gpMbFoy
	- 널널한 개발자 네트워크 입문 시리즈 


## 네트워크와 인터넷, ISP

- 네트워크
	- 컴퓨터나 기기들이 데이터를 주고 받기 위해 구성된 유선/무선으로 연결된 통신 체계
- 인터넷
	- 여러 네트워크들의 집합
	- 모든 공개 네트워크에 접근 가능함
- ISP(Internet service provider)
	- 사람/기관이 인터넷을 사용할 수 있도록 인터넷 연결 서비스를 제공하는 업체.
	- 고성능 라우터와 스위치 등을 사용한 대규모 네트워크를 가지고 있음.
	- 1~3티어로 구성됨. 그 이하도 있을 수 있으나, 주로 2~3티어까지만 다루는 것 같음.
	- 티어 종류
		- 1티어
			- 전 세계 인터넷 트래픽을 직접 연결하는 네트워크를 소유하고 있다.
			- 다른 ISP와 트래픽 교환에 비용을 지불하지 않는다. 
				- 어차피 다른 ISP들도 다른 ISP와 통신을 위해 거쳐야 하기 때문
				- 글로벌 인터넷 백본(backbone, 중추)을 형성
			- AT&T, Verizon, NTT
		- 2티어
			- 1 티어 ISP로부터 인터넷 접속을 구입하며, 자체 네트워크도 소유하여 다른 ISP와 피어링(peering, 서로의 네트워크를 연결)을 통해 트래픽을 교환한다.
			- SK 브로드밴트, KT 등이 여기 속함. (한국 회사는 1티어 ISP 없음)
		- 3티어
			- 소규모 지역 ISP, 상위 티어로부터 인터넷 접속 비용을 지불한다.
			- 한국에는 크기가 작아서 없고, 미국같이 땅이 큰 경우 있는듯

- Host 간 통신 과정에서 일어나는 일 - 많은 내용 생략됨 주의
	- Host에서 다른 Host로 IP 프로토콜을 통해 전송
	- 라우터(3계층 장비)에서 3계층까지 디캡슐레이션해서 IP 프로토콜 패킷을 읽고 다른 라우터로 넘긴다.
		- 어느 순간에 ISP 네트워크로 진입한 패킷은 고성능 네트워크를 통해 빠르게 처리된다.
	- 패킷이 Host까지 도달하면 패킷을 디캡슐레이션해서 처리한다.

- https://www.youtube.com/watch?v=6l7xP7AnB64&list=PLcXyemr8ZeoSGlzhlw4gmpNGicIL4kMcX&index=2
	- 이거 말고 다른 강의들도 괜찮은거 있으면 추가하기
- https://en.wikipedia.org/wiki/Tier_1_network
- https://en.wikipedia.org/wiki/Tier_2_network
- https://en.wikipedia.org/wiki/Internet_service_provider


## LAN, WAN, NIC, MAC Addr, NAT << 정리 예정

- 주로 참고할 것
	- https://www.youtube.com/watch?v=oFKYzp6gGfc&list=PLcXyemr8ZeoSGlzhlw4gmpNGicIL4kMcX
	- https://www.youtube.com/watch?v=dsoAkoxZ13o
	- 검증용 대학 서적이나, 공신력 있는 다른 자료들도 있으면 좋을듯?
	- 내가 이전에 정리했던거 - [네트워크 기초 개념](../../Computer%20Science/네트워크%20기초%20개념.md) << 여기 있는데 걍 이거 봐도 될 듯?

## 네트워크 강의
- https://www.youtube.com/watch?v=dsoAkoxZ13o
	- 네트워크 기초 무료 강의 | 새내기 개발자들을 위한 필수 가이드 - 4시간짜리, 뭘 배워야 하는지 알기 좋을 듯?
- https://www.youtube.com/watch?v=oFKYzp6gGfc&list=PLcXyemr8ZeoSGlzhlw4gmpNGicIL4kMcX
	- 쉬운코드 네트워크 시리즈 - 중간에 강의가 끊긴게 아쉽긴 한데, 강의의 질 자체는 매우 좋다.
	- 특히 백엔드 개발자를 위한 영상이라 너무 딥하지 않고 적절하게 다루는게 좋음 (널널한 개발자는 일반 백엔드 개발자 입장에서 너무 low level까지 다루는 느낌이 없지않아 있다.)
- https://www.youtube.com/watch?v=k1gyh9BlOT8&list=PLXvgR_grOs1BFH-TuqFsfHqbh-gpMbFoy
	- 널널한 개발자 네트워크 입문 시리즈 - 단순 참고 용이 아니라 제대로 볼거면 인프런 강의 사서 보는게 나을듯?
- https://www.youtube.com/watch?v=c62qssA4hYI&list=PLYH7OjNUOWLVwdRF6_QmJVR4cQdMp0SU1
	- 혼자 공부하는 네트워크
- https://www.youtube.com/watch?v=XFYRbeqGEtg
- https://www.youtube.com/watch?v=Xp1IKwJfDAA&list=PLg9hpcQTPCpdRwBGjWC2kRuaTMuUObPY9
	- 웹개발입문 네트워크편
- https://www.youtube.com/watch?v=0lyrd5FlETQ&list=PLpO7kx5DnyIGzsb-4N9Z76RIR1xfprlIl
	- 얄코 개발자 유튜브


