- - -

# 주의점
깊게 알아보지 않고 간단하게 알아보았다.

공식문서도 튜닝 관련이고 구현을 깊게 다루는 건 찾기 어려워서 네이버 개발 블로그,자바 최적화 책 위주로 작성하였다.

더 자세히 알고 싶으면 영어 도서나 자료를 찾아봐야 한다.

# Java Garbage Collection

### 용어 정리

GC 설명하는데 필요한 용어와 의미 설명

##### STW(Stop-The-World)
> GC 사이클이 발생하여 가비지를 수집하는 동안에는 모든 애플리케이션 스레드가 중단됩니다.
> 따라서 애플리케이션 코드는 GC 스레드가 바라보는 힙 상태를 무효화할 수 없습니다. 단순 GC 알고리즘에서는 대부분 이럴 때 STW가 일어납니다.

대부분의 GC가 이러한 STW을 줄이면서도 효율적인 알고리즘을 구현하는 걸 목적으로 하며, STW를 줄이기 위해서 GC 튜닝을 진행하기도 한다.
##### Concurrent(동시성)
> GC 스레드는 애플리케이션 스레드와 동시(병행) 실행될 수 있습니다. 이는 계산 비용 면에서 아주, 아주 어렵고 비싼 작업인 데다, 실상 100% 동시 실행을 보장하는 알고리즘은 없습니다. 
> (생략)... 핫스팟의 CMS(Concurrent Mark and Sweep)는 사실상 '준 동시(mostly concurrent)' 수집기라고 해야 맞습니다.

여러 GC에서 GC 스레드와 애플리케이션 스레드와 동시에 실행될 수 있도록 동시성을 보장하고자 하나, 비용의 문제로 100%를 보장하지 못한다. 즉, STW가 발생한다.

##### Compaction(압축, 압착)
할당된 메모리(즉, 살아남은 객체들)는 GC 사이클 마지막 과정에서 연속된 단일 영역으로 배열되는데, 이러한 과정을 Compaction이라고 한다.

메모리 단편화(Fragmentation)를 해소하기 위해 사용한다.

힙 내부의 빈 공간을 줄이고, 연속적인 메모리 영역을 생성하여 메모리 할당 및 해제가 더 효율적으로 이루어질 수 있다.

##### Mark and Sweep
GC가 동작하는 기본적인 과정을 말한다.

1. **Mark**: GC Root(힙 외부에서 접근할 수 있는 변수나 오브젝트)에서 시작해 이 Root가 참조하는 모든 오브젝트, 또 그 오브젝트들이 참조하는 다른 오브젝트들을 탐색해 내려가며 마크(Mark)한다.
2. **Sweep**(청소하다, 쓸다): 가비지 컬렉터는 힙 내부를 전체를 돌면서 Mark되지 않은 메모리들을 해제(Reclaim)한다.

아래는 GC Root가 될 수 있는 것들이다.
1. 실행중인 쓰레드 (Active Thread)
2. 정적 변수 (Static Variable)
3. 로컬 변수 (Local Variable)
4. JNI 레퍼런스 (JNI Reference)
5. 스택 프레임(Stack Frame) 
6. 레지스터(끌어올려진hoisted (호이스트된) 변수)
7. (JVM 코드 캐시에서) 코드 루트
8. 로드된 클래스의 메타데이터
(책이랑 블로그 내용을 합친거라 내용이 겹칠수도 있음, 나중에 정확하게 알아보기)

##### [Weak Generational Hypothesis](https://docs.oracle.com/en/java/javase/11/gctuning/garbage-collector-implementation.html)(약한 세대별 가설)
> 이는 대부분의 물체가 짧은 시간 동안만 생존한다는 것을 나타냅니다.
> 
> (생략)...놀라울 정도로 많은 수의 응용 프로그램이 이러한 일반적인 모양을 가지고 있습니다. 대부분의 개체가 "어릴 때 죽는다"는 사실에 초점을 맞추면 효율적인 수집이 가능해집니다.

Weak Generational Hypothesis는 아래 2가지 가설(전제, 경향)을 말한다.
- 대부분의 객체는 금방 접근 불가능한 상태(Unreachable)가 된다.
- 오래된 객체에서 새로운 객체로의 참조는 아주 적게 존재한다.

