- - -
### JCP, Java Community Process
> 1998년에 도입된 Java™ 기술 사양, 참조 구현 및 테스트 제품군을 개발하고 개정하기 위한 개방적이고 참여적인 프로세스입니다.

(Java 소프트웨어 플랫폼 == Java technology)

Java 소프트웨어 플랫폼에 대한 표준 기술 사양을 개발/개정 할 수 있도록 하는 단체.

자바는 초기에 썬 마이크로시스템즈가 만들었다.   
썬은 1998년에 JCP라는 단체를 설립.     
JCP은 JSR이라는 공식 문서를 통해서 Java 기술의 표준 기술 사양을 개발하고 있다.   

### JSR,  Java Specification Request
자바 사양 요청은 새로운 사양의 개발이나 기존 사양의 중요한 개정을 제안하기 위한 문서이다.
JCP 프로그램에는 현재 자바의 다음 버전을 포함하여 많은 자바 기술 사양이

### 그 외 JCP 용어
- **Specification Lead** 
	- 특정 JSR을 개발하는 전문가 그룹(EG)의 대표
- **PMO(Program Management Office)**
	- Java 커뮤니티 프로세스를 감독하고 프로그램의 일일 실행을 관리하도록 지정된 오라클 내 그룹입니다.
	- 실제 개발을 진행하는 그룹은 아니다.
- **EC(Executive Committee)**
	- 집행위원회라는 뜻, [Executive Committee Info](https://www.jcp.org/en/participation/committee)에서 현재 집행위원회 멤버를 확인 가능하다.
	- JCP의 핵심 사항을 통한 사양 통과를 승인하고 사양과 관련 TCK 간의 불일치를 조정하는 일 등을 담당한다.
	- 임기도 2년으로 존재하고, 주기적으로 회의에 참여하며, JSR에 투표하는 등 여러 의무/책임이 있다.
- **EG(Expert Group)**
	- JCP 회원 중 특정 JSR을 개발하는 전문가 그룹
- **RI (Reference Implementation)**
	- 최종 릴리즈 및 유지보수 단계에서 필요한 결과물
	- JSR이 구현될 수 있음을 증명하는 JSR의 구현체
- **TCK(Technology Compatibility Kit)**
	- 최종 릴리즈 및 유지보수 단계에서 필요한 결과물
	- 최종 사양의 구현을 테스트하여 완벽하게 호환되는지 확인하는 데 사용한다.

### JCP Flow
JCP는 4가지 주요 단계가 있다.   
`INITIATION -> DRAFT RELEASES -> FINAL RELEASE -> MAINTENANCE 

![JSR_Flow](notes/Java%20Platform/imgs/JSR_Flow.png)

# Reference
[FAQ: General JCP Questions](https://www.jcp.org/en/introduction/faq)     
[JCP 2.11: Process Document](. https://jcp.org/en/procedures/jcp2_11) - JSR의 구체적인 명세를 확인 가능
