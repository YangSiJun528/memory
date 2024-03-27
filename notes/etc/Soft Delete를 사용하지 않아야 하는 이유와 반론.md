## 개요

실무에서 여러 이유로 데이터를 Hard Delete 할 수는 없다. 성능적인 이유도 있고, 법적인 이유, 비즈니스적인 이유로 데이터를 일정 기간동안 보관해야하는 경우도 있다.

`delete_at` or `flag` 값을 사용해서 삭제된 상태를 표시하는 Soft Delete 방식이 일반적인 방법이라고 알고 있다.    
구현이 쉽고, 쉽게 복구할 수 있기 때문이다.

하지만 이러한 Soft Delete를 사용하지 말아야 한다는 글을 보게 되었고,   
관련된 내용을 찾아서 함께 정리하였다. 

## 이유
- 복잡성을 높인다.
	- SQL 쿼리/인덱스에 삭제된 레코드를 매번 필터링해야 한다.
- 데이터 무결성
	- 데이터를 유효한 상태로 유지하려는 데이터베이스의 이점이 손실된다.
	- (논리적으로) 삭제되었지만, Unique한 데이터를 포함하는 경우, 해당 값의 새로운 데이터를 추가할 수 없다.
- 보안 위험
	- 논리적으로 삭제되었으나 여전히 같은 테이블에서 읽고 수정할 수 있다. (이는 고객이 데이터 삭제를 요청하면 익명화해서 보관하거나 삭제해야 하는 법적인 규칙을 상대적으로 위반하기 쉽다.)
 	- 복구가 쉬운 만큼, [잘못 사용될](https://blog.bemi.io/soft-deleting-chaos/) 가능성도 높다.	

## 대안책
- history or audit 테이블 사용하기
	- ex: 데이터를 직렬화하여 audit 로그에 작성, 삭제된 데이터를 저장하는 테이블(미러 테이블) 사용

## 실무에선?

flab 멘토님에게 물어본 결과, 실제로는 Soft Delete를 많이 사용한다고 한다.   
삭제된 데이터의 비율은 적고(5% 이하, 인덱스 조건을 사용할 정도는 아니라 이렇게 말씀하신 듯?) 분리해서 얻는 복잡도와 노력 대비 output이 높지 않기 때문이라고 한다.

위 사용하지 말라는 내용도, 잘못 사용할 가능성이 있다는 것이지, 주의하면 문제를 예방할 수 있기도 하고...

# References
- [The Day Soft Deletes Caused Chaos](https://blog.bemi.io/soft-deleting-chaos/)
- [Why soft deletes are evil and what to do instead](https://jameshalsall.co.uk/posts/why-soft-deletes-are-evil-and-what-to-do-instead)
- [Why should not we use soft deletes in the models/tables used for authenticating into the application](https://debiprasad.medium.com/why-we-should-not-use-soft-deletes-in-the-model-table-which-used-for-authenticating-into-the-c350e5f1c2c9)
- [stackoverflow - Physical vs. logical (hard vs. soft) delete of database record? [closed]](https://stackoverflow.com/questions/378331/physical-vs-logical-hard-vs-soft-delete-of-database-record)
