

## 파일 시스템의 Block과 

## 순차 I/O, 랜덤 I/O

- B+Tree 인덱스가 느린 이유

- https://hudi.blog/storage-and-random-sequantial-io/
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
	- B-Tree와 달리 leaf 노드에만 값을 가지고, 나머지(internal)은 범위만 저장함.
- 항상 모든 leaf Node의 level이 같다.

#### 인덱스의 자료구조인 이유

(DB,OS의 파일 시스템에 개념에 대한 이해가 필요함)

1. 데이터는 block 단위로 secondary storage에 저장된다. 
	1. secondary storage에 접근하는 횟수가 많을수록 느리다.
2. RDBMS의 row와 **index node** 역시 block 단위로 저장된다.
	1. 데이터가 너무 많아서 메모리에 전부 올릴 수 없음, 필요한거만 그때그때 읽어서 써야 한다.
3. tree 형태의 자료구조는 메모리 상에서 연속적이지 않다. 따라서 random I/O가 발생한다.
	1. random I/O는 sequential I/O보다 성능이 더 느리다.
4. 따라서 I/O가 적게 발생하는 자료구조가 좋다.
5. B-Tree의 경우 N차 B-Tree를 통해 한번에 넒은 범위의 데이터를 검사할 수 있다.
	1. 쉬운코딩 예시로 101차에 level이 3인 경우 경우 최선의 경우 약 1억의 데이터를 읽을 수 있다.
	2. n차일 때, 최선의 경우 m = n, 최악의 경우는 m = (n/2)+1고, 다음 식을 통해 구하기 가능
		1. `(m^level + m^level-1 ... m^1)`
6. 이는 B-Tree가 다른 방식보다 낮은 level을 가진다는 의미고, 그만큼 I/O 횟수도 줄어든다.

- 추가 - (약간 내 뇌피셜도 있음)
	- B-Tree(밒 B+Tree)는 읽기에 최적화되어 있다. 쓰기 작업이 느리다. (특히 level이 변해서 재구성 하는 경우 더 느려진다.)
		- RDBMS는 일관성이나 무결성을 중요시하기 때문에 이 작업이 동기적으로 이루어 져야 한다.
		- 영향이 가는 인덱스 개수가 많이질수록 수정 작업은 더 느려진다.
		- 애초에 RDBMS자체가 테이블(스키마) 간의 관계를 통해 데이터 중복을 줄이는게 특징이고, 읽기 작업의 비중이 높으므로 읽기 작업의 성능을 중점으로 보기 때문에 이런 단점을 가지면서도 사용하는 것.
			- (NoSQL은 수정 비율이 더 높아서 B+Tree 대신 LSM Tree를 쓴다.)

- https://engineerinsight.tistory.com/336
- https://www.youtube.com/watch?v=bqkcoSm_rCs&list=PLcXyemr8ZeoR82N8uZuG9xVrFIfdnLd72&index=25
- https://en.wikipedia.org/wiki/B-tree


## LSM Tree - B Tree vs LSM Tree

- 실제로 트리 구조를 띄기 때문에 LSM(log-structured merge) Tree라고 부른다.


- https://www.youtube.com/watch?v=I6jB0nM9SKU
- https://www.youtube.com/watch?v=i_vmkaR1x-I
- https://en.wikipedia.org/wiki/Log-structured_merge-tree

