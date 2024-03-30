`ThreadLocal`은 스레드 단위(~ per thread)로 로컬 변수를 사용할 수 있게 해준다.

주의점 
1. static을 사용해야 한다. static으로 사용해야만 스레드 단위로 상태를 저장할 수 있다. (API Docs를 보면 알겠지만, 의도 자체가 이쪽 방향이다.)
2. Thread Pool에서 사용하는 경우, Thread를 반환할 때 초기화해주어야 한다.

- 내부 구조
	- 상태
		- `ThreadLocal`은 static inner class `ThreadLocalMap`을 가진다.
		- `Thread`는 `ThreadLocalMap`를 필드로 가진다.
			- `ThreadLocalMap`는 스레드의 변수/상태를 저장한다.
	- 동작 원리 - 코드를 보는게 이해가 잘 된다.
		- `ThreadLocal<T>`가 `get()`, `set()` 등을 실행한다.
		- `Thread.currentThread()`로 현재 스레드를 구하고 그 스레드의 `ThreadLocalMap`를 구한다.
		- `ThreadLocalMap`에서 `this(ThreadLocal)` 를 key 값으로 `T`를 찾거나 저장한다.


예제 코드
```Java
// get 부분만 설명
public T get() {
	// 현재 실행중인 스레드 t의 ThreadLocalMap map을 구한다.
    Thread t = Thread.currentThread();
    ThreadLocalMap map = getMap(t);
    if (map != null) {
	    // map(이 스레드의 상태 저장하는 ThreadLocalMap)에서 
	    // this(ThreadLocal)를 Key 값으로 T를 가져온다.
        ThreadLocalMap.Entry e = map.getEntry(this);
        if (e != null) {
            @SuppressWarnings("unchecked")
            T result = (T)e.value;
            return result;
        }
    }
    // 결과가 없으면 기본값으로 설정
    return setInitialValue();
}
```


- 사용 예시
	- Spring
		- ~ContextHolder의 전략으로 (내가 알기론) 무조건 제공
			- 이건 스프링이 일반적인 웹 요청이 request per thread라서 그런걸로 알고 있음. 전략이니까 다른 전략을 사용하면 사용 안할 수도 있고. 
			- `RequestContextHolder`: `HttpServletRequest` 저장
			- `SecurityContextHolder`: `SecurityContext` 저장, `MODE_THREADLOCAL`
		- `TransactionSynchronizationManager`: 스프링 트랜잭션에서 Service에서 Connection을 직접 관리하지 않고, DAO에서 Connection을 가져와서 쓰도록 트랜잭션을 저장하는 객체. ThreadLocal을 사용한다. 

좀 별개의 이야기지만, 반응형/비동기의 경우 thread 단위 작업이 아니라서 `ThreadLocal`만을 사용해서 구현하는건 어렵다고 알고 있음. 반응형/비동기가 어떻게 구현되어 있는지는 모르겠는데.

# References & 추천 자료
- [Thread의 개인 수납장 ThreadLocal](https://dev.gmarket.com/62)
- [ThreadLocal을 알아보자!](https://catsbi.oopy.io/3ddf4078-55f0-4fde-9d51-907613a44c0d) - 여기 그림이 맞는지는 모르겠다. 아마 아닌거 같은데. 사용 예시 부분이 좋음
- [[Java] 스레드 로컬(ThreadLocal)과 상속 가능한 스레드 로컬( InheritableThreadLocal)에 대하여](https://mangkyu.tistory.com/333)
- [자바 ThreadLocal: 사용법과 주의사항](https://madplay.github.io/post/java-threadlocal)
- [Java 17 ThreadLocal](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/ThreadLocal.html)
- [Why should Java ThreadLocal variables be static](https://stackoverflow.com/questions/2784009/why-should-java-threadlocal-variables-be-static)