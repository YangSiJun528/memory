- - -
### OpenJDK의 역사
(Oracle의 유료화 정책에 관한 이해를 돕기 위해서 작성되으며, 그 외 사건은 기록하지 않았다.)

#### 2006년
Oracle OpenWorld 컨퍼런스에서 썬 마이크로시스템즈는 Java SE를 오픈소스화 하는 계획 발표.  
#### 2007년
썬은 GPLv2 + Classpath Exception 라이선스를 가지는 OpenJDK를 공개.  
라이선스 전체 이름은 (GNU General Public License, version 2, with the Classpath Exception)   
이 시점에서 배포권을 가지지 않는 일부 기능은 제외되었음(GUI, SNMP)
#### 2010년
배포권이 없는 독점 상태의 클래스를 오픈소스로 대체하여 전체 JDK를 오픈소스화 완료
오라클이 썬 마이크로시스템즈를 인수
#### 2011년
OpenJDK 7 출시, 이 시점이후 Java SE의 공식 참고 구현
#### 2018년
오라클이 OracleJDK를 영구 라이선스에서 Java SE Subscription이라는 년 단위 구독(Subscription) 라이선스로 변경.    
이로 인해서 타 벤더의 OpenJDK로 많은 회사가 넘어감,   
	ex: [LINE의 OpenJDK 적용기: 호환성 확인부터 주의 사항까지](https://engineering.linecorp.com/en/blog/line-open-jdk)

### 오라클의 Oracle JDK 라이선스 전환
기존까지는 BCL(Binary Code License)를 사용했다. 일반 목적 컴퓨팅은 무료지만, 특수 목적의 컴퓨팅 장비에 사용 시 유료.    
2018년 OracleJDK를 영구 라이선스에서 Java SE Subscription이라는 년 단위 구독(Subscription) 라이선스로 변경 계획 발표. 비상업적인 용도에 한해서는 무료, 그 외에는 전부 라이선스가 필요함

Oracle JDK 8의 경우, 2019년 1월 이후 버전을 사용하기 위해선 구독 라이선스 필요.    
Oracle JDK 11 이후부터 구독 라이선스 적용, 라이선스 구독자만이 LTS 업데이트 지원을 받을 수 있다.    
#### 사람들의 오해 - Java는 무료 아닌가?
맞다. Java 플랫폼의 명세(SE인 JVMs, JLS, EE, ME 등)는 GPL 라이선스로 유료가 아니다.   
유료화가 되는건 Oracle JDK로 Oracle 회사에서 제공하는 JDK이다.   
Oracle JDK와 Java를 동일하게 판단하여 생긴 문제.   
#### 해결 방법
1. OracleJDK를 돈을 주로 라이센스를 구독한다.
2. 여러 벤더의 OpenJDK 중 하나를 선택하여 사용한다.
	- OpenJDK 기반 JDK는 무료로 제공하는 벤더가 많다.
	- 대부분 TCK를 통과하므로 기능 자체에 문제가 있을 확률은 적다.
	- 또한 OracleJDK또한 OpenJDK기반이므로 적용에 있어 큰 무리가 없다.

### OpenJDK의 특징

- 오픈소스
	- GPLv2 + Classpath Exception 라이선스를 가진다.
	- 오라클이 저작권을 가지고 있으며, 여러 단체/개인이 참여한다. 
	- JEP라는 절차를 통해 OpenJDK를 개선한다.
- Java SE의 공식 참고 구현
	- 따라서 ME, EE는 관리하지 않는다.
	- 최근 버전에서 FX는 SE와 분리되었으나 OpenJDX의 하위 프로젝트(OpenJFX)로 관리
- OCA, Oracle Contributor Agreement
	- Oracle이 후원하는 오픈소스에 기여하기 위해서 서명해야 하는 문서.
	- 기여자와 Oracle이 공동 저작권을 가지게 된다.
	- 코드베이스의 무결성을 보호하기 위해 사용한다.
	- Oracle은 법적 문제가 발생할 경우 커뮤니티를 대신하여 행동한다.
	- 출처: [Oracle - OCA FAQ](https://oca.opensource.oracle.com/?ojr=faq)
- OCTLA, OpenJDK Community TCK License Agreement
	- Oracle이 제공하는 JCK(Java SE TCK)를 사용하기 위해 서명해야 하는 문서.
	- [OpenJDK - OCTLA 서명자 목록](https://openjdk.org/groups/conformance/JckAccess/jck-access.html)
	- JCK를 통과하는 제품은 JSR을 만족하는 제품임을 보장한다.
- JEP, JDK Enhancement Proposal
	- JDK 개선 제안과 여러 노력(프로세스 및 인프라 개선 등)에 대한 제안 결과를 수집, 검토, 정렬 및 기록하는 프로세스
		- JDK 릴리스 프로젝트 및 관련 노력을 위한 장기 로드맵 역할을 한다.
		- 간단히 JDK 관련 개선에 대한 프로세스(제안서 초안 -> ... -> 완료 까지의 절차, 문서 형식 등)
		- 실제 코드 구현은 포함되지 않으며, 명세 또는 제안서에 가깝다.
	- OpenJDK에서 사용되는 개념으로, JCP에서 사용되는 개념이 아니다.
		- JEP에서 채택되었더라고 JCP에 등록되지 않았으므로, 자바 플랫폼 명세에 포함되지 않는다. 따라서 JCP에서 JSR을 통해 표준으로 등록하는 프로세스를 수행해야 한다.
			- "이 프로세스는 모든 OpenJDK 커미터에게 열려 있습니다. 특정 제안에 대한 결정은 투명한 방식으로 이루어지지만 궁극적으로 OpenJDK Lead에게 달려 있습니다. 이 프로세스는 어떤 방식으로든 JCP를 대체하지 않습니다." 
			- "JCP는 모든 표준 Java SE API 및 관련 인터페이스에 대한 관리 기관으로 남아 있습니다. 이 프로세스에 채택된 제안이 기존 표준 인터페이스를 수정하거나 새로운 인터페이스를 정의하려는 경우 이러한 변경 사항을 설계, 검토 및 승인하기 위한 병행 노력이 JCP에서 수행되어야 합니다."
			- 참고: [JEP 1: JDK 개선 제안 및 로드맵 프로세스](https://openjdk.org/jeps/1)
		- JEP가 최종 통과하여 로드맵에 올라가더라도 그걸 구현할지는 확실하지 않다.
			- "특정 JEP가 로드맵에 나타나는 것은 그것이 기술적인 관점에서 기록을 제안한다는 의미일 뿐입니다. 누구든지 작업할 것이라는 보장은 없으며 최종 결과가 JDK 릴리스 프로젝트에 나타날 것이라는 보장도 없습니다."  
		- 자유로운 아이디어 표현 가능
			- "이 프로세스는 공격적이고, 틀에 얽매이지 않으며, 심지어 완전히 엉뚱한 아이디어에도 명시적으로 열려 있습니다. 이러한 아이디어는 JDK 자체의 개선 사항으로 제안되기 전에 상당한 사전 연구, 실험 및 사회화를 필요로 하는 경우가 많습니다. 아이디어를 구체화하는 작업은 하나 이상의 탐색적 연구 제안에서 수행될 수 있습니다. 연구 JEP의 최종 결과는 JDK에서 작동하는 코드가 아니라 해결되는 문제와 솔루션 공간에 대한 더 깊은 이해를 문서화하고 구체적인 개선 제안을 뒷받침할 만큼 충분한 세부정보를 제공하는 것입니다."
	- 더 자세한 내용은 [Java Community Process란? - JCP, JSR, TCK](notes/Java%20Platform/Java%20Community%20Process란?%20-%20JCP,%20JSR,%20TCK.md) 참고
- 프로젝트
	- 하나의 JEP로 작업하기 어려운 큰 범위의 작업은 JDK 프로젝트로 간주된다.
	- 대표적으로 Amber, Loom, Valhalla 등이 있다.
	- Amber에 대해서만 정리해보자면, 생산성 지향적인 Java 언어 기능을 탐색하고 배양하는 걸 목적으로 한다. Java SE 17, 21에 추가된 여러 편리한 문법(JEP)이 Amber에서 만들었다.
- HotspotVM
	- OpenJDK는 HotspotVM을 사용한다.
	- 대부분의 VM 성능 개선/VM 구조 문서는 HotspotVM을 기준으로 작성된다.
		- GC 알고리즘 또한 HotspotVM의 구현으로, HotspotVM이 아닌 경우 다른 GC 구조, 알고리즘을 가질 수 있다.
			- Java SE JVMs에는 GC를 자유롭게 구현할 수 있도록 구현에 대한 명세가 존재하지 않는다.
	- OpenJDK를 사용하지 않고 자체적으로 구현한 JDK의 경우 HotspotVM을 사용하지 않는 경우도 있다.
	- 추가적인 내용은 [OpenJDK와 타 JDK와 비교 - JVM 비교](notes/Java%20Platform/OpenJDK와%20타%20JDK와%20비교%20-%20JVM%20비교.md) 참고

# Reference

[위키피디아 - OpenJDK](https://en.wikipedia.org/wiki/OpenJDK)    
[OKKY - Java 유료 논쟁, Oracle JDK와 OpenJDK의 차이 정리](https://okky.kr/articles/490213)
[“자바가 더 좋아지는 방법” JDK 개선 제안 프로세스와 최신 JEP](https://www.itworld.co.kr/news/239204)

