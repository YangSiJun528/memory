

## 파일 시스템의 Block과 DB의 논리적 I/O 최소 단위(Page)

OS의 파일 시스템에는 Block이라고 읽고 쓰는 논리적인 최소 단위가 있다. (논리적임, 주로 4kb, 8kb 등 2배수)

이 크기만큼 데이터를 읽고 쓰는데, 0,1 하나 수정하는 것도 Block을 읽고 다시 써야 함.

RDBMS나 NoSQL 등의 DB는 당연하게도 내부적으로는 이런 block을 사용하나. (당연히 OS단 기능이니까.)

DB의 논리적인 최소 단위는 DB마다 다르다.

대부분의 RDBMS에서는 Page라고 부르고,  (Oracle만 Data Block이라고 부름)

경우에 따라 OS의 Block과 DB의 Page를 비교했을 때, 더 작거나, 같거나, 클 수도 있다.

각각의 장단점이 있는 편.

NoSQL의 경우 워낙 다양하기 때문에 공식문서를 찾아보는게 나을 것 같다. (다른 DB도 마찬가지)

- f-lab 멘토님 피셜
- SQLP 책 참고 - 대충 읽은거라 정확하지는 않음.

## Sequential(순차) I/O, Random(랜덤) I/O

I/O에는 물리적인 차이에 따라서 2가지 종류가 있음.

DB나 자료구조를 다룰 때, 효율성 면에서 자주 이야기가 나오기 때문에 알아두면 좋음

- HDD와 SDD에 따라 성능 차이가 다름
	- HDD는 물리적인 읽기(디스크 암 회전)가 필요해서 Random I/O가 느림
	- SDD는 전기적인 방식으로 동작해서 Random I/O와 Sequential I/O와 큰 차이가 없음. 그래도 랜덤이 더 느림.

- Sequential I/O
	- 주소가 물리적으로 순차적으로 위치한 여러개의 Block를 읽는 경우
	- 읽기/쓰기 작업이 연속적으로 진행되므로 빠른 I/O가 가능
- Random I/O
	- 물리적으로 떨어져있는 주소를 한 Block 씩 읽는 경우, 가깝거나 같은 주소라도 한 Block씩 읽는 것도 포함함. (RDB의 경우 Block을 Page로 볼 수도 있음)
	- Block이 여러 곳에 분산되어 I/O를 수행하기 때문에 느리다.


- https://hudi.blog/storage-and-random-sequantial-io/
- SQLP 가이드 책

## B+Tree 인덱스가 느린 이유

- 모든 연산이 Random I/O이기 때문에
	- 디스크에 있는 root node를 읽고, 다른 internal node의 디스크 주소를 찾아서 Random I/O함.
	- leaf node에 도달해도, 인덱스 외에 추가 조건이 있으면 조건에 맞는지 매번 Random I/O 해야 함.
	- 물론 이 사이에 RDB는 메모리 버퍼 풀이 있어서 메모리에서 캐싱될 수도 있지만, 그래도 Random I/O인건 변하지 않음.

- 반대로 full sacn은 Sequential I/O라서 더 효율적일 때도 있음, 특히 파티셔닝을 통해서 검색 범위가 줄었을 때

- SQLP 가이드 책

## SQL,DB 튜닝이란?

- 읽는 데이터 수가 적은가?
	- 인덱스 추가,수정,재구성(기존 쿼리들 분석해서 싹 다 갈아엎기)
	- 실행계획 분석하고 효율적이게 바꾸기
		- 조인 전략, 스캔 순서 등
- 읽는 데이터 수가 많은가?
	- 파티셔닝
- 추가하는 데이터 수가 많은가?
	- (흑마법 아님 이거?) 제약조건 지우고 나중에 걸기

## DB 아키텍처

DB마다 달라서... 여러가지 공식문서나 자료 가져와서 비교하는 식으로??

걍 이건 공식문서 확인하고 그때그떄 링크 남기면 될 듯?

## 트랜잭션과 Lock, ACID

## 낙관적, 비관적 Lock

## DBMS란? DBMS와 DB

DBMS (Database Management System)

DB (Database)


