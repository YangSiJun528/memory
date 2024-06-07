## 브라우저에서 URL을 입력하면 일어나는 일

![bytebytego](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2F4c457b4f-55e1-44dc-9ae2-05f441b3354d_2640x2127.png)

1. 브라우저가 URL 입력
2. 브라우저의 DNS 캐시 -> OS의 DNS 캐시 순으로 URL에 해당되는 IP 조회. 있으면 캐싱됨
3. 없으면 브라우저는 DNS Resolver에 요청.
4. DNS Resolver는 재귀적으로 DNS Server에 요청
	1. [Root nameserver](https://ko.wikipedia.org/wiki/%EB%A3%A8%ED%8A%B8_%EB%84%A4%EC%9E%84_%EC%84%9C%EB%B2%84) -> TLD nameserver(`.net`, `.com` 등) -> Authoritative nameserver
5. DNS Resolver가 IP 주소를 반환하면, HTTP 통신을 통해 정보를 받음.
6. 브라우저가 HTTP 콘텐츠를 렌더링 함.

- https://blog.bytebytego.com/p/what-happens-when-you-type-a-url
- https://www.youtube.com/watch?v=AlkDbnbv7dk

## DNS 패킷 읽어보기 - 실습

- https://www.geeksforgeeks.org/dns-in-wireshark/

## DNS
도메인 이름 체계(DNS. Domain Name System)

사람이 읽을 수 있는 도메인을 IP 주소로 변환하는 시스템

 - [Root nameserver](https://ko.wikipedia.org/wiki/%EB%A3%A8%ED%8A%B8_%EB%84%A4%EC%9E%84_%EC%84%9C%EB%B2%84) 
	 - TLD nameserver에 대한 IP를 저장
	 - `a~m.root-servers.net` 총 13개의 루트 서버 운영자(organizations)
	 - 루트 서버 인스턴스는 수백개 이상, anycast를 사용해 사용자에게 가장 가까운 인스턴스로 라우팅
 - TLD nameserver(`.net`, `.com` 등)
	 - Authoritative nameserver에 대한 IP를 저장
	 - 예시: `.com` nameserver는 `google.com`을 비롯한 `~~.com` 형태의 모든 IP를 알고 있음.
 - Authoritative nameserver
	 - 최종 IP 주소를 가지고 있음. (DNS 기준으로 최종, 해당 IP가 로드벨런서 같은 거일 수도 있음. 이 경우 사용자 입장에서 최종이 아니긴 함.)
	 - 예시: `example.com` nameserver는 `~.example.com`의 모든 IP를 알고 있음.
	 - Authoritative: 신뢰할 수 있는

- 브라우저와 운영체제는 DNS 캐시를 가진다.

- DNS Resolver
	- DNS Resolver는 디바이스와 별개로 별도의 서버에 위치한다. 
		- (로컬 캐싱된거를 관리하고 다른 DNS Resolver에 요청을 보내는 애들 Local/OS DNS Resolver라고 부르기도 하는거 같은데, 일반적으로 설명하는 IP를 구하는 역할은 로컬에서 수행하지 않는다.)
			- hosts 파일이라고 부름. 윈도우: `C:\Windows\System32\drivers\etc\hosts`, 맥/리눅스: `/etc/hosts`
	- DNS Resolver는 여러 종류가 있다.
		- ISP DNS Resolver
			- 통신사에서 제공하는 DNS, AT&T. SK Broadband 등
		- 공용 DNS Resolver 
			- Google Public DNS, Cloudflare DNS 등
	- DNS Resolver는 URL의 IP를 찾는다.
	- DNS Resolver는 재귀적으로 name server에 요청을 보낸다.
		- [Root nameserver](https://ko.wikipedia.org/wiki/%EB%A3%A8%ED%8A%B8_%EB%84%A4%EC%9E%84_%EC%84%9C%EB%B2%84) -> TLD nameserver(`.net`, `.com` 등) -> Authoritative nameserver
		- 왜냐면 DNS는 계층적인 구조를 가져서, 각 네임스페이스가 가지는 정보가 다르기 때문
	- DNS Resolver도 캐시를 가지고 있다. 즉, 통신사 DNS 면 그 통신자 사용자들이 자주 들어오는 곳이 캐싱됨.

- DNS 주의점
	- 각 name server마다 TTL이 있으므로 변경이 반영되기까지 오래 걸린다. (심지어 일부 DNS Resolver는 설정한 TTL을 지키지 않기도 한다.)
		- TTL을 충분히 낮추기 (ex: 30초)
		- 기존 IP도 일부 기간동안 서비스하기

- https://www.youtube.com/watch?v=27r4Bzuj5NQ
- 책 "읽자마자 IT 전문가가 되는 네트워크 교과서"
- https://www.lenovo.com/us/en/glossary/dns-resolver/ 
- https://aws.amazon.com/ko/route53/what-is-dns/
- https://www.cloudflare.com/ko-kr/learning/dns/what-is-dns/

## URL, URI, URN

![](https://lh4.googleusercontent.com/K9t30Sjp6cIbsLg67B-amTH927yS3q06E68DvSLK_4t364b38otQLLibFbyj_I0JH6fLB8tp5zEgVObcaSj810X1Xs2XWuwxD-_ibK8JtfsUaGgm5JNVhUhfjcH3vairIchwhKfviglEmKYYk5aH4cc74ncEaW6N9fRS5VZtKS4EGmvGSYIUSLvx04r0xQ)


- schme(스킴): 리소스에 접근하는 데 사용할 프로토콜
- host: 서버의 호스트 이름
- path: 접근할 경로에 대한 상세 정보
- URL: 프로토콜(접근 방법) + URI(자원의 위치)
- URI: 자원의 위치
- URN: 웹 문서 자체, **식별자** 역할

- https://www.elancer.co.kr/blog/view?seq=74
	- 아마 위 자료의 원본: https://danielmiessler.com/p/difference-between-uri-url
- https://developer.mozilla.org/ko/docs/Glossary/URL
- https://developer.mozilla.org/ko/docs/Glossary/URN
- https://developer.mozilla.org/ko/docs/Glossary/URI


## 브라우저 구성 요소

![](https://d2.naver.com/content/images/2015/06/helloworld-59361-1.png)

- UI: 사용자의 입력을 받는 역할. 웹 페이지 자체를 제외한 나머지 전부. tap bar나 주소줄 등
- 브라우저 엔진: UI와 렌러링 엔진 사이의 동작 제어
- 렌더링 엔진: HTML, CSS 파싱해서 그리기 
- 통신: HTTP 등 네트워크 통신 지원
- 자바스크립트 엔진: V8 (Chrome, Edge), SpiderMonkey (Firefox) 등
- UI 백엔드: 버튼 및 창과 같은 기본적인 위젯 그리기. OS와 통신
- 데이터 저장소: 로컬스토리지나 쿠키, 사용기록 등 필요한 데이터 저장

- https://d2.naver.com/helloworld/59361


## HTTPS 와 TLS/SSL
TLS(Transport Layer Security)는 SSL의 후속 버전으로 더 강력한 보안 기능을 제공한다.    
또한 TLS 프로토콜 버전마다 동작 방식이 다르다.

HTTPS는 HTTP에 TLS 프로토콜이 추가된 것.

- HTTP의 문제점
	- 클라이언트와 호스트가 주고받는 데이터가 외부에 그대로 노출되어 보안이 좋지 않음.
	- 접근한 호스트가 안전한지 알 수 없다.
		-  [DNS 스푸핑](https://ko.wikipedia.org/wiki/DNS_%EC%8A%A4%ED%91%B8%ED%95%91) 등의 문제. 악성 프로그래밍 로컬의 DNS 캐시 정보를 수정하거나 hosts 파일 정보를 수정하면 올바른 주소를 입력해도 피싱 사이트로 이동할 수도 있다.

- TLS 인증서란?
	- 웹 서버와 브라우저 간의 안전한 통신을 보장하기 위한 인증서
	- 도메인 이름, CA, 발급일, 공개키 등을 가진다.
	- 비대칭키 방식으로, 비밀 키는 서버만 가지고 있다.
	- 자세한 정보는 [여기서](https://www.cloudflare.com/learning/ssl/what-is-an-ssl-certificate/) 참고

- [CA](https://ko.wikipedia.org/wiki/%EC%9D%B8%EC%A6%9D_%EA%B8%B0%EA%B4%80)(Certificate Authority. 인증 기관)
	- 인증서를 발급하고, 인증서가 유효한지 확인해준다.
	- 브라우저는 이런 CA의 목록을 저장하고 있다.

- HTTPS 동작 원리
	- 자세한 내용은 [여기서](https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/) 참고
	- 간단하게 요약
		1. 서버가 인증서를 보냄
		2. 클라이언트는 CA에 인증서가 유효한지 확인
			- 접근한 도메인이 안전하고 유효한지 확인
		3. 비대칭 키로 요청을 주고 받으면서 연결 대상끼리만 공유하는 대칭 키인 Session 키 생성
		4. Session 키를 기반으로 암호화된 데이터를 주고 받음
			- 외부에서는 암호화된 데이터를 읽을 수 없음

- HTTPS와 대칭키와 비대칭키
	- TLS 인증이 비대칭키를 사용하는 이유
		- 비밀 키를 가지는 안전한 주체임을 확인할 수 있음. (= 공개키 대비 안전)
	- 대칭 키인 Session 키를 생성해서 통신하는 이유
		- 비대칭 키 기반의 암호화/복호화는 더 많은 연산이 사용되기 때문

- HTTPS는 HTTP보다 더 빠르다.
	- HTTP/2는 SSL/TLS에서만 지원한다. 
	- 즉, HTTP 1.1과 HTTP 2.0의 속도 차이 때문에 이런 결과가 나옴.
	- [참고](https://tech.ssut.me/https-is-faster-than-http/)

![](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2F0e18db0d-f511-4f85-bb58-388fce70d42e_2631x2103.png)

- https://www.youtube.com/watch?v=H6lpFRpyl14
- https://www.yalco.kr/31_https/
- https://www.cloudflare.com/ko-kr/learning/ssl/what-is-https/
- https://www.youtube.com/watch?v=j9QmMEWmcfo&t=278s
- https://blog.bytebytego.com/i/53596514/how-does-https-work


## HTTP란? 특징 <<< 작성하기


## HTTP 버전 특징 - 1.0 ~ 3.0

HOLB: Head Of Line Blocking

(여기 설명 외에도 많은 기능 있음)

- #### 1.0
	- 동일한 서버의 요청에도 매번 TCP 연결이 필요함.
- #### 1.1
	- [keep-alive 헤더](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Keep-Alive) 도입 - 지속 연결(Persistent Connection) 지원 - TCP 연결을 재사용
	- pipelining(파이프라이닝 도입) - 응답을 기다리지 않고 여러 요청 가능
		- 단, HOLB 문제. HTTP 요청 순서에 맞춰서 응답해야만 함. 특정 요청이 오래 걸리는 경우(패킷 손실, 무거운 요청), 그 이후 요청은 빨리 끝나도 계속 대기해야 함. 
		- 구현하기 어려웠고, 많은 브라우저/리버스 프록시에서 지원하지 않게 됨
	- 브라우저는 성능을 위해 여러 커넥션을 맺은 상태를 유지해야 했음. 
- #### 2.0
	- "스트림" 도입 - 하나의 TCP 연결에서도 요청 순서 상관 없이, 데이터 주고받을 수 있음.
		- HTTP 단의 HOLB 문제를 해결했으나, TCP 단의 HOLB 문제는 남아있음. (TCP는 패킷 순서를 지켜야 함)
	- HTTP/2 Push 기능 도입 - 서버가 클라이언트가 요청 시 명시적으로 요청하지 않은 여러 데이터를 추가로 응답 가능. (요청 필요)
		- polling이 없어도 되므로 효율적임.
		- (주의) SSE (Server Sent Events, HTML5 표준)와는 별개의 개념.
			- SSE는 서버에서 클라이언트로 단방향 이벤트 스트림 제공, 클라이언트 요청 없이 데이터 전송 가능 
- #### 3.0
	- TCP 대신 Google에서 사용하던 QUIC 프로토콜을 사용함. QUIC는 UDP 기반임.
		- TCP의 HOLB와 레거시(TCP + TLS Handshake, IP 바뀌면 재연결해야 함, 확장/커스텀 공간 부족) 문제 때문에 UDP를 사용함. [참고](https://evan-moon.github.io/2019/10/08/what-is-http3/?fbclid=IwAR1V1-yWjkzWEAqm_1OZfe_gtG05EuVo7WXXyVdEz_J0UHZBpGruU8PU0FY#3-way-handshake)
	- IP+Port(TCP)대신, Connection ID라는 식별자를 사용해서 네트워크가 바뀌어도 계속 연결 가능 (모바일에 유리)
	- QUIC 스트림은 QUIC 연결을 공유하고, 각 스트림은 독립적이라 HOLB 문제가 발생하지 않음.
	- QUIC는 TCP + TLS 과정을 3 RTT(대충 왕복 통신이라는 의미)를 1 RTT로 줄여준다. (효율적임)

<details>
  <summary>GPT 피셜 정리</summary>
  <div markdown="1">
    <p>
      HTTP(하이퍼텍스트 전송 프로토콜)은 웹 상에서 데이터를 전송하는 프로토콜로, 다양한 버전이 존재합니다. 각 버전은 성능 향상, 보안 강화 및 새로운 기능 추가를 통해 발전해왔습니다. 다음은 주요 HTTP 버전들의 특징입니다.
    </p>
    
    <h3>HTTP/0.9</h3>
    <ul>
      <li><strong>출시 시기:</strong> 1991년</li>
      <li><strong>특징:</strong> 
        <ul>
          <li>매우 간단한 프로토콜로, 오직 GET 메서드만 지원</li>
          <li>HTTP 헤더가 없으며, 단순히 HTML 페이지를 전송하는 기능만 제공</li>
          <li>요청 라인 예시: <code>GET /index.html</code></li>
        </ul>
      </li>
    </ul>
    
    <h3>HTTP/1.0</h3>
    <ul>
      <li><strong>출시 시기:</strong> 1996년</li>
      <li><strong>특징:</strong> 
        <ul>
          <li>GET, POST, HEAD 메서드 지원</li>
          <li>HTTP 헤더 도입으로 메타데이터 전송 가능</li>
          <li>상태 코드 및 콘텐츠 타입 지정 가능</li>
          <li>비지속적인 연결: 요청마다 새로운 TCP 연결을 생성하고 응답 후 연결을 종료</li>
        </ul>
      </li>
    </ul>
    
    <h3>HTTP/1.1</h3>
    <ul>
      <li><strong>출시 시기:</strong> 1997년</li>
      <li><strong>특징:</strong> 
        <ul>
          <li>지속 연결(Persistent Connection) 지원: 연결을 재사용하여 성능 향상</li>
          <li>파이프라이닝(Pipelining) 지원: 하나의 연결에서 여러 요청을 순차적으로 보내 대기 시간 감소</li>
          <li>호스트 헤더 필드 도입: 가상 호스팅 지원으로 한 IP 주소에 여러 도메인 호스팅 가능</li>
          <li>캐싱, 콘텐츠 인코딩, 청크 전송 인코딩 등 다양한 기능 추가</li>
          <li>더욱 세밀한 상태 코드 및 HTTP 메서드 추가</li>
        </ul>
      </li>
    </ul>
    
    <h3>HTTP/2</h3>
    <ul>
      <li><strong>출시 시기:</strong> 2015년</li>
      <li><strong>특징:</strong> 
        <ul>
          <li>바이너리 프레이밍 계층 도입: 텍스트 기반이 아닌 바이너리 형식으로 데이터 전송</li>
          <li>멀티플렉싱 지원: 하나의 연결에서 여러 스트림을 동시에 처리하여 병목 현상 해결</li>
          <li>헤더 압축: 헤더 크기 감소로 성능 향상 (HPACK 압축 사용)</li>
          <li>서버 푸시: 클라이언트 요청 없이도 서버가 리소스를 푸시 가능</li>
          <li>기존 HTTP/1.1과 호환성 유지</li>
        </ul>
      </li>
    </ul>
    
    <h3>HTTP/3</h3>
    <ul>
      <li><strong>출시 시기:</strong> 2020년 (초안)</li>
      <li><strong>특징:</strong> 
        <ul>
          <li>QUIC 프로토콜 기반: TCP 대신 UDP를 사용하여 연결 수립 및 데이터 전송</li>
          <li>빠른 연결 수립: TCP 핸드셰이크가 필요 없어 지연 시간 감소</li>
          <li>내장된 보안: TLS 1.3을 기본적으로 사용하여 보안 강화</li>
          <li>향상된 멀티플렉싱: HTTP/2의 단점 보완, 패킷 손실에도 성능 저하 없음</li>
          <li>더 나은 성능 및 안정성 제공</li>
        </ul>
      </li>
    </ul>
  </div>
</details>

- https://www.youtube.com/watch?v=a-sBfyiXysI
- https://dev.to/accreditly/http1-vs-http2-vs-http3-2k1c
- https://blog.bytebytego.com/p/http-10-http-11-http-20-http-30-quic

## [북마크만] Web 역사, 배경 

- https://www.youtube.com/watch?v=sG1YA6IssTY
	- HTTP와 Web 탄생
- https://www.youtube.com/watch?v=YuslTfJUJm0
	- JS의 탄생과 브라우저 분쟁, AJAX
- https://www.youtube.com/watch?v=Rpue8mJj4is
	- JS의 표준화와 FE 프레임워크, TS, 웹 어셈블러
- https://www.youtube.com/watch?v=KpMDqrBEySs, https://www.books.weniv.co.kr/basecamp-network/chapter11/11-3
	- 웹의 역사
- https://www.youtube.com/watch?v=Cngpm5NCb-Q
	- 웹 개발 업무
		- php -> java, C# 순으로 소개하는데, 이거는 웹 생태계? 기준으로 본격적으로 참여한 시점 정도로 보면 될거 같고. 실제 시기는 java가 php보다 먼저임
- https://www.youtube.com/watch?v=1JjUYaoxJ9Y&list=PLcXyemr8ZeoSGlzhlw4gmpNGicIL4kMcX&index=4
	- WWW의 개념과 발명 과정

## OAuth, OpenID Connect(OIDC)

- OAuth2
	- 인가 목적
	- 액세스 토큰(랜덤 문자열도 가능)이나 스코프(가능한 범위)에 대한 표준이 없음.
		- 각 제공자마다 알아서 사용함.
			- OIDC를 제공하지는 않지만, 액세스 토큰에 JWT로 식별 정보를 가지고 있는 경우도 있음.
	- 사용자의 정보(인증,식별 정보)를 확인하기 위해서 액세스 토큰을 사용해서 요청해야 함.
- OIDC(OpenID Connect)
	- OAuth2의 상위 집합. 인가에 초점이 맞춰진 OAuth2에 인증 기능을 추가하는 프로토콜 정도로 보면 될 듯.
	- ID 토큰(액세스 토큰이랑 별개거나 하나인 듯?)이라고, 정해진 형식(몇 개는 필수인) JWT를 사용함
		- 사용자 이름이나 식별정보 등의 정보를 가지고 있어서, 사용자 식별 정보를 요청하지 않아도 됨.
	- 제공자에 따라 OAuth2는 지원하지만, OIDC 지원하지 않기도 함.
		- 구글이나 슬랙은 OIDC를 지원한다.
		- Github은 OIDC를 지원하지 않는다.

- https://www.youtube.com/watch?v=DQFv0AxTEgM&t=1774s
	- OAuth 관련 역사, 현재, 미래
- https://www.youtube.com/watch?v=iOUicQDEAyI&t=155s
	- OAuth , JWT
- https://devocean.sk.com/blog/techBoardDetail.do?ID=165453&boardType=techBlog
	- OAuth, OIDC - 출처 부분 참고할만하게 있음
- https://hudi.blog/open-id/
	- 설명 잘 해줌
- https://gist.github.com/nicolasdao/5f428529426d2183e2f1358fb46ba642
	- 정리 잘 됨

## JWT
JSON Web Token의 약자. 한글로는 JWT 그대로 부르지만, ByteByteGo 등 외국 영상을 보면 "jot"(좌ㅌ)?라고 발음한다.

- 토큰 기반의 인증 방식으로, 표준화 된 방식 중 하나.
- 서버가 인증 정보를 저장하지 않고, 클라이언트가 저장하는 JWT에 인증(식별, 사용자 정보)가 포함된다.

- 왜 쓰는가?
	- 등장 배경: JWT는 분산된 환경에서 인증 서버의 부하를 줄이기 위함
		- 인터넷이 확장됨에 따라 많은 트래픽을 처리해야 함.
		- 인증은 각 서비스에서 처리해야 하는데, 관리하기 어려움.
		- 그래서 하나의 인증 서버를 두고 처리
		- 인증 서버에 병목이 발생함. - 매 요청마다 계속 읽어야 함. MSA면 특히 더 심하고
		- 클라이언트가 인증 정보를 가져서 인증/인가 서버의 부하를 줄이도록 JWT를 사용.
	- 사용 목적
		- 상태 비저장 인증: 서버에 상태를 저장하고 싶지 않거나, 그래도 되는 서비스인 경우.
		- 서버 간의 통신: API 간의 신뢰할 수 있는 데이터 교환 - OAuth나 OIDC가 대표적.
		- 인증 서버의 부하가 많이 발생하는 대규모 서비스에서 트래픽 개선
		- 모바일과 같이 여러 환경을 제공해야 하는 경우: 브라우저가 아니라면 쿠키를 사용할 수 없음. 
			- (그래도 토큰 키를 사용해서 세션처럼 사용할 수 있는거 아닌가?)

- JWT 설명
	- 비대칭키로 만들어진다.
		- 비밀 키는 서명 역할, 공개 키는 검증 및 복호화를 통한 사용자 식별 정보 접근.
	- 헤더(알고리즘), 페이로드(필요한 정보, 커스텀 추가 가능), 시그니처(서명 정보, 검증에 사용됨)
	- 한계점이 명확하다. 서버가 아니라 클라이언트가 인증 정보를 저장하는 것이 근본적인 원인
		- 서버가 아니라 클라이언트가 인증 정보를 저장하므로 세세한 처리가 어렵다.
			- 로그아웃과 같은 토큰 무효화 처리
		- JWT는 상대적으로 큰 크기를 가진다. 네트워크 대역폭에 영향 가능성
		- 보안 문제, 외부에서 쉽게 복호화 가능하다.
			- Base64URL로 디코딩하면 내용을 읽을 수 있다.
			- 따라서 민감한 개인정보를 저장하면 안된다.
			- 이런 문제를 해결하기 위해서 2중으로 처리하기도 하는 듯?
		- 빈번한 상태 변경에 취약하다.
			- JWT를 매번 새로 작성해주어야 함.

- https://www.youtube.com/watch?v=iOUicQDEAyI&t=155s
	- OAuth , JWT
- https://www.youtube.com/watch?v=P2CPd9ynFLg
	- ByteByteGo
- https://ko.wikipedia.org/wiki/JSON_%EC%9B%B9_%ED%86%A0%ED%81%B0#cite_note-rfc7519-1
- https://jwt.io/
- https://blog.bytebytego.com/p/ep34-session-cookie-jwt-token-sso
- https://www.youtube.com/watch?v=7abbNwuCXbg
- https://www.youtube.com/watch?v=THFmV5LPE6Y - JWT 한계점