대부분의 GC가 이러한 가설을 기반으로 구현된다.

## GC 알고리즘 종류

### 고전적인 구조

지금은 거의 사용하지 않는 GC지만, 이후 설명할 내용에도 연관되어 있다.

Heap 영역은 Young/Old 영역으로 나뉘어져 있다.(물리적으로 분리되어 있다. 또한 Heap에 Perm 영역이 있었으나 Java 8부터 제거되었다. [JDK8 버전 이후 제거된 JVM Permanent 영역](notes/Java%20Platform/JDK8%20버전%20이후%20제거된%20JVM%20Permanent%20영역.md) 참고)

![](notes/Java%20Platform/files/basic_heap_layout.png)

Young은 Eden, Survivor 0, Survivor1으로 나뉜다.

Minor GC 시 Old 영역에 있는 객체가 Young 영역의 객체를 참조하는 경우,  
Old에 존재하는 카드 테이블(card table)을 사용한다.  

Old 영역에 있는 객체가 Young 영역의 객체를 참조할 때마다 기록하고, Minor GC 시 이 카드 테이블만 확인하여 GC 대상을 식별한다.
	Serial, Parallel, CMS은 카드테이블을 사용한다.

- Young
	- 효율적인 GC를 위해 3개의 영역으로 나눈다.
	- Young 영역에 대한 GC를 Minor GC라고 한다. 영역이 적은만큼 빠른 편이다.
	- Eden
		- 새롭게 생성된 객체가 할당(Allocation)되는 영역
	- Survivor 0/1
		- 최소 1번의 GC 이상 살아남은 객체가 존재하는 영역
		- 0/1 두가지 영역이 서로 form, to 역할을 번갈아 수행. 한 번에 둘 중 하나의 영역만 사용한다.
		- 왜 하나의 영역만 사용하는가?
			- (Flab 멘토링 중 멘토분 말씀이라 신뢰성있는 문서(출처)는 없다. 또 요약하는 과정에서 잘못 이해했을수도)
			- 메모리 파편화를 막기 위해서 일종의 Compaction을 수행하기 위해서라고...
			- (내 생각) Sweep이후 Compaction을 따로 수행하는 것보다 더 효율적이라서 그런게 아닌가 싶다.
- Old
	- 일반적으로 Young Generation 개체에 대해 임계값이 설정되고 해당 Age(세대 카운트, generational count)가 충족되면 개체가 Old Generation으로 이동된다. (이를 Promotion,승급 이라고 한다.)
	- Young 영역보다 크게 할당되며, 영역의 크기가 큰 만큼 GC 횟수는 적으나 STW가 더 오래걸린다.
	- Old 영역에 대한 GC를 Major/Full GC라고 한다.

#### GC 수행과정
1. 새로 생성한 대부분의 객체는 Eden 영역에 위치한다. 이 겍체는 age가 0이다.
2. Minor GC가 발생하면 Eden과 Survivor from에서 사용 가능한 객체는 Survivor to로 이동한다. 살아남은 객체는 age가 증가한다.
3. 이 과정을 반복하다가 age가 임계치를 넘는 객체는 Old 영역으로 이동하게 된다.
### GC 알고리즘 

#### Serial GC
- GC를 처리하는 쓰레드가 1개 (싱글 쓰레드)라서 STW 시간이 길다.
- Minor GC 에는 Mark-Sweep을 사용하고, Major GC에는 Mark-Sweep-Compact를 사용한다.
#### Parallel/Parallel Old GC
- Serial GC와 기본적인 알고리즘은 같지만, GC를 멀티 쓰레드로 수행한다.
	- Parallel GC: Minor GC만 멀티 쓰레드, Java 8 이전의 디폴트 GC
	- Parallel Old GC: Minor, Major GC 전부 멀티스레드. Java 8의 디폴트 GC
- Serial GC에 비해 STW 시간 감소
- STW 시간이 힙 크기에 거의 비례한다는 주요 단점을 가진다.
#### CMS(Concurrent Mark Sweep) GC 
- CMS는 중단 시간을 최소화하기 설계되었다.
- 대신 애플리케이션 스레드 실행 중애 많은 작업을 하고 GC 대상을 파악하는 과정이 복잡하다.
	- 전반적으로 동시성 특성으로 인해 더 많은 CPU 시간을 사용한다.
