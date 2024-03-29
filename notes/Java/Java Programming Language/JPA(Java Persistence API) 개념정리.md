
주의: 이 글에선 실제 엔티티를 정의하고 사용하는 법은 다루지 않음

### JPA 개념/등장

JPA(Java Persistence API)란, 자바 진영의 ORM 기술 표준이다.

SQL 의존적인 개발의 문제(객체지향적인 방식과 불일치 발생)을 해결하기 위해 데이터를 객체로 다루는 ORM이 등장하였다.

ORM은 DB를 객체로 다룰 수 있도록 중간에서 매핑해주는 프레임워크를 의미한다.

자바에선 표준 인터페이스로 JPA를 정의하였다. 

### JPA를 왜 사용해야 하는가?

- SQL 중심적인 개발에서 객체 중심으로 개발
- 생산성
- 유지보수
	- 필드를 추가하는 경우, SQL문을 여러 개 수정할 필요 없이 객체에 필드를 추가하기만 하면 된다.
- 패러다임의 불일치 해결
- 성능 최적화 기능
    1. 1차 캐시와 동일성(identity) 보장
        1. 같은 트랜잭션 안에서는 같은 엔티티를 반환 - 약간의 조회 성능 향상
        2. DB Isolation Level이 Read Commit이어도 애플리케이션에서 Repeatable Read 보장
    2. 트랜잭션을 지원하는 쓰기 지연(transactional write-behind)
    3. 지연 로딩(Lazy Loading)
- 데이터 접근 추상화와 벤더 독립성

### JPA는 Spring Data JPA와 다르다. 

