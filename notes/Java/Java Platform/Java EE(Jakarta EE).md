- - -
### 주의점
Java EE, Jakarta EE 용어를 혼용하여 설명하고 있는데, 이름의 차이는 크게 중요하지 않으므로 대충 알아서 보기

### Java EE(Jakarta EE)의 역사
1999년 썬 마이크로시스템즈가 J2EE(Java 2 Enterprise Edition)라는 이름으로 발표한 분산 애플리케이션 개발 목적의 산업 표준 플랫폼이다.  
기존에도 Enterprise Edition의 기능이 있긴 하였으나, J2EE로 분리하여 관리되는 건 1999년도.

이후 꾸준히 발전되어 왔으나     
2017년, 오라클이 Java EE를 이클립스 재단에 이관하기로 결정한다.    
(수익화에 실패하면서 기술 주도권을 넘긴 것으로 보인다. - 오라클이 공식적으로 이야기한 자료는 찾지 못했으나 여러 매체에서 그렇게 주장함)

이클립스 재단으로 넘어가면서 _Java_ 라는 이름을 사용할 수 없게 되었고, Jakarta EE로 이름을 변경하였다.   
(_Java_ 라는 이름은 Oracle이 가지고 있다. - 정확하지 않는데, 사업적인 이유로 이름을 넘겨주지 않았다고 보는 사람들도 있는듯)

2019년 이클립스 재단은 Java EE 8과 완벽하게 호환되는 Jakarta EE 8 발표.

2020년 Jakarta EE 9 발표, API namespace가 `javax` 에서 `jakarta`로 변경되었다.

### Jakarta EE
Jakarta EE로 변경되면서 JCP에서 더 이상 다루지 않게 되었다.   
EFSP([Eclipse Foundation Specification Process](https://www.eclipse.org/projects/efsp/))에서 Jakarta EE를 발전시켜나가고 있다.

TCK 역시 8 이후 버전은 이클립스 재단에서 제공하고 있다.

### Jakarta EE Spec
Jakarta EE에는 웹 페이지 생성, 트랜잭션 데이터베이스 읽기 및 쓰기, 분산 대기열 관리와 같은 다양한 목적을 제공하는 여러 사양이 포함되어 있다.

Jakarta EE API는 Java SE API를 기반으로 Jakarta Enterprise Beans, connectors, servlets, JSP(Java Server Pages) 등 여러 웹 서비스의 기술을 포함한다.

![JavaEE Platform Specification Diagram](notes/Java/files/java_ee_platform_specification_diagram.png)

#### 서블릿 컨테이너
Jakarta EE의 여러 사양 중, Jakarta Servlet 사양을 구현하는 서버.   
대표적으로 Tomcat 및 Jetty가 있는데, 이를 서블릿 컨테이너라고 부른다.  
#### Jakarta Enterprise Beans(구 EJB, Enterprise JavaBeans)
EJB는 JavaEE 사양의 하위 집합으로, 서버에 필요한 기능(보안, 서블릿, 트랙잭션 등)을 제공한다.

##### Spring의 등장
초기에는 실무에 맞지 않는 사용법, 특정 환경/기술의 종속(테스트가 어려워진다), EJB 컨테이너를 실행시키기 위한 고가의 WAS 등 문제가 많았다.    
이로인해 상대적으로 가볍고 객체지향적인 Spring이 등장하게 되었다.

그러나 요즘(EJB 3.0 이후)에는 이전 사양과 다르게 가볍고, Spring의 영향을 받아 DI가 등장하는 등 더 나아지고 있다고 한다. - (출처 부족한 내용, 중요해서 검토하지 않았는데 나중에 인용할꺼면 추가로 조사하기)

# Reference
[[Spring] POJO 프로그래밍과 스프링 프레임워크의 탄생](https://mangkyu.tistory.com/281)
