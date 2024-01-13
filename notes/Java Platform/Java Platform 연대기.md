- - -

### 주의점

- 자바 플랫폼을 이해하기 위해서 요약한 정보로, 정확하지 않을 수 있다. 
- 자바 SE, EE 위주로 정리하였다. ME나 그 외 플랫폼에 관한 부분은 거의 다루지 않았다.

### 연대기

- 1995
	- 썬 마이크로시스템즈, 자바 알파버전 출시
- 1996
	- 자바 첫 안정 버전(Java 1.0) 출시
- 1998
	- JCP 설립
		- 자바 플랫폼의 새로운 기능/변경 사항은 JCP를 통해서 결정한다.
		- [JCP 집행위원회(EC)](https://www.jcp.org/en/participation/committee)가 있으며 여러 의사결정을 수행한다.
			- 2024. 01. 기준 JetBrains, Oracle, Microsoft, Amazon, Japan Java User Group, IBM 등이 포함되어있다.
	- ISO/IEC, ECMA에 표준으로 정식 승인을 받으려 했으나 절차에서 철수
		- (해당 정보는 위키피디아 외에 더 자세한 출처를 찾을 수 없었다.)
		- 하지만, C#은 일부 버전이나마 ECMA, ISO에 등재되어있다.
			- (ECMA-334 / ISO/IEC 23270)
		- 자바는 표준이 아니고, 사실상의 표준([_De facto_ standard](https://en.wikipedia.org/wiki/De_facto_standard))이다.
- 1999
	- 자바 플랫폼 명칭을 3종류로 통일. - J2SE, J2SE, J2SE
- 2005
	- 자바 SE 사용을 위해 구글과 썬의 자바 사용권 협의 시작
- 2006
	- 구글과 썬의 자바 사용권 협의 결렬
		- 구글, 자바 플랫폼의 완전히 새로운 버전을 만들기로 결정
		- 새로운 JVM을 Dalvik으로 명명
	- Java SE를 오픈소스화 하는 OpenJDK 계획 발표
	- 또한 오픈소스이며 Java EE의 공식 참조 구현인 GlassFish 출시
- 2007
	- Java SE 6 출시
	- OpenJDK를 공개
		- [GPLv2+CE](http://openjdk.java.net/legal/gplv2+ce.html) 라이선스를 가진다.
			- Classpath Exception이 붙은 이유 간단 설명
				- GPL 라이선스는 파생 코드도 GPL 라이선스를 가지고 코드를 공개해야만 한다. 
				- GPL+CE는 GPL에서 API를 사용하는 경우에는 제외한다는 예외가 추가된 버전이다.
				- 즉, JDK 구현을 건드리지 않고, JDK를 단순 사용하여 프로그램을 구현하는 경우는 소스코드를 공개하지 않아도 된다.
		- 썬이 배포권을 가지지 않는 일부 기능은 제외하여 출시하였다.
			- 2010년까지 이러한 제외된 기능을 전부 오픈소스 코드로 대체한다.
		- OpenJDK 6는 백포트(최신 버전의 특정 코드 가져와 이전 버전으로 포팅하는 작업, [위키피디아 backporting 참고](https://en.wikipedia.org/wiki/Backporting)) 때문에 성능에 문제가 있었다.
	- OpenJDK Community TCK 사용권 발표
		- 자바 표준과 호환 여부를 확인하는 TCK이다.
		- 타 Vender의 구현체더라도 TCK를 통과하면 'Java Compatible' 상표를 사용 가능
- 2010
	- 오라클, 썬을 인수
		- 두 회사는 합병되었으며, 오라클이 특허권과 저작권을 양도받음
	- 오라클, 구글을 자바 특허권 침해로 소송
		- 이 글에서 이 이후 오라클, 구글 사이의 소송에 관해선 다루지 않는다.
		- 자세한 내용은 아래 Reference를 참고해서 찾기
- 2011
	- Java SE 7 출시
		- 출시가 길어진 이유
			- 썬의 불황, 오라클로 인수되고 나서 자바 팀을 재정비 등 내/외부 적으로 여러 문제가 있었다.
		- Java SE 7부터 OpenJDK가 자바 SE의 공식 참조 구현이다. - [참고](https://jdk.java.net/java-se-ri/7)
			- 그렇다고 모든 JDK 구현체가 OpenJDK로부터 구현되는 것은 아니며, Azul처럼 자체적인 구현(JVM)을 가지는 경우도 있다.
			- Oracle JDK도 OpenJDK를 기반으로 구현된다.
- 2014
	- Java SE 8 출시
		- 함수형 프로그래밍(람다, 스트림), 널 안전성(Optional), 새로운 날짜 API(LocalDate), Default method 등 기능이 추가되었다.
		- 자바 기초 책이나 자료는 대부분 SE 8을 기준으로 작성되어 있고, 여태까지도 가장 많이 사용되는 버전이다. - [Jetbrains 2023년도 개발자 에코시스템 현황 기준](https://www.jetbrains.com/ko-kr/lp/devecosystem-2023/java/) 참고
- 2017
	- Java SE 9 출시
		- Java Platform Module System(Jigsaw) 및 그 외 기능들 추가
			- Jigsaw로 인해서 JRE는 더 이상 제공되지 않는다.
			- 자세한 내용은 [Jigsaw란? - 아직 미완](notes/Java%20Platform/Jigsaw란?%20-%20아직%20미완.md) 참고
	- Java EE 8 출시
	- 오라클이 Java EE를 Eclipse 재단에 이관하기로 결정
		- 이 시점 이후 Java EE를 JCP에서 다루지 않는다.
- 2018
	- Java SE 11 (LTS) 출시
- 2019
	- Java EE 8 과 완벽한 호완성을 가지는 Jakarta EE 8 출시
		- [참고](https://jakarta.ee/news/jakarta-ee-8-released/)
		- GlassFish도 Eclipse GlassFish로 변경, Github Repo도 Eclipse 재단으로 넘어갔다.
- 2020
	- Jakarta EE 9 출시
		- [참고](https://jakarta.ee/news/jakarta-ee-9-released/)
		- 이 글에서 이후 버전은 정리하지 않겠다.
		- 패키지 구조가 `javax.*`에서 `jakarta.*`로 변경
		- JCP 대신 Jakarta EE Specification Process (JESP)를 통해서 발전한다.
		- TCK 또한 Oracle 대힌 Eclipse 재단에서 제공한다.
- 2021
	- Java SE 17 (LTS) 출시
- 2023
	- Java SE 21 (LTS) 출시
# Reference

도서 - “자바 탄생 20주년” 자바가 걸어온 길, 자바가 걸어갈 길      
Wikipedia - Java (programming language) [영어](https://en.wikipedia.org/wiki/Java_(programming_language)#cite_note-32)/[한국어](<https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_(%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D_%EC%96%B8%EC%96%B4)>)      
[소프트웨어 정책 연구소 - 구글과 오라클 간의 자바 API 분쟁 역사 - (1)](https://spri.kr/posts/view/22066?code=data_all&study_type=industry_trend)      
[소프트웨어 정책 연구소 - 구글과 오라클 간의 자바 API 분쟁 역사 - (2)](https://www.spri.kr/posts/view/22162?code=industry_trend)      
[OKKY - Java 유료 논쟁, Oracle JDK와 OpenJDK의 차이 정리](https://okky.kr/articles/490213)      
[와탭 블로그 - 혼란스러운 Java 버전의 진실](https://www.whatap.io/ko/blog/12/)      

