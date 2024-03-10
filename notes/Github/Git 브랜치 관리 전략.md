- - -
서비스 운영 시 운영 환경과 기능 개발을 분리하기 위해, 여러 브랜치 관리 전략을 사용할 수 있다.

대표적으로 Git Flow와 GitHub Flow가 있다.

(GitHub Flow는 trunk-based라고도 부르는 것 같다. 온전히 같은 의미인지는 모르겠지만.)

- ### Git Flow
	- 여러 브랜치를 사용한다.
		1. `master`: 제품 출시 버전을 관리하는 메인 브랜치 
		2. `develop`: 다음 출시 버전을 위해 개발하는 브랜치
		3. `feature`: 새로운 기능을 개발하는 브랜치
		4. `release`: 다음 출시 버전을 준비하는 브랜치
		5. `hotfix`: 출시된 제품의 버그를 고치기 위한 브랜치
	- 장/단점
		- 단점: 더 많은 제어와 복잡성을 가진다. -> 유연성이 (상대적으로) 떨어진다.
		- 장점: 배포 안정성과 버전 관리 및 롤백 등 체계적인 운영이 가능하다.
- ### GitHub Flow
	- `master`와 `feature` 브랜치만 사용한다.
	- `master`를 바로 서비스로 운영한다.
	- 장/단점
		- 장점: Agile한 방식, 여러 절차 없이 바로 릴리즈(허술하진 않음, 그만큼 더 세세하게 검사)
		- 단점: (상대적으로) 여러 버전을 동시에 운영하거나 하는 체계적인 운영이 어려울 수 있다.

# References & 참고하면 좋을 글

- [[GIT] 📈 깃 브랜치 전략 정리 - Github Flow / Git Flow](https://arc.net/l/quote/eyndaqzl)
- [Git Branch 전략 비교 - Git Flow vs GitHub Flow](https://devocean.sk.com/blog/techBoardDetail.do?ID=165571&boardType=techBlog)
- [우아한 기술 블로그 - 우린 Git-flow를 사용하고 있어요](https://techblog.woowahan.com/2553/)
- [Git Flow에서 트렁크 기반 개발으로 나아가기 - 맘시터 기술블로그](https://tech.mfort.co.kr/blog/2022-08-05-trunk-based-development/)
