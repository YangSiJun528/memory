
인텔리제이 기준으로 설명. 아마 다른 IDE도 사용법만 다르지 대부분 기능은 지원해줄거 같긴 함.

## 디버깅이란?

> 디버깅은 모든 소프트웨어에서 소스 코드의 오류 또는 버그를 찾아서 수정하는 과정입니다. 소프트웨어가 예상대로 작동하지 않으면 컴퓨터 프로그래머는 오류가 일어나는 원인을 알아내기 위해 코드를 분석합니다. 이들은 디버깅 도구를 사용해 소프트웨어를 제어된 환경에서 실행하고 코드를 단계별로 확인하여 문제를 분석하고 수정합니다.
> -AWS 개발자 문서-

## 디버깅 방법과 팁

- 마인드 셋? (뭐라고 해야 할지 모르겠네)
	- 컴퓨터는 논리적이므로 버그에는 어떤 논리적인 이유가 있다.
	- 지속적으로 노력하면 문제는 해결 된다.
	- 한계(시간, 역량)을 알고, 필요하다면 다른 사람에게 도움을 받자.
	- 버그에는 우선순위가 있다. 우선순위가 높은 문제부터 해결하려고 해야 한다.
	- 문제가 해결되지 않는다면, 시간을 두고 떨어져서 생각하거나, 팀원들과 공유하기

- 버그 신고 단계 - 많은 정보 구하기
	- 최대한 많은 정보 제공받기
		- 스크린샷이나 녹화, 버그가 발생하는 (재현) 조건 등, 사용자에게 많은 정보 구하기
	- 정보 구하기
		- 버그와 관련된 로그, 에러 등
- 재현 가능한 환경 구하기
	- staging server가 있는게 제일 좋음
- 버그 분석하기
	- log 남기기 or 디버깅하기: 에러가 발생하는 상황의 문맥 구하기
- 문제 재현이 불가능하다면?
	- 문제 재현이 어려운 이유
		- Production 환경에서만 발생할 수 있음.
		- 드물게 발생하는 Race Condition 문제.
		- 고객의 기기나 상태에 의존해서 발생할 수도 있음. (ex: 트위터 인플루언서 문제)
	- 어떻게 해결해야 하는가? 
		- 최대한 많은 정보 구하고 원인 분석하기 - 아래는 예시
			- 에러를 포함한 로그 계속 수집하기 (Prod 환경에 로그 추가로 남기기)
			- 에러가 발생하는 요청의 모든 생명 주기 분석하기

## window tools

- [Frames](https://www.jetbrains.com/help/idea/examining-suspended-program.html#examine-frames): thread 들의 call stack을 볼 수 있음.
- [Variables](https://www.jetbrains.com/help/idea/examining-suspended-program.html#variables): 현재 컨텍스트에서 사용 가능한 변수의 목록. 상태를 보거나 수정할 수도 있다.
- [Watches](https://www.jetbrains.com/help/idea/examining-suspended-program.html#watches): watch 들을 관리. watch는 사용자가 추가한 일종의 표현식으로, 
- Console: 디버거 출력 외에는 일반적인 실행과 동일함.
- [Threads](https://www.jetbrains.com/help/idea/examining-suspended-program.html#examine-frames): 살아있는 쓰레드 목록과 전환 가능
- [Memory](https://www.jetbrains.com/help/idea/analyze-objects-in-the-jvm-heap.html): 현재 힙에서 사용 가능한 객체에 대한 정보를 제공, 특정 시점 간 개수 변화나, 새로운 인스턴스 추적, 인스턴스 크기 파악 등 기능 제공
- [Overhead](https://www.jetbrains.com/help/idea/monitor-debugger-overhead.html): 디버거 기능의 리소스를 모니터링, 디버그 성능 최적화 목적. (잘 안쓸듯?)

## Step (실행 제어)

- **Step Over (F8)**: 현재 코드 라인을 넘기고 다음 라인으로 이동.
- **Step Into (F7)**: 메서드 내부로 진입하여 세부 사항 확인.
- **Smart Step Into (Shift+F7)**: 여러 메서드 호출 중 특정 메서드로 진입.
	- 한 줄에 여러 메서드 호출이 있는 경우 사용, 화살표나 키보드, 마우스 클릭 등으로 원하는 메서드 선택
- **Step Out (Shift+F8)**: 현재 메서드를 빠져나와 호출한 곳으로 이동.
- **Run to Cursor (Alt+F9)**: 커서 위치까지 실행.
- **Force Step Into/Over/Run to Cursor**: 특정 조건 무시하고 강제로 실행.
	- Force Step Into: 기본 자바 API는 Step Into 무시되지만, 강제로 Step Into 실행
	- Force Step Over: 현재 하이라이트(실행 이전에 멈추는 코드)된 부분에서 메서드 호출을 무시하고 넘어가기.
	- Force Run to Cursor: 중간 Breakpoint 무시하고 커서 위치까지 넘기기
- **Reset frame**: 콜스택을 이전 단계로 복원 가능. 단, 지역 변수를 제외한 정적/인스턴스 변수의 값을 되돌리지 않는다는 점 주의.

## 중단된 프로그램 검사(Examine suspended program)

[링크](https://www.jetbrains.com/help/idea/examining-suspended-program.html)

- Frame,Variables Window 등에서 사용하는 아이콘 설명하는데, 정리는 안함.

#### Evaluate expressions

Evaluate(평가하다) expressions(표현식, 조건문이나 메서드 같은거)

디버깅 동안 표현식을 수행해서 현재 상황의 추가 세부 정보나 실행 시나리오 테스트 가능

테스트할 코드 영역을 드래그하고, Setp ~ 아이콘들과 함께 있는 계산기같은 아이콘을 클릭해서 사용 가능

## 그 외 공식문서 설명

- Stream API 디버깅 방법
- 비동기 디버깅 방법 (메타 정보를 주입해주는 어노테이션 정의 필요)
- 원격 제어, 상황 제어(에러 던지기, 강제 값 반환 등)

## 내 기준 핵심 포인트

- Call Stack 참고하기
	- 현재 코드의 문맥 확인하기 좋음
	- Step Out 하면 Call Stack이 한 칸 낮아짐 (현재 메서드 빠져나옴)
- Evaluate expressions 활용하기
	- 간단한 표현식 실행 가능 (~ 상황에서 이런 코드가 잘 동작하는지 검증 가능)
- Breakpoint 추가 기능 활용하기
	- Conditional Breakpoint
	- Exception Break
- Memory(JVM Heap) 기능 참고하기
	- 문제 해결에 도움은 안 돼도, 메모리 관점에서 이해도 쌓기 좋을듯?

# References 겸 북마크
- [Debug Java Like a Pro in IntelliJ IDEA](https://www.youtube.com/watch?v=IeUZZoZE3sU&list=LL)
- [IntelliJ IDEA Docs - Debug code](https://www.jetbrains.com/help/idea/debugging-code.html) << 주로 참고
- [What is Debugging?](https://aws.amazon.com/ko/what-is/debugging/)
- [IntelliJ IDEA Youtube - Debugger](https://www.youtube.com/playlist?list=PLPZy-hmwOdEUWF85MuwrKV8YVWLmZW4ZA)