## B Tree와 B Tree가 RDB의 인덱스의 자료구조인 이유

#### B-Tree

- 이진 탐색 트리가 아닌 N개의 자식 노드를 가지는 탐색 트리
	- 오름차순으로 정렬된 root, 중간 Node의 key 목록이 하위 노드들의 범위를 결정한다.
	- 데이터 자체는 leaf노드에만 가지고 있다.
- 정확히 따지면 B+Tree가 RDB의 인덱스, B Tree(B-Tree라고도 함)는 아님.
	- B-Tree와 달리 B+Tree는 leaf 노드에만 값을 가지고, 나머지(internal)은 범위만 저장함.
- 항상 모든 leaf Node의 level이 같다.

#### 인덱스의 자료구조인 이유

(DB,OS의 파일 시스템에 개념에 대한 이해가 필요함)

1. 데이터는 block 단위로 secondary storage에 저장된다.
	1. block/page라고 부르며, RDBMS의 I/O의 최소 단위이다. 일반적으로 4kb임.
	2. (이게 RDB가 block/page 방식이라 RDB의 B Tree가 page를 사용하는지, 아니면 B Tree가 원래 page를 사용하는건지 모르겠음. )	
	3. secondary storage에 접근하는 횟수가 많을수록 느리다.
3. RDBMS의 row와 **index node** 도 block과 비슷한 단위를(DB마다 다름) 사용한다.
	1. 데이터가 너무 많아서 메모리에 전부 올릴 수 없음, 필요한거만 그때그때 읽어서 써야 한다.
4. tree 형태의 자료구조는 메모리 상에서 연속적이지 않다. 따라서 random I/O가 발생한다.
	1. random I/O는 sequential I/O보다 성능이 더 느리다.
5. 따라서 I/O가 적게 발생하는 자료구조가 좋다.
6. B-Tree의 경우 N차 B-Tree를 통해 한번에 넒은 범위의 데이터를 검사할 수 있다.
	1. 쉬운코딩 예시로 101차에 level이 3인 경우 경우 최선의 경우 약 1억의 데이터를 읽을 수 있다.
	2. n차일 때, 최선의 경우 m = n, 최악의 경우는 m = (n/2)+1고, 다음 식을 통해 구하기 가능
		1. `(m^level + m^level-1 ... m^1)`
7. 이는 B-Tree가 다른 방식보다 낮은 level을 가진다는 의미고, 그만큼 I/O 횟수도 줄어든다.

- 추가 - (약간 내 뇌피셜도 있음)
	- B-Tree(밒 B+Tree)는 읽기에 최적화되어 있다. 쓰기 작업이 느리다. (특히 level이 변해서 재구성 하는 경우 더 느려진다.)
		- RDBMS는 일관성이나 무결성을 중요시하기 때문에 이 작업이 동기적으로 이루어 져야 한다.
		- 영향이 가는 인덱스 개수가 많이질수록 수정 작업은 더 느려진다.
		- 애초에 RDBMS자체가 테이블(스키마) 간의 관계를 통해 데이터 중복을 줄이는게 특징이고, 읽기 작업의 비중이 높으므로 읽기 작업의 성능을 중점으로 보기 때문에 이런 단점을 가지면서도 사용하는 것.
			- (NoSQL은 수정 비율이 더 높아서 B+Tree 대신 LSM Tree를 쓴다.)

- https://engineerinsight.tistory.com/336
- https://www.youtube.com/watch?v=bqkcoSm_rCs&list=PLcXyemr8ZeoR82N8uZuG9xVrFIfdnLd72&index=25
- https://en.wikipedia.org/wiki/B-tree
- https://www.youtube.com/watch?v=yLe7_3cGSeU


## LSM Tree - B Tree vs LSM Tree

- 트리 구조를 띄기 때문에 LSM(log-structured merge) Tree라고 부른다.

