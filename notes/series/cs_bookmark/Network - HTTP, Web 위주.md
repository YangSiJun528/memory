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

![](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2F0e18db0d-f511-4f85-bb58-388fce70d42e_2631x2103.png)

- https://www.youtube.com/watch?v=H6lpFRpyl14
- https://www.yalco.kr/31_https/
- https://www.cloudflare.com/ko-kr/learning/ssl/what-is-https/
- https://www.youtube.com/watch?v=j9QmMEWmcfo&t=278s
- https://blog.bytebytego.com/i/53596514/how-does-https-work


## HTTP란? 배경과 장단점


## HTTP Handshake, 연결 과정


## [북마크만] Web 역사, 배경 

- https://www.youtube.com/watch?v=sG1YA6IssTY
	- HTTP와 Web 탄생
- https://www.youtube.com/watch?v=YuslTfJUJm0
	- JS의 탄생과 브라우저 분쟁, AJAX
- https://www.youtube.com/watch?v=Rpue8mJj4is
	- JS의 표준화와 FE 프레임워크, TS, 웹 어셈블러
- https://www.youtube.com/watch?v=KpMDqrBEySs, https://www.books.weniv.co.kr/basecamp-network/chapter11/11-3
	- 웹의 역사

