
### JDBC
- 개념: [JDBC](https://en.wikipedia.org/wiki/Java_Database_Connectivity)(Java Database Connectivity)는 데이터베이스에 접근할 수 있도록 정의한 표준 인터페이스(API)이다.
- 등장 이유: 각 DB 제공사의 구현(DB Driver)에 의존하고 있었다.
- 한계
	- Connection을 관리하고, SQL을 보내는 것 등 많은 작업이 표준화되었으나, SQL문법, 데이터 타입이 특정 DBMS에 의존하는 경우가 많다. (ANSI가 있다지만, 일반적인 기능만 정의한다.) 
		- 이후 등장한 JPA(Java Persistence API)를 사용하면 이런 문제도 상당수 해결된다.
	- 표준을 정했을 뿐, 반복되는 Boilerplate가 많다.
- 관련 객체 - 이후 등장할 DB 관련 프레임워크에서 사용된다.
	- [java.sql.Connection](https://docs.oracle.com/javase/8/docs/api/java/sql/Connection.html)
		- 특정 DB와의 Connection(Session)이다. 네트워크 상의 연결 그 자체를 의미한다.
		- 연결이 되어 있어야 SQL 실행/결과 주고받기, 트랙잭션 시작/커밋 등의 작업이 가능하다.
	- [java.sql.DriverManager](https://docs.oracle.com/javase/8/docs/api/java/sql/DriverManager.html)
		- 표준 커넥션 인터페이스이다.
		- 연결 과정
			1. `getConnection(URL, USERNAME, PASSWORD)`을 통해 커낵션(연결) 시도
			2. 라이브러리에 등록된 드라이버 목록을 인식해서 해당 `Connection`을 처리할 수 있는지 확인하고, 유효한 드라이버에게 넘긴다.
			3. 드라이버 구현체가 커낵션이 맺어진 구체적인 `Connection`객체를 돌려준다.
	- [java.sql.Statement](https://docs.oracle.com/javase/8/docs/api/java/sql/Statement.html): SQL 내용을 구성한다.
	- [java.sql.ResultSet](https://docs.oracle.com/javase/8/docs/api/java/sql/ResultSet.html)
		- SQL 호출 이후 리턴 타입으로,  결과를 표시한다.
		- Cursor를 사용해서 순차적으로 다음 연결을 가져올 수 있다.
- 커넥션 풀
	- 등장 이유: 데이터베이스 커넥션을 새로 만드는 것은 복잡한 과정과 많은 시간이 걸림, 따라서 일정 개수의 커넥션을 생성하고 재사용하는 커넥션 풀 기법이 등장.
	- [javax.sql.DataSource](https://docs.oracle.com/javase/8/docs/api/javax/sql/DataSource.html)
		- 커넥션 풀 역할의 자바의 표준 인터페이스이다. (JDBC 1.4부터 도입)
		- `getConnection()`을 사용해 DriverManager에서 얻는 `Connection`과 동일한 `Connection`을 반환한다.
		- 일반적으로 DataSourse의 구현체는 JNDI에 등록되는데, DB 관련 이야기는 아니므로 생략.

[JDBC와 DataSource 이해하기](https://tecoble.techcourse.co.kr/post/2023-06-28-JDBC-DataSource/) 글이 정리가 잘 되어 있다.

![DB 커넥션 구조](https://user-images.githubusercontent.com/52024566/190993965-827d69c3-0189-4417-b6d4-d21eeb71b07a.png)

#### SQL Mapper
- 개념: 특정 표준이 아닌, SQL 문과 객체의 필드를 매핑하여 데이터를 객체화 해주는 프레임워크를 의미한다.
- 등장 이유: JDBC는 반복되는 Boilerplate를 줄여주고, 결과를 원하는 객체로 매핑하여 처리할 수 있다.
- 대표적인 구현체로 Spring JDBC와 Mybatis가 있다.
- 단점: 여전히 SQL의존적인 개발을 해야 하며, DB 벤더에 의존적이다.

#### JPA 
- 자바 진영의 ORM기술 표준/명세이다.
- ORM(Object-relational mapping, 객체 관계 매핑): 데이터를 객체처럼 사용할 수 있도록 사이에서 매핑을 해주는 계층
- ORM의 필요성
	- DB 벤더와 독립적 - 방언(dialect) 처리
	- 생산성, 유지보수, 패러다임 불일치 해결, 성능 최적화
- 대표적인 구현체로 하이버네이트가 있다.
- 단점
	- 성능 저하(잘 못 사용했을 때(N+1) or 용도와 맞지 않게 사용했을 떄, JPA는 일반적으로 OLTP에 맞춰져 있다.)
	- 복잡한 쿼리의 생성이 불가
		- JPQL(or JPQL 빌더인 QueryDSL)이나 SQL을 직접 호출하여 해결한다.
- Criteria과 QueryDSL
	- 복잡한 JPQL을 문자열이 아닌 자바 코드로 작성하고 컴파일 시점에 오류를 잡을 수 있게 도와준다.
	- Criteria는 JPA 공식 기능이나, 복잡하고 실용성이 떨어진다.
	- 실무에선 QueryDSL을 많이 사용한다.
		- [QueryDSL](http://querydsl.com/)은 JPA 뿐만 아니라 많은 기술을 지원하는 통합 쿼리 프레임워크이다.


# References
- 본문에 남긴 링크들
- 강의 - 자바 ORM 표준 JPA 프로그래밍 - 기본편
- 강의 - 스프링 DB 1편 - 데이터 접근 핵심 원리
- https://www.elancer.co.kr/blog/view?seq=231