
## Synchronous-Asynchronous

작업을 호출한 스레드가 그 결과를 직접 처리하는지 여부를 의미하는 개념이다.    
I/O 뿐만 아니라 더 넓은 범위에 적용된다.

- #### 동기(Synchronous)
	- 작업을 호출한 스레드가 그 결과를 직접 처라한다.
	- 결과 처리 순서가 보장된다.
- #### 비동기(Asynchronous)
	- 작업을 호출한 스레드가 결과를 기다리지 않고, 다른 스레드/워커가 결과를 처리한다.
	- 순서 보장이 어렵다. 직접 처리하느냐 하지 않느냐에 따라 달라진다. (동기 방식보다 디버깅, 코드 구현이 복잡하다.)

#### 비동기 사용 예시
- Node.js는 싱글 스레드 Event Loop를 사용하며, 비동기 I/O System Call과 Worker Thread를 통해 비동기 Callback 작업을 수행한다.
- WebFlux도 Event Loop 방식을 사용하여 Async/NonBlocking이다. 다만 I/O 처리를 하는 DB Driver가 아직 비동기를 많이 지원하지 않는다.
- 비동기를 사용하는 주된 이유는 호출자 스레드의 트래픽에 영향을 받지 않고 별도로 결과를 처리할 수 있어 시스템 확장성(scalability)이 좋아지기 때문이다.
- 외부 메시지 큐잉 시스템(예: Apache Kafka)을 사용하는 것도 비동기 처리에 해당된다.

## Blocking-NonBlocking

I/O 작업을 수행할 때 CPU의 작업 대기 여부를 의미한다.   
즉, 호출되는 함수가 바로 리턴하는지 여부로도 볼 수 있다.     
제어권이라는 개념으로 설명하기도 한다. (결과와 상관없이 바로 리턴하면 비동기)

- #### 블로킹(Blocking) I/O
	- 작업을 요청한 스레드가 해당 작업의 완료를 기다리며 블록(blocked) 상태가 된다.
	- 작업이 완료될 때까지 스레드는 다른 작업을 수행할 수 없고 대기 상태이다.
- #### 논블로킹(Non-Blocking) I/O
	- 작업을 요청한 스레드가 작업 완료를 기다리지 않고, 다른 작업을 계속 수행할 수 있다.

## 조합 설명
![img](https://blog.kakaocdn.net/dn/b98rl5/btroVrU8BYn/RJYgbIGAMqXY6gVa1k5R3k/img.jpg)

- **Blocking-Sync**
	- 호출하고 결과(Sync)를 받을 수 있을때까지 대기(Blocking)한다.
- **Blocking-Async**
	- 호출 후 결과를 받을 때 까지 대기 (Blocking)
	- 결과를 호출 스레드에서 처리하지 않아도 된다. (ASync)
	- Async의 의미가 없는 작업
	- 대표적으로 MySQL 동기 Driver와 Node.js(비동기 호출)의 조합
- **NonBlocking-Sync**
	- 호출 후 바로 리턴 받는다. (NonBlocking)
	- 결과를 호출 스레드에서 처리해야 하므로 계속 확인한다. (Sync)
	- 이러한 작업을 Polling이라고 볼 수 있을 것 같다.
- **NonBlocking-Async**
	- 호출 후 바로 리턴 받는다. (NonBlocking)
	- 결과를 호출 스레드에서 처리하지 않아도 된다. (ASync)
	- 일반적으로 비동기 작업 or 논블로킹 작업이라고 부르면 여기 해당된다.
	- 주로 Event-Driven이나 외부 Message Queue 등으로 사용한다.

# References
- [Node - The Node.js Event Loop](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick)
- [Node.js 이벤트 루프(Event Loop) 샅샅이 분석하기](https://www.korecmblog.com/blog/node-js-event-loop)
- [Spring WebFlux는 어떻게 적은 리소스로 많은 트래픽을 감당할까?](https://devahea.github.io/2019/04/21/Spring-WebFlux%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%A0%81%EC%9D%80-%EB%A6%AC%EC%86%8C%EC%8A%A4%EB%A1%9C-%EB%A7%8E%EC%9D%80-%ED%8A%B8%EB%9E%98%ED%94%BD%EC%9D%84-%EA%B0%90%EB%8B%B9%ED%95%A0%EA%B9%8C/)
- [HomoEfficio - Blocking-NonBlocking-Synchronous-Asynchronous](https://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/)
