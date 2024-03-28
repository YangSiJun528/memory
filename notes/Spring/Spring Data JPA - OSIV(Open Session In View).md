Spring Data JPA나 J2EE 등 프레임워크에서 제공하는 기능이다.     
(JPA의 기능을 사용하는건 맞는데, 생명주기를 프레임워크가 관리하므로 프레임워크가 제공하는 기능이라고 보는게 맞다고 생각해서 이렇게 표현함)

(JPA에서는 OEIV(Open EntityManager In View) 하이버네이트에선 OSIV(Open Session In View)라고 보는게 맞는 것 같다. 하지만 일반적으로 OSIV라고 부른다.)

OSIV(Open Session In View)는 영속성 컨텍스트(EntityManager)를 View까지 열어둔다는 의미이다.

클라이언트의 요청이 들어오면, 서블릿 필터나 스프링 인터셉터에서 영속성 컨텍스트를 생성한다. (선택 가능)

따라서 View 영역(트랜잭션 범위 바깥)에서도 Lazy Loading을 사용해서 다른 엔티티를 가져올 수 있다.    
단, 트랜잭션 범위 바깥이므로 수정 작업은 불가능하다.

이처럼 읽기 작업은 트랜잭션이 없어도 되는데, 이를 **트랜잭션 없이 읽기(Nontransactional reads)** 라고 한다.

트랜잭션 범위 바깥에는 flush를 수행하지 않고, 바로 EntityManager를 닫아버린다.

![OSIV 사용](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbd835C%2FbtqTPzbjhqa%2FAPhn7gWwr4pRhzCL79N4q1%2Fimg.png)

스프링에선 OSIV를 기본적으로 사용하지만, 옵션을 설정해 비활성화 할 수 있다. `spring.jpa.open-in-view` property 사용.      
(이 경우 트랜잭션 범위와 영속성 컨텍스트 생존 범위가 동일하다.)

![OSIV 미사용](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FemsRZ9%2FbtqTLyKOQQ7%2F5cKM4Ma00A7LYKwhde77Zk%2Fimg.png)
##### 단점
- 영속성 컨텍스트를 여러 트랜잭션이 공유할 수 있다.
	- 웹 요청 단위로 영속성 컨텍스트가 유지되기 때문.
- 장기간 데이터베이스 커넥션을 점유한다.
	- 실시간 트래픽이 많은 애플리케이션에서는 커넥션 부족 현상이 발생할 수 있다.
	- Lazy Loading을 위해서 계속 커넥션을 점유해야 한다.

# References
- 책  - 자바 ORM 표준 JPA 프로그래밍
- [JPA - OSIV(Open Session In View) 정리](https://ykh6242.tistory.com/entry/JPA-OSIVOpen-Session-In-View%EC%99%80-%EC%84%B1%EB%8A%A5-%EC%B5%9C%EC%A0%81%ED%99%94)