- 여러 NoSQL에서 많이 사용된다.
- 쓰기 작업에 최적화되어 있다.
	- 모든 작업이 쓰기로 이루어진다.
		- 삭제도 실제로 삭제하지 않고, "삭제된 상태"를 저장한다.
		- 변경도 역시 쓰기를 수행한다.
		- Memtable -> Level N(0에서 부터)의 최근 SSTable 순으로 읽는 식.
			- 각 영역에서 가장 먼저 찾은 값이 최신 상태임.
			- 각 SSTable는 정렬된 상태를 가지므로 빠르게 검색 가능.
- 구조 설명
	- Memtable: 메모리에서 관리되며, 쓰기 작업 시 데이터를 먼저 이곳에 저장
		- Balanced Binary Tree 형태이며 정렬된 상태를 가진다.
	- Level 0: Memtable이 가득 차면 디스크로 플러시되어 SSTable로 변환한다. 이 초기 SSTable은 Level 0에 저장된다. 
		- SSTable는 로그 형태로 저장되어 있다.
		- SSTable를 작성할 때는 순차 I/O가 발생하는데, 이는 Memtable가 정렬된 상태를 가지기 때문이다.
	- Level 1, Level 2, ...: SSTable이 일정 기준을 초과하면 컴팩션(compaction) 과정을 통해 하위 레벨로 이동. 이 과정에서 중복된 데이터나 삭제된 데이터(tombstone)를 제거하여 더 큰 SSTable로 병합한다.
		- SSTable은 합쳐지기 쉬움 (merge sort와 비슷한 방식)
	- 경우에 따라 효율적인 처리를 위해 블룸 필터(무조건 없음 or 있을 수도 있음. 상태를 반환함)를 사용함.
		- 특정 SSTable에 검색 조건(key)가 있는지 확인

- B+Tree와 비교
	- B+Tree
		- 모든 I/O가 랜덤 I/O (디스크 접근 기준)
		- 여러 Page 업데이트가 발생(level 변경이나 트리 구조를 유지하기 위한 변경) 가능
		- 읽기 작업에 최적화 되어 있음.
		- 그러나 위 특징으로 데이터 쓰기 속도에 제한됨.
	- LSM Tree
		- 디스크에 데이터 저장(삭제/수정도 저장으로 봐야 함) 시 순차 I/O가 발생한다. 즉, 모든 디스크 I/O가 순차 I/O이다.
		- 트리 구조를 지키거나 할 필요 없기 때문에 쓰기 작업에 시간이 적게 걸림
		- SSTable Compaction은 비동기적으로 수행 가능 (백그라운드에서 수행, B+Tree는 구조를 유지해야 하므로 이 과정에서 쓰기 작업 속도가 느려짐.)
		- 정렬된 상태를 가지므로 Range 검색 가능
		- 쓰기 작업에 최적화되어 있음.
		- 그러나 위 특징으로 B Tree에 비해 읽기 속도가 상대적으로 느리고, 디스크를 더 많이 차지함.
			- level을 순차적으로 내려가면서 읽어야 함.
			- Memtable나 SSTable 정렬 된 상태를 가지므로 엄청 느린건 아님.

- https://www.youtube.com/watch?v=I6jB0nM9SKU
- https://www.youtube.com/watch?v=i_vmkaR1x-I
- https://en.wikipedia.org/wiki/Log-structured_merge-tree

### WAL (Write-Ahead Logging)

(본격적인 작성은 나중에)

대충 설명: 트랜잭션마다 DB(디스크)에 매번 쓰고, 트랜잭션이 끝날 때까지 완료를 기다리면 오버헤드가 심함. (예: 테이블에 데이터 추가 + 여러 인덱스에 추가 -> 특히 랜덤 I/O가 많음)

그래도 트랜잭션이 끝나면 ACID(특히 Durability)를 보장하기 위해 디스크에 데이터를 써야 하므로, 로그 형식으로 트랜잭션 수행 결과를 저장(영속성 보장)해놓고 나중에 비동기적으로 반영함.

- (근데 이러면 궁금한게, WAL에만 저장되고 DB에 반영되기 전이면 반영된 결과를 못 읽는거 아니야?)
	- Dirty Page를 가져오기 전에 WAL을 Disk에 반영한다거나, Disk 읽고 WAL을 읽어서 최신 데이터를 가져온다거나... 방법은 많을거 같음. 뇌피셜이니까 나중에 찾아보기
	