- JPA를 사용하기 쉽게 해주는게 Spring Data JPA, JPA는 EntityManager와 트랜잭션을 직접 관리해야 한다.
- 또한 트랜잭션을 쉽게 관리할 수 있게 하는 `@Transactional`도 스프링에서 제공한다.
	- Java EE도 `@Transactional`(`javax.transaction.Transactional`)을 지원하는데? - Spring Tx와는 별개의 어노테이션이였음. 지원되는 것도 아니였고. 특정 버전(정확하진 않음, 4.2) 이후 부터 해당 어노테이션도 Spring에서 트랜잭션으로 지원함. 그리고 패키지를 보면 알겠지만 JPA의 기능이라고 보기는 어려움 - [참고](https://stackoverflow.com/questions/26387399/javax-transaction-transactional-vs-org-springframework-transaction-annotation-tr)
- J2EE 컨테이나나 Spring 환경에서 JPA를 사용하면 해당 프레임워크가 제공하는 기능을 따라야 한다.

### JPA 간단 정리
- 방언 (dialect)
	- 각 DB는 표준을 지키지 않는 고유한 기능(방언)을 가진다.
	- 이러한 DB의 방언을 `hibernate.dialec`에 저장해서 관리한다.
	- JPA는 이러한 방언을 사용해, 사용자가 특정 DB에 의존하지 않게 해준다.
- META-INF/persistence.xml
	- 해당 파일에 persistence unit을 작성한다.
	- EntityManager의 구성(configuration)이 포함된다.
	- 이 구성을 읽고 EntityMangerFactory를 생성한다.
	- ![](https://user-images.githubusercontent.com/52024566/133097530-c0572700-aa49-466a-961f-afc73a613367.png)
- [EntityMangerFactory](https://jakarta.ee/specifications/platform/8/apidocs/?javax/persistence/EntityManagerFactory.html)
	- EntityManger을 반환하고, 자원(커넥션 풀 포함)을 관리한다.
		- EntityManger을 줄 때, 커넥션을 연결해서 준다.
	- thread-safe하고, 여러 곳에서 재사용하여 사용된다.
- [EntityManger](https://jakarta.ee/specifications/platform/8/apidocs/javax/persistence/entitymanager)(persistence context)
	- persistence context와 연관된다.
		- 상황에 따라 persistence context와 EntityManager는 1:N, N:1, 1:1 관계가 될 수 있다.
		- 스프링의 경우, 트랙잭션 범위와 persistence context가 같다. 트랜잭션 안에서는 여러 EntityManager도 같은 context를 사용하고, 다른 트랜잭션에 있으면 한 EntityManager도 여러 context를 가진다.
	- thread-safe하지 않고, 데이터베이스 커넥션과 밀접한 관계가 있으므로 스레드 간에 공유하거나 재사용하지 않아야 한다.
- 영속성 컨텍스트(persistence context) 
	- JPA의 핵심 개념, 엔티티를 영구 저장하는 환경이다.
	- 어플리케이션과 DB 사이의 중간 계층이라고 볼 수 있다.
	- persistence context는 논리적인 개념으로 실체가 없다.
	- EntityManager를 통해 영속성 컨텍스트에 접근한다.
- 엔티티 생명주기
	- 영속성 컨텍스트에 의해 관리되며, 4가지 상태를 가진다.
	- 비영속(new/transient): 영속성 컨텍스트와 전혀 관계가 없는 상태
	- 영속(managed): 영속성 컨텍스트에 저장된 상태
	- 준영속(detacked): 영속성 컨텍스트에 저장되었다가 분리된 상태
	- 삭제(removed): 삭제된 상태
	- ![생명주기](https://user-images.githubusercontent.com/52024566/133262753-b1194492-286d-4ca9-b1ea-e905d68f57a0.png)
- 영속성 컨텍스트의 흐름
	- ![영속 컨텍스트](https://user-images.githubusercontent.com/52024566/133262740-10892bdd-d6fc-4f82-b79a-23bd3fc51f1f.png)
	- 1차 캐시에서 엔티티의 스냅샷, ID, 엔티티의 현재 상태를 관리한다.
		- JPA 조회 시 1차 캐시를 확인한다.
	- 수정/생성/삭제 요청은 flush 되기 전까지 쓰기 지연 SQL 저장소에서 buffer한다.
- flush(플러시)
	- flush가 되면 현재 상태를 DB와 동기화한다.
	- 동작
		- 1차 캐시에서 엔티티가 수정되었다면 DB에 수정된 정보를 반영한다. 이를 더티 체크라고 한다.
		- buffer헀던 SQL을 한번에 보낸다.
	- 발생 조건
		- JPQL 쿼리 실행
			- JPQL은 영속 컨텍스트를 읽지 않고 DB에 요청을 보내기 때문에 현재 작업 상태를 DB와 동기화해야 함
		- 트랜잭션 커밋 - 즉, flush는 커밋과는 별개이다. (트랜잭션은 `EntityManager.getTransaction()`을 통해서 `EntityTransaction`를 가져온다.)
		- 직접 메서드 호출
- JPA 연관관계 
	- 엔티티에서 다른 엔티티를 필드로 가지기 위해서 사용한다.
	- DB에서는 FK를 통한 연관관계가 맺어진다. FK 안 쓸수도 있는데 기본은 그럼)
	- 단방향, 양방향 연관관계를 맺을 수 있다.
	- 양방향은 실제 양방향이 아니다. 단방향을 양방향 처럼 보이게 만드는 것
		- 따라서 연관관계의 주인(외래 키를 가지는 엔티티)을 지정해줘야 한다.
		- `mappedBy` 가 붙지 않은 쪽이 주인(FK를 가짐)이다.
		- 자식 엔티티에서 주인 엔티티를 추가/수정 하는 경우 주인 엔티티만 추가되고 자식 엔티티는 null로 저장될 수 있다. (따라서 두 방향 다 입력해주는게 좋음)
	- 조인 테이블
		- M:N 해소를 위해서 중간 테이블인 조인 테이블을 사용하는게 좋다. (중간 연결에 여러 데이터가 필요해질 수 있기 때문이다.)
		- 이때, 두 엔티티 간의 같은 관계가 성립되지 않게 복합 유니크 제약조건을 걸어야 한다.
	- 그 외 여러 매핑 기능을 제공한다.
		- 복합키, `@MappedSuperclass`, 상속 관계 매핑 등...
- 값 타입의 문제와 해결방법(불변)
	- 엔티티의 특정 값으로 객체(`@Embadded`)를 저장할 수 있다.
	- 단순 객체를 사용할 수도 있고, 컬렉션을 저장할 수도 있다.
		- 단, 값 자체를 의미하므로 식별자 없이 엔티티에 생명주기를 의존한다.
	- 여러 엔티티에서 공유하면, 한 곳에서의 수정이 다른 곳에도 영향을 준다. 이런 문제를 예방햐기 위해 불변으로 사용하는게 좋다.
- JPA 영속성 전이
	- 특정 엔티티를 영속 상태로 만들때, 연관된(자식) 엔티티에게도 상태를 전파하는 것
		- CaseCade를 사용할 수 있다.
		- ALL, PERSIST, REMOVE, MERGE, REFRESH, DETACH 가 있다.
			- 삭제 시 함께 삭제, 생성 시 함께 생성 등
	- `orphanRemoval = true`
		- 고아 엔티티를 지운다.
		- 한 엔티티에 의해서만 참조되어야 정상적으로 삭제된다. (참조가 있으면 고아 객체가 아니므로)
		- `CascadeType.REMOVE`, `orphanRemoval = true`
			- 부모 엔티티가 지워지면 자식 엔티티가 지워지는 것은 동일하다.
			- 부모 엔티티가 자식을 참조하지 않게 변경할 떄, `CascadeType.REMOVE`는 자식 엔티티를 삭제하지 않는다. `orphanRemoval = true`는 자식 엔티티가 고아가 되었으므로 삭제한다.
- JPA 복잡한 조회 문제 해결
	- Criteria과 QueryDSL - [Java - DB 관련 기술 - JDBC, SQL Mapper, JPA](notes/Java/Java%20Platform/Java%20-%20DB%20관련%20기술%20-%20JDBC,%20SQL%20Mapper,%20JPA.md) 참고
	- 네이티브 SQL 기능: SQL이지만, 영속성 컨텍스트 등 JPA의 기능 활용 가능
- Lazy와 N+1 문제
	- Proxy를 사용해서 연관관계의 다른 엔티티를 필요할 때 Lazy하게 가져올 수 있다. (필요없으면 로딩 안함)
	- 실무에서는 Lazy Loading을 기본으로 설정한다.
	- N+1 문제
		- 한 엔티티 조회 시, 그 엔티티의 관련된 엔티티를 조회하기 위해 N개의 쿼리가 발생하는 것
		- 결국 연관된 데이터가 필요한데, 한번에 하나씩 쿼리를 많이 날리는게 문제
		- 해결
			- fetch 조인 사용: 한 번에 연관된 엔티티를 가져옴
			- 하이버네이트 `@BatchSize`: 설정한 size 값 만큼 묶어서 가져옴
- `@Transactional(readOnly = true)`
	- 영속성 컨텍스트에서 스냅샷 비교 등 수정에 필요한 작업을 하지 않는다. (= 메모리 최적화)
	- DB에 readonly 요청을 보내므로 경우에 따라 효과적인 작업 수행 가능 (Replication 부하 분산, slave에 읽기 연산 수행)
	- 트랜잭션이지만, readolny 작업이라는 의미를 명시하는 효과도 있음
- 나는 연관관계 대신 id를 사용하는 걸 선호
	- 왜? N+1문제가 발생하지 않고, 직접적인 엔티티의 참조하지 않는다. 따라서 어떤 도메인의 DB가 변경되거나 서비스가 분리될 때 코드를 수정하지 않아도 된다. - 연관관계를 느슨하게 만드는 것
	- 물론 직접적인 의존관계 - 라이프사이클(생성/삭제)가 같은 경우면 사용하겠지만, 그 외에는 id를 통한 조회를 선호함

# References
- 강의 - 자바 ORM 표준 JPA 프로그래밍 - 기본편
