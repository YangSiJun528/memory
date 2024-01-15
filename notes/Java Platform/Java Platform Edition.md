- - -
자바 플랫폼은 자바 프로그래밍 언어로 개발된 프로그램을 개발하고 실행하는 일을 쉽게 해주는 프로그램의 모임이다.

아래와 같은 자바 플랫폼 에디션이 있다.
- Java Card
- Java ME (Java Platform, Micro Edition, 약칭 Java ME)
- Java SE (Java Platform, Standard Edition)
- Jakarta EE (Jakarta EE, Enterprise Edition, 구 명칭: Java EE)
- Java FX (버전 8부터 10까지는 Oracle의 JDK에 번들로 제공되지만 11부터는 별도로 제공)

## Java Card
Smart Card(IC카드)에서 필요한 기술/환경을 제공하는 자바 플랫폼이다.

> Java Card 기술은 Java 플랫폼을 스마트 카드 및 다른 환경에 적합하도록 적응시킵니다. 이러한 환경은 일반적으로 J2ME 장치보다 훨씬 특수화되어 있으며, 메모리 및 처리 제약이 더 심한 장치입니다. [^1]

> **❓내 생각**   
> [오라클의 자바 EE 6 공식문서](https://docs.oracle.com/javaee/6/firstcup/doc/gkhoy.html)에는 Java Card 플랫폼이 존재하지 않는다. 위키피다아가 잘못되었을 수도 있다. 
## Java ME, Micro Edition
명칭 변화 : J2ME -> Java ME

자원이 제한된 IoT 기기, 모바일 환경에서 필요한 기술/환경을 제공하는 자바 플랫폼이다.

임베디드 장치에서 동작하기 위해 자바 SE의 일부 기능만을 제공한다.[^2]

> 사물 인터넷의 임베디드 및 모바일 장치에서 실행되는 애플리케이션을 위한 강력하고 유연한 환경을 제공합니다.    
> Java ME에는 유연한 사용자 인터페이스, 강력한 보안, 내장 네트워크 프로토콜, 동적으로 다운로드할 수 있는 네트워크 연결 및 오프라인 애플리케이션 지원이 포함되어 있습니다.[^3]

> **❓내 생각**    
> [JSR-302](https://www.jcp.org/en/jsr/detail?id=302)(2021년) 에서 J2ME를 다루는 걸 보면, 여전히 자바 플랫폼에 속하고 있어 보인다.   
> OpenJDK에서 찾지 못했고, 아마 오라클에서 구현체로 제공해주는 것 같다.
## Java SE, Standard Edition
명칭 변화 : J2SE -> Java SE

표준적인 컴퓨팅 환경을 지원하기 위한 자바 플랫폼이다. JVMs, JLS, Java API 집합을 포함한다.    
자바 EE, ME 등 다른 플랫폼은 구체적인 목적에 따라 SE를 기반으로 API를 추가하거나 일부 명세를 택해서 정의된다.

OpenJDK를 통해서 관리되며, OpenJDK는 버전 7 이후로 자바 SE의 공식 레퍼런스 구현체이다.

[Java Platform Standard Edition의 구현체는 JRE, JVM가 있다.](https://docs.oracle.com/javase//7/docs/)

[Java SE 8 기준 Java 개념도](https://docs.oracle.com/javase/8/docs/)

## Jakarta EE, Enterprise Edition
명칭 변화 : J2EE -> Java EE -> Jakarta EE

분산 컴퓨팅 및 웹 서비스와 같은 엔터프라이즈 기능을 제공하는 플랫폼

기존에 Java EE로서 자바 플랫폼에 속해 있었으나, 현재는 이클립스 재단으로 이관되면서 Jakarta EE로 변경되었다.    
(Java EE 8 이후로 JSR이 올라오지 않는 모습으로, JCP 내에서 더 이상 관리하지 않는다.)

## Java FX 
JavaFX는 경량 사용자 인터페이스 API를 사용하여 풍부한 인터넷 애플리케이션을 생성하기 위한 플랫폼이다.   
Java Swing을 대체하기 위해 고안되었다.

버전 8부터 10까지는 Oracle의 JDK에 번들로 제공되지만 11부터는 별도로 제공된다.

OpenJDK의 하위 프로젝트인 OpenJFX에서 관리되고 있다.
# Reference
- 주로 참고
	- 위키피디아 - 자바 (소프트웨어 플랫폼) [한국어](<https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_(%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4_%ED%94%8C%EB%9E%AB%ED%8F%BC)>)/[영어](<https://en.wikipedia.org/wiki/Java_(software_platform)>)

[^1]: [An Introduction to Java Card Technology - Part 1](https://www.oracle.com/java/technologies/java-card/javacard1.html)
[^2]: [Difference Between Java SE/EE/ME](https://www.baeldung.com/java-se-ee-me)
[^3]: [Java Platform, Micro Edition (Java ME)](https://www.oracle.com/java/technologies/javameoverview.html)