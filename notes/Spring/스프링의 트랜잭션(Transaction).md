
### 추상화

여러 DB 관련 구현체마다 트랜잭션 관리 방법/예외 등 구현 방식이 다르다.

따라서 스프링인 순수한(특정 구현에 의존하지 않는) 비즈니스 로직을 위해 추상화한다. 

`org.springframework.transaction.PlatformTransactionManager`를 사용해 추상화 된 트랜잭션을 제공한다.

![스프링 트랜잭션 추상화 다이어그램](https://user-images.githubusercontent.com/52024566/194064254-2764c447-95b6-4991-a630-efdb8d2df11a.png)


### 트랜잭션 동기화 

트랜잭션을 유지하려면 트랜잭션의 시작부터 끝까지 같은 데이터베이스 커넥션을 유지해아 한다.

그래서 추상화를 사용하지 않으면 서비스 계층에서 커넥션(트랜잭션)을 연결하고, DB 접근 계층으로 전달해서 처리하게 해야만 했다.

스프링은 트랜잭션 매니저와 "트랜잭션 동기화 매니저"라는 걸 제공한다. 

트랜잭션 동기화 매니저는 `ThreadLocal`을 사용해서 커넥션을 동기화하고,    
트랜잭션 매니저는 내부에서 트랜잭션 동기화 매니저를 사용한다.

(아마 강의에서는 동기 서비스 기준으로 설명하는 것 같고, 반응형 프로그래밍에서는 `org.springframework.transaction.ReactiveTransactionManager`를 사용한다. 그리고 `ThreadLocal`도 사용할 수 없다. - 참고 [스프링 공식문서](https://docs.spring.io/spring-framework/reference/data-access/transaction/event.html) `... uses the Reactor context instead of thread-local variables ...`)

- 트랜잭션 흐름
	- 트랜잭션 시작 ![트랜잭션 시작](https://user-images.githubusercontent.com/52024566/194569724-646726ce-e0e4-4f37-a9b7-e90b980f8e71.png)
		- 서비스 계층에서 트랜잭션 시작
		- 트랜잭션 매니저가 커넥션을 생성하고 동기화 매니저에게 저장, 동기화 매니저는 쓰레드 로컬에 커넥션을 보관 (thread-safe함)
	- 로직 실행 ![로직 실행](https://user-images.githubusercontent.com/52024566/194569728-b55e1aae-267e-4a53-ad79-a1475f840901.png)
		- 서비스 계층에서 커넥션을 전달하지 않음.
		- DAO가 `DataSourceUtils.getConnection()`을 사용해서 트랜잭션 동기화 매니저에 보관된 커넥션을 꺼내서 사용.
	- 트랜잭션 종료 ![트랜잭션 종료](https://user-images.githubusercontent.com/52024566/194569731-93a77cfd-6dd0-4370-a042-1f309f633f31.png)
		- 서비스 계층에서 트랜잭션 매니저를 통해 롤백하거나 커밋함
		- 전체 리소스 정리
			- 쓰레드 로컬을 사용하므로(정리해야 함) 사용이 끝난 트랜잭션 동기화 매니저를 정리한다.
			- 커넥션을 커넥션 풀에 반환한다.

### AOP 사용

하지만 여전히 서비스 계층에서 매번 트랜잭션을 관리해야한다.

스프링은 AOP를 제공하여 `org.springframework.transaction.annotation.Transactional`이 붙은 클래스(의 모든 메서드), 메서드를 확인하고 해당 메서드에 트랜잭션을 프록시를 자동으로 적용해준다.

![프록시 도임](https://user-images.githubusercontent.com/52024566/195048961-4142fff5-6032-4c09-8962-552a6cd7fd86.png)

### 전체 구조

- 트랜잭션 호출 방법/예외처리(후술할 내용) 추상화
- 트랜잭션 동기화(관리)를 통해 서비스에서 커넥션이 깊게 관여하지 않음
- AOP를 사용하여 반복되는 트랜잭션 관심사 분리

![전체 구조](https://user-images.githubusercontent.com/52024566/195048963-9a1a940d-b8cd-4672-a635-8e0dcd342172.png)

### 예외처리

발생하는 예외도 각 구현체마다 다르다.

스프링에서는 특정 상황에서 던져지는 에러를 매핑하여 어떤 구현체에 의존하지 않게 만든다.

수많은 예외를 구현해서 일관되면서 자세한 예외 계층을 제공

![예외 계층](https://user-images.githubusercontent.com/52024566/198304759-949f86db-80a6-4864-a908-0b37a12736ee.png)

### 테스트
실제 데이터베이스에 접근해서 데이터를 잘 저장하고 조회할 수 있는지 확인하는 테스트가 필요하다.    

(단위 테스트가 아니므로 `@SpringBootTest`를 사용해서 스프링 컨테이너를 로드해서 통함 테스트를 진행한다.)   
(Spring Boot에서는 기본적으로 자동으로 사용 가능한 in-memory DB를 사용해서 DB를 구성한다. 다른 설정을 원한다면 직접 구성하면 된다.)

이 때, 아무 환경에서 문제 없이 실행되도록 로컬 DB를 사용하거나, 테스트 환경에서 잘 동작하도록 테스트 용 DB 설정을 따로 구성한다.

또 이런 테스트 동작은 확인 이후에 DB에 반영되지 않고 롤백되어야 하는데, 스프링은 테스트에서 `@Transactional`을 사용하면 발생했던 모든 작업을 테스트가 끝나고 롤백한다.


### 스프링 트랜잭션과 트랜잭션 전파

#### 스프링 트랜잭션

실제 서비스 객체 대신 프록시 객체를 스프링 컨테이너가 빈으로 등록한다. (다형성)

![스프링 컨테이너에 프록시 등록](https://user-images.githubusercontent.com/52024566/204138774-c016d326-b5df-4657-b7fa-bd9aaea3e499.png)

##### 프록시와 내부 호출 시 문제
프록시는 메서드가 실행되었을 때 트랜잭션을 확인하고, 실제 서비스를 호출한다. 
이후 실제 서비스가 내부적으로 트랜잭션이 걸린 객체를 호출하더라도 적용되지 않는다.
![트랜잭션이 적용되지 않는 상황](https://user-images.githubusercontent.com/52024566/204537738-21b97be9-e6f3-4d51-8d9a-19976be6f100.png)

이런 경우, 위 예시의 `external()`에 트랜잭션을 적용한다.    
적용할 수 없다면, `internal()`를 수행하는 객체를 만들어 별도의 객체로 분리한다.    
(이 때는 또 다른 프록시 객체가 `internal()`의 트랜잭션을 실행하므로 잘 동작한다.)

##### 예외와 커밋,롤백
예외 발생시 스프링 트랜잭션 AOP는 예외의 종류에 따라 트랜잭션을 커밋하거나 롤백한다.
- 기본 전략
	- 언체크 예외인 `RuntimeException`, `Error`와 하위 예외 발생 시 롤백
	- 체크 예외인 `Exception`과 그 하위 예외가 발생 시 커밋
	- 왜 그런가?
		- 스프링이 기본적으로 체크 예외는 비즈니스 의미가 있을 때, 언체크 예외는 복구 불가능한 예외에서 사용한다고 가졍했기 때문이다.
- 커스텀
	- `@Transactional`의 `rollbackFor`, `noRollbackFor` 옵션을 수정하여 특정 예외 발생 시 롤백/커밋 행동을 변경할 수 있다.

#### 트랜잭션 전파

트랜잭션 도중 다른 트랜잭션을 수행할 수 있다.

그 때, 기존에 트랜잭션이 있는 상황에서 어떻게 참여할 것인가를 결정하는게 트랜잭션 전파(propagation)이다.

여러 옵션이 있지만, `REQUIRED`와 `REQUIRES_NEW`만 알아도 나머지는 이해가능. (정확히는 논리/물리 트랜잭션)

`REQUIRED`가 기본이다.

#### 물리 트랜잭션, 논리 트랜잭션

스프링은 이해를 돕기 위해 논리 트랜잭션과 물리 트랜잭션이라는 개념을 나눈다.

일반적인 하나의 커넥션을 가지는게 물리 트랜잭선, 실제 커넥션을 통해서 트랜잭션을 시작하고 커밋/롤백한다.

논리 트랜잭션은 트랜잭션 매니저를 통해 트랜잭션을 사용하는 단위. 기존 트랜잭션에 참여하는 경우, 논리 트랜잭션이 된다.

두 트랜잭션은 다음과 같은 원칙을 가진다.
- 모든 논리 트랜잭션이 커밋되어야 물리 트랜잭션이 커밋된다.
- 하나의 논리 트랜잭션이라도 롤백되면 물리 트랜잭션은 롤백된다.
	- (다른 논리 트랜잭션이 커밋되었어도 함께 롤백된다.)

![물리/논리 트랜잭션](https://user-images.githubusercontent.com/52024566/207364035-d1f4d41f-d4ec-4c40-b7f6-a0b017b92b0e.png)

내부적으론 다음과 같이 전파된다.

![트랜잭션 전파 과정](https://user-images.githubusercontent.com/52024566/207610656-482d77ec-8106-479b-a4a9-7b6f8271f898.png)


##### REQUIRES_NEW
위의 설명은 기본 옵션인 `REQUIRES`이다. `REQUIRES_NEW`는 조금 다른 전파 작업을 수행한다.

`REQUIRES_NEW`는 기존 트랜잭션이 있는 상황에서 다른 트랜잭션에 참여할 때, 새로운 물리 트랜잭션을 생성하고 해당 물리 트랜잭션에 참여한다.

![REQUIRES_NEW 트랜잭션](https://user-images.githubusercontent.com/52024566/208439511-5ba08516-8ce6-4f00-8381-c4cd1697118d.png)

별개의 물리 트랜잭션이므로, 별개의 커넥션을 가지고, 에러가 발생해도 영향을 주지 않는다.
![REQUIRES_NEW 내부 동작](https://user-images.githubusercontent.com/52024566/208439515-0465962c-9649-4925-87e4-7448b955d507.png)

##### 트랜잭션 전파 종류
![트랜잭션 전파 전략 종류](https://img1.daumcdn.net/thumb/R800x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbFg3yL%2FbtrIH6dAJRK%2FNy6PUAZR9kFd6k7x3tkiM0%2Fimg.png)

##### 트랜잭션 전파와 옵션
`isolation`, `timeout`, `readOnly`는 물리 트랜잭션이 시작될 때만 적용된다.     
논리 트랜잭션으로 참여할 때는 해당되지 않음.

### 그 외

##### 선언적 트랜잭션 관리 vs  프로그래밍 방식 트랜잭션 관리

- 선언적 트랜잭션 관리
	- 일반적으로 사용하는거, @Transactional, XML 등으로 트랜잭션 관리
	- 편하다.
- 프로그래밍 방식 트랜잭션 관리
	- 직접 트랜잭션을 코드로 관리하는 것
	- AOP, 스프링 컨테이너 없이 사용 가능
		- 일반적으로 이런 경우가 많이 없음
		- (내 생각: 트랜잭션이 아주 제한적으로 쓰이는 서비스라면 사용할지도?)
	- 테스트 시에 가끔 사용한다고 함


# References
- 인프런 강의 - 스프링 DB 1편 - 데이터 접근 핵심 원리
- [누가 해당 강의 정리한 블로그](https://melodist.github.io/docs/Spring/DB1)
- [[Spring] 스프링의 트랜잭션 전파 속성(Transaction propagation) 완벽하게 이해하기](https://mangkyu.tistory.com/269)
