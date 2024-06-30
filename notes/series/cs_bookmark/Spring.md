## 공통

검증 목적이라면 공식문서가 최고고, 공부나 이해 목적이면 시니어 개발자의 강의가 좋음.

강의를 구매하지 않았으면, "강의를 정리한 블로그 + 공식문서 검증" 을 추천

## Spring Batch

![](https://terasoluna-batch.github.io/guideline/5.0.0.RELEASE/en/images/ch02/SpringBatchArchitecture/Ch02_SpringBatchArchitecture_Architecture_StepTaskletFlow.png)

- 구조
	- 실행: 주로 외부 스케줄러에 의해 실행 (cron, jenkins, github actions 등)
		- 입력 받은 배치 작업을 수행하고 끝나면 종료됨. 항상 켜저있는 웹 서버와 다른 생명주기
	- JobRepository
		- JobInstance에 대한 meta 정보 저장, 실패 여부, 수행 시간 등
	- JobLauncher
		- Job을 실행하는 역할
	- JobExecution & StepExecution
		- 실행중인 Job & Step의 상태를 저장하는 역할
	- JobInstance: 실행되는 Job
	- JobParameter : Job 실행 시 부여되는 파리미터 값
	- Job : 배치 작업 - Step의 결과 여부 등에 따라 전체 Flow를 설정 가능
		- Step : Job의 세부 작업 
			- Tasklet : Step이 수행하는 작업, ChunkOrientedTasklet 아니면 그냥 로깅 같은 용도로 사용
			- ChunkOrientedTasklet : Chunk(조각?) 단위로 나눠서 수행, 주로 외부 데이터(DB)를 나눠서 가져올 때 사용

- 기본적으로 각 작업은 동기적으로 수행됨 (정확하진 않음)

- JPA는 배치랑 별로 안 어울린다. bulk도 지원 안하고 영속성 매니저 등... 배치 환경과는 적절하지 않다.
	- cursor도 DB 단 cursor를 제공하지 않고, (하이버네이트는 제공하는 듯?)

- https://terasoluna-batch.github.io/guideline/5.0.0.RELEASE/en/Ch02_SpringBatchArchitecture.html
	- 스프링 배치 잘 소개하는 글
- https://d2.naver.com/helloworld/9879422
	- 네이버 기술 블로그 - 코틀린 DSL 기반으로 배치 구현하는 사내 라이브러리 소개
- https://jojoldu.tistory.com/category/Spring%20Batch
	- 이동욱 개발자 블로그 - 한국 스프링 배치 소개 중에는 제일 좋음 - 2021년도 글이라 최근 공식문서와 비교하면서 보기
- https://tech.kakaopay.com/post/spring-batch-performance/
	- 카카오 기술 블로그
	- 해당 글 세미나 영상 - https://www.youtube.com/watch?v=VSwWHHkdQI4&t=652s
- https://www.youtube.com/watch?v=2IIwQDIi3ys
	- 카카오 세미나 배치 소개 + 성능
- https://www.youtube.com/watch?v=_nkJkWVH-mo
	- 우아한 테크 세미나 - 이동욱 - 스프링 배치 소개부터 사용법까지 - 좀 예전 내용일 수도 있는데, 사용법에 대한 인사이트 얻을 수 있음

## Spring



## Spring Boot



## Spring Security

- 중요 포인트
	- Servelt 기준으로 프록시 필터 + 인증 필터
	- 비번 암호화 변경 가능한 거
	- 인증/인가 객체 생성/관리 방식

## Spring + DB

- 다음의 역할이 각자 다르다는 걸 알고, 설명할 수 있으면 됨
	- Spring TX
	- JPA/Hibernate
	- Spring Data JDBC/ Data JPA
	- QueryDSL / JOOQ

- 자료 추천
	- 김영한님 Spring DB + JPA 강의
		- 내 github private repository에 정리해둔 거 있음.

