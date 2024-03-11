### 개요

Github는 모든 서비스에서 FK를 사용하지 않는다.

(Github gh-ost의 이슈 중에 [Thoughts on Foreign Keys?](https://github.com/github/gh-ost/issues/331)를 보면 Github의 데이터베이스 엔지니어 [Shlomi Noach](https://github.com/shlomi-noach)의 답변을 확인하자.)

여러 인터넷이나 f-lab 멘토, 우테코 과제 등에서도 실무에서는 FK를 사용하지 않는다고 한다.

그 이유가 무엇일까?

### FK란?
Foreign Key(FK, 외래키)는 관계형 데이터베이스(RDB)에서 사용하는 개념으로, 다른 테이블의 PK를 참조하여 데이터 무결성을 보장하는 제약조건이다.

또한, 부모 테이블의 데이터가 없는 자식 레코드(고아 상태)를 막고, 부모 데이터가 삭제되었을 때, 자식 데이터를 삭제하거나 NULL로 설정하게 할 수 있다. (이런 기능을 제공하여 무결성을 지켜준다.)

#### 데이터 무결성이란?
데이터가 부정확하거나 모순되거나 누락되지 않고 고유하게 유지되는 상태를 말한다.   
(= 데이터의 정확성, 완전성, 일관성이 유지되는 것)

### FK의 단점
1. 성능 문제
	- 외래키 참조 무결성 제약조건을 검사하기 위해 DB에서 추가 작업이 필요하므로 성능 저하가 발생할 수 있다.
	- 또한 자동으로 FK 인덱스가 생성된다. (조인을 위해선 인덱스가 필요하므로, 이건 성능 문제라고 보긴 어려우나 인덱스가 추가되므로 성능이 감소하는 건 맞다.)
2. 유연성 감소
	- FK는 온라인 스키마 마이그레이션에서 제대로 작동하지 않는다.
		- Shlomi Noach의 [The problem with MySQL foreign key constraints in Online Schema Changes](https://code.openark.org/blog/mysql/the-problem-with-mysql-foreign-key-constraints-in-online-schema-changes) 참고
	- (무결성을 지키기 위해서) 생성, 삭제, 갱신 작업이 제한 될 수 있다.  FK 제약 조건을 지키지 않아도 되는 어플리케이션 테스트 환경에서도 번거로운 작업이 추가 된다.
	- 하지만, 외래키 제약조건 없이 데이터를 관리하면 애플리케이션 로직에서 데이터 무결성을 관리할 수 있어 유연성이 높아진다. (분산 시스템으로 확장이나 스키마 변경 등)
3. 분산 시스템 환경
	- 마이크로서비스 아키텍처나 분산 시스템 환경에서는 외래키 제약조건을 적용하기 어렵다.
	- (내 생각: 이 경우 일괄적으로 어플리케이션 단에서 무결성을 관리하는게 나을지도 모른다.)

### 결론

외래키를 사용하면 데이터의 무결성을 보장할 수 있지만, 성능과 유연성을 제한하는 요소가 되기도 한다.

따라서 실무에서는 잘 FK를 사용하지 않는다.

#### 그럼 어떤 경우에 FK를 사용해야 하는가?

1. 데이터 무결성이 중요한 경우
	- 어플리케이션 단에서 무결성을 보장하는 로직을 구현하더라도, 수십년간 만들어진 DB의 무결성 보장 로직보다 완벽하지 못한다.
	- 예: 금융, 의료
2. 규모가 작은 서비스이면서 확장을 고려하지 않아도 되는 경우
	- 데이터 규모와 트래픽이 크지 않아 FK 제약조건으로 인한 성능 저하가 크지 않은 경우
3. 단일 DB 인스턴스를 사용하는 경우
	- 분산 시스템이 아닌 단일 DB를 사용하므로 FK 관리가 용이한 경우

사실 사용 할 수 있는 상황이라면 꼭 사용하는게 좋은게 아닌가 싶다.
하지만 멘토링이나 여러 자료를 보면, 성능 뿐만 아니라 고려해야 할 문제가 많아 잘 사용하지 않는 것 같다.

# References
- [Thoughts on Foreign Keys?](https://github.com/github/gh-ost/issues/331)
- [The problem with MySQL foreign key constraints in Online Schema Changes](https://code.openark.org/blog/mysql/the-problem-with-mysql-foreign-key-constraints-in-online-schema-changes)