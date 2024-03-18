## 개요

실무에서 여러 이유로 데이터를 Hard Delete 할 수는 없다. 성능적인 이유도 있고, 법적인 이유, 비즈니스적인 이유로 데이터를 일정 기간동안 보관해야하는 경우도 있다.

`delete_at` or `flag` 값을 사용해서 삭제된 상태를 표시하는게 일반적인 방법이라고 알고 있다.    
구현이 쉽고, 쉽게 복구할 수 있기 때문이다.

`delete_at` or `flag` 값을 사용하는 Soft Delete를 사용하지 말아야 한다는 글을 보게 되었고,   
관련된 내용을 찾아서 함께 정리하였다. 

## 이유
- 복잡성을 높인다.
	- SQL 쿼리/인덱스에 삭제된 레코드를 매번 필터링해야 한다.
- 데이터 무결성
	- 데이터를 유효한 상태로 유지하려는 데이터베이스의 이점이 손실된다.
	- 삭제되었지만, Unique한 데이터를 포함하는 경우, 해당 값의 새로운 데이터를 추가할 수 없다.
- 보안 위험
	- 논리적으로 삭제되었으나 여전히 같은 테이블에서 읽고 수정할 수 있다.

## 대안책
- history or audit 테이블 사용하기
	- ex: 데이터를 직렬화하여 audit 로그에 작성, 삭제된 데이터를 저장하는 테이블 사용
# References
- [The Day Soft Deletes Caused Chaos](https://blog.bemi.io/soft-deleting-chaos/)
- [Why soft deletes are evil and what to do instead](https://jameshalsall.co.uk/posts/why-soft-deletes-are-evil-and-what-to-do-instead)
- [Why should not we use soft deletes in the models/tables used for authenticating into the application](https://debiprasad.medium.com/why-we-should-not-use-soft-deletes-in-the-model-table-which-used-for-authenticating-into-the-c350e5f1c2c9)
- [stackoverflow - Physical vs. logical (hard vs. soft) delete of database record? [closed]](https://stackoverflow.com/questions/378331/physical-vs-logical-hard-vs-soft-delete-of-database-record)