- Compaction을 수행하지 않아 메모리 파편화가 발생한다.
- Java 9부터 Deprecated, Java 14에서는 제거되었다.

#### G1(Garbage First) GC
![](notes/Java%20Platform/files/garbage_collector_heap_layout.png)
- CMS GC를 대체하기 위해서 만들어 졌다.
	- 하드웨어가 점점 발전하면서 대용량 메모리에 적합한 솔루션을 제공하기 위해 나타났다.
- 기존의 고전적인 Heap 구조와 달리 Region 단위를 사용해 영역을 동적으로 할당한다.
	- Region의 단위는 Heap의 크기에 따라 1, 2, 4, 8, 16, 32, 64MB 크기의 영역을 가진다.
	- Region을 이용하면 각 세대를 불연속적으로 배치할 수 있고, 수집기가 매번 실행될 때마다 전체 가비지를 수집하지 않고 Region 단위로 수행할 수 있다.
- 주로 쓰레기로 가득 찬 영역의 공간을 회수하는 데 중점을 두기 때문에 Garbage First 이라고 부른다.
- 좀 더 자세한 정보
	- 영역을 절반 이상을 점유한 객체는 거대 객체(humongous object)로 간주하여 거대 영역(humongous region)이라는 별도 공간에 곧바로 할당된다. 이는 연속된 Region이다.
	- Available/Unused: 아직 사용되지 않은 Region
	- 캐시 테이블 대신, RSet(Remembered Set)을 사용해서 영역을 추적한다.

#### [ZGC](https://wiki.openjdk.org/display/zgc)(The Z Garbage Collector)
![](notes/Java%20Platform/files/ZGC_layout.png)
- Java 15에 release 되었다.
- 비세대적 접근 방식을 사용하여 Young/Old 객체를 구분하지 않는다.
- ZPage라는 논리적인 단위로 구분한다.
	- 이 ZPage는 G1 GC의 Region과 달리 크기가 동적이다.
- 대량의 메모리8MB ~ 16TB)를 low-latency로 처리하는 것이 목적이다.
- 적은 메모리나 큰 메모리에서 STW 시간을 최대한 적게(10ms 이하로) 가져가기 위해 제작되었다.

#### Generational ZGC
- Java 21에 release 되었다.
- 기존 ZGC에 세대 개념를 도입하였다. 
	- Young/Old으로 구분하며, 기존의 Young의 세부 영역은 존재하지 않는다.
- ZGC에 비해 다음과 같은 이점이 있다.
	- 할당 정체의 위험 감소
	- 필요한 힙 오버헤드 감소
	- 가비지 컬렉션(GC)에 대한 CPU 영향 감소

##### TMI
zee gee cee, zed gee cee 등으로 발음한다,

z는 예전에 Zettabyte File System의 약자였으나 지금은 아무 의미 없다.

64bit 운영체제에서만 사용 가능하다.

# References

- [D2 - Java Garbage Collection](https://d2.naver.com/helloworld/1329)
- 책 자바 최적화 6,7 챕터
- [오라클 Java SE 11 GC 튜닝 가이드](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/toc.html)
- [가비지 컬렉션 동작 원리 & GC 종류 총정리](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EA%B0%80%EB%B9%84%EC%A7%80-%EC%BB%AC%EB%A0%89%EC%85%98GC-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%F0%9F%92%AF-%EC%B4%9D%EC%A0%95%EB%A6%AC#%EA%B0%80%EB%B9%84%EC%A7%80_%EC%BB%AC%EB%A0%89%EC%85%98_%EB%8F%99%EC%9E%91_%EA%B3%BC%EC%A0%95)
- [Baeldung - Choosing a GC Algorithm in Java](https://www.baeldung.com/java-choosing-gc-algorithm)
- [Oracle - Java Garbage Collection Basics](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html) - Perm 영역이 있는걸로 보아 Java SE 7 이하 내용
- [가비지 컬렉터(Garbage Collector)와 Mark & Sweep](https://imasoftwareengineer.tistory.com/103)