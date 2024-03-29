
JPA를 편리하게 사용하도록 도와주는 도구, Spring에서 제공한다.

- EntityManager를 직접 관리하지 않아도 됨
- 스프링 예외 추상화를 지원
- 공통 인터페이스 기능 지원
	- `JpaRepository`를 사용해서 기본적인 CRUD 기능 제공 ![상속 구조](https://user-images.githubusercontent.com/52024566/200572119-fbf39588-8ca8-4ee5-a044-f061ab1eae82.png)
	- 쿼리 메서드 기능 제공: 메서드 이름을 분석해서 자동으로 쿼리를 만들어줌
		- Embadded 타입도 쿼리로 사용하는 등, 여러 복잡한 쿼리도 가능(대신 이름이 길어진다.)
	- JPQL 작성: 메서드 위에 `@Query`를 사용하여 작성 가능
	- JPA의 네이티브 쿼리 기능 지원: SQL을 직접 작성할 수 있다.
- 런타임 동작
	- `JpaRepository`를 구현하는 `SimpleJpaRepository`를 사용
	- (예시) 작성한 `JpaRepository` 를 상속하는 `AbcRepository`의 구현체인 `AbcRepositoryProxy`를 사용
	- `AbcRepository`를 사용하는 서비스 객체는 인터페이스를 의존하므로 이런 변경을 알 필요 없음
	- ![예시](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F995883335CC5630C16)
	- 참고로 `SimpleJpaRepository`는 `@Repository`와 `@Transactional`을 적용하고 있다. 
		- 따라서 `JpaRepository`를 상속받는 인터페이스는 `@Repository`를 사용하지 않아도 되고,
		- 꼭 필요하지 않다면 `@Transactional`을 사용하지 않아도 된다.