## Redis

> 레디스(Redis)는 Remote Dictionary Server의 약자로서, "키-값" 구조의 비정형 데이터를 저장하고 관리하기 위한 오픈 소스 기반의 비관계형 데이터베이스 관리 시스템(DBMS)이다. 2009년 살바토르 산필리포(Salvatore Sanfilippo)가 처음 개발했다. 2015년부터 Redis Labs가 지원하고 있다. 모든 데이터를 메모리로 불러와서 처리하는 메모리 기반 DBMS이다. BSD 라이선스를 따른다. DB-Engines.com의 월간 랭킹에 따르면, 레디스는 가장 인기 있는 키-값 저장소이다.
>
> -위키피디아-

(상장하고, 자유오픈소스가 아니라 다른 기술로 뺏길 수도 있지만, 역할에 대한 이해는 필요함. 역할은 잘 대체되지 않기 때문에.)

- Redis의 초기 목적: 빠른 속도의 Key-Value DB
- Redis가 빠른 이유 (초당 수백만 건 처리 가능, 평균 작업 속도 < 1ms)
	- InMemory DB
		- 자연스럽게 지연시간이 짦고, 구현 난이도도 낮음
	- 싱글 스레드로, lock 알고리즘이 필요 없음. (이벤트 루프)
		- epoll과 같은 I/O 멀티플렉싱 기능을 사용.
		- 단, 여러 코어를 사용하지 못한다는 단점
			- (해결하기 위해서 코어에 여러 redis 인스턴스를 실행시키고, Redis Cluster를 사용하기도 하는듯?)
		- 정확히는 처리를 수행하는 스레드가 하나.
			- OS/네트워크 I/O를 처리하는 스레드 풀이 필요함.
			- 설정하면 처리도 여러 스레드에서 할 수 있다고는 함.
	- 다양한 데이터 타입 효율적인 지원
- Redis가 뜬 이유
	- Replication(복제, Master-Slave 관계, 읽기 속도 향상), Cluster(수평확장(샤딩), 안전성?)을 적용하기 쉽고 안전하기 때문에.
		- (이 부분 잘 이해 안감)
	- 다양한 Data Type 지원
	- 기업 친화적인 기능 확장
		- 지속성 옵션, Pub/Sub, 여러 데이터 지원 등 
		- 단순 캐시 DB 뿐만 아니라 다양한 기능 제공
			- 반면에, memcached는 단순하게 캐싱 기능에 특화됨
- Redis 사용 용도
	- 캐싱
	- [RAG](https://aws.amazon.com/ko/what-is/retrieval-augmented-generation/)를 사용해서 Gen AI 개발? 튜닝?
		- 거의 동일한 질문 저장 / 프롬프트 저장 등
	- 개인 맞춤형 서비스
		- 사용자의 프로파일 정보 관련 서비스를 Redis에 캐싱
	- 실시간 데이터 스트리밍
		- (근데 영속성 보장 안되는 특징을 가지지 않나?, 뭐 사용 예시 정리하는거니까)


- 북마크 겸 참고
	- [위키피디아 - 레디스](https://ko.wikipedia.org/wiki/%EB%A0%88%EB%94%94%EC%8A%A4)
	- [일반 NoSQL DB와 Redis가 다른 점은? [세미남417@토크아이티, 김봉환 Solution Architect]](https://youtu.be/PR2avqOerWw?si=zcSeeV01-1Rz4rJ8)
	- [System Design: Why is single-threaded Redis so fast?](https://www.youtube.com/watch?v=5TRFpFBccQM)
	- [AI시대, 인메모리 NoSQL DB Redis가 주목받는 이유: RAG, 개인화, 자율운영](https://www.youtube.com/watch?v=VtWa9t_9CKA&t=66s)
	- [[미니컨] 대용량 서비스에 자주 쓰이는 레디스에 대해 알아보자](https://www.youtube.com/watch?v=M2u3pOT7I8E) << 추천
	- [[NHN FORWARD 2021] Redis 야무지게 사용하기](https://youtu.be/92NizoBL4uA?si=jKM9JmXXCTvk8p18) << 추천
