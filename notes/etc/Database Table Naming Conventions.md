
컨벤션이 왜 필요한가? 간단하게 설명하자면 사용자마다 다르게 사용할 수 있는 부분의 규칙을 정하고 지키면, 프로젝트를 운영하고 유지보수하는데 도움이 된다.

인터넷에서 나오는 컨벤션의 주장과 그 이유을 정리해 보았다. (일부 주장은 정리하지 않았지만, 대부분 그 반대의 주장도 있을 것이다.)

개인적으로는 어떤 컨벤션이 옮다고 볼 수 없다고 생각한다. 팀 내에서 사용하는 규칙이 있다면 그걸 지켜야한다. 그래야 컨벤션의 의의에 맞기 떄문


- Table 네이밍
	- 단수형을 사용하라
		- 시스템 전체, 어플리케이션 전체(DB, Server 등)에서 일관성을 보장한다.
		- 테이블의 개념에 더 알맞다.
		- 만들기 쉽고 이해하기 쉽다. 영어가 아닌 개발자가 많다.
		- 1:M 연관관계를 잘 표현한다.
			- `Orders`, `OrderDetails`에서 사용하는 order는 같은 테이블이지만`Order`와 `Orders`로 작성된다.
	- 복수형을 사용하라
		- 복수 테이블 이름은 예약된 키워드와 충돌할 가능성이 적다.
- ID 네이밍
	- `id`를 사용하지 말것, `{table_name}_id`를 사용하자.
		- 조인 시 어떤 테이블과 조인을 하는지 알기 어렵다.
	- 테이블의 기본키와 왜래키의 이름을 통일해야 한다.
		- 다른 의미를 포함하는 경우 예외적으로 다른 이름을 가질 수 있다. (ex: `follow` table에서는 `user_id`를 `follow_id`, `follwing_id`로 표현한다.)
- 컬럼 네이밍
	- 약어를 사용하지 않는다.
		- 예외적으로 약어가 더 자주 사용되는 경우 약어를 사용할 수 있다. (ex: `i18n`)
	- 정확한 의미를 사용한다.
		- `temperature` 보다는 `fahrenheit`가 더 명확하다.
		- `util`, `common` 같은 의미가 모호한 단어도 사용하지 않는다.
	- 단어를 구분하기 위해 언더스코어(`_`)를 사용하라
		- SQL이 대소문자를 구분하지 않기 떄문이다.
		- 더 읽기 쉽고 명확하다.
		- CamelCase를 사용하자는 의견도 있는데, 둘 다 개인적인 취향 정도로 보는게 좋을 듯. 결국 시간낭비
			- `> That being said, this is a personal preference and not a strong recommendation. Many people love their CamelCase and this quickly becomes a "tabs versus spaces" debate (e.g., a waste of time).`


# References
- [Dev - Database Naming Standards](https://dev.to/ovid/database-naming-standards-2061)
- [bealdung - Database, Table, and Column Naming Conventions](https://www.baeldung.com/cs/database-table-column-naming-conventions)
- [Singular vs Plural Table Names: Should Database Table Names Be Singular or Plural?](https://www.databasestar.com/database-table-naming-conventions/)
- [Database Naming Convention](https://github.com/RootSoft/Database-Naming-Convention#database-naming-convention)