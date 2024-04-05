
### 구조
- Workflows
	- 워크플로우는 하나 이상의 작업을 실행하는 구성 가능한 자동화된 프로세스입니다.
	- 이벤트, 수동, 일정(스케줄링)에 따라 트리거됨
- Event
	- Workflows를 실행하는 트리거, Repository의 특정 활동으로 발생한다.
	- 푸시, 특정 일정, 머지 등
- Jobs
	- 한 workflow의 일련의 단계
	- 동일한 Runner에서 동작한다.
	- 다른 작업과 종속적으로 실행되거나 병렬(기본)로 실행될 수 있다.
- Steps
	- 하나의 Job이 가지는 여러 순차적인 작업 단계
- Actions
	- 복잡하고 자주 반복되는 작업을 재사용 가능하게 정의한 코드
	- Marketplace에서 찾거나 직접 만들 수도 있다.
		- 레포지토리 형태로 저장된다. [커스텀 예시](https://smartstudio.tech/custom-github-actions/)
- Runner
	- 워크플로를 실행하는 서버이다. (가상머신, 컨테이너 등 독립된 환경)
	- 한번에 하나의 Job을 실행할 수 있다.
	- 더 큰 Runner를 사용하거나, 본인이 호트팅하여 Runner를 돌릴수도 있다.

# References
- [Understanding GitHub Actions](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions#workflows)
- [GitHub Actions의 소개와 핵심 개념](https://www.daleseo.com/github-actions-basics/)