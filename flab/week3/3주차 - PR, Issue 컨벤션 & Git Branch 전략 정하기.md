- - -

## 개요 

[3주차 - 커밋, 코드 컨벤션 정하고 적용하기](flab/week3/3주차%20-%20커밋,%20코드%20컨벤션%20정하고%20적용하기.md) 까지는 마무리했지만,   
프로젝트를 진행하고, 리뷰를 받기 위해선 Issue와 PR을 작성해야 한다. 

그래서 Issue와 PR, Git 브랜치 전략에 대한 컨벤션을 정하기로 했다.

우선 작업이 이루어지는 흐름이나 브랜치 관리 전략 정도만 다루고,   
PR 템플릿이나 Issue 템플릿은 이후에 프로젝트를 진행하면서 필요한 것만 적용해보기로 했다.


## PR 컨벤션 정하기

[3주차 - 커밋, 코드 컨벤션 정하고 적용하기](flab/week3/3주차%20-%20커밋,%20코드%20컨벤션%20정하고%20적용하기.md)에서 [Conventional Commits](https://www.conventionalcommits.org/ko/v1.0.0/) 명세를 기반으로 커밋 메시지를 작성하기로 하였다.

완전히 동일하지는 않을 수 있지만, 비슷한 규칙을 사용하는 [angular](https://github.com/angular/angular)의 github를 참고하였다.

#### angular PR 규칙 분석하기

PR 제목과 내용은 커밋 컨벤션과 크게 다르지 않았다.

PR이 머지되는 과정이 좀 신기했는데,    
기여자가 [PR](https://github.com/angular/angular/pull/54746)을 작성하고 관리자는 머지 대신 PR에`action: merge` labels을 붙인다.  
그 다음 관리자가 기여자의 작업을 main 브랜치로 가져와서 [commit](https://github.com/angular/angular/commit/2258ac7a32bf83dc3e33a7ff9526b501ea95e33d)하고 기존 PR은 close한다. 

아마 커밋 컨벤션을 지키기 위해서 이렇게 한 것 같은데, 결과만 보자면 `Squash & Merge`이랑 비슷하다.

### PR 컨벤션 결정하기
#### PR 제목 & 내용 
커밋 컨벤션과 동일한 Conventional Commits 명세를 사용한다. 

`Squash & Merge`를 사용해서 PR 단위로 머지하기로 했다.

`main` 브랜치 기준으로 commit log를 볼때,  
자잘한 commit이 있는 것보다 작업 단위의 commit이 있는게 가독성이 좋다고 생각했다. 

## Issue 컨벤션 정하기
개인 프로젝트인 만큼 내가 할 일을 적는 것 외에는 용도가 없다.

따라서 템플릿 외에는 크게 고려할 게 없는데, 템플릿은 나중에 정하기로 했으므로 아무 규칙도 정하지 않고 넘어갔다.

## Git Branch 전략 정하기

내가 예전에 주로 활동했던 팀에서는 [Git Flow](https://techblog.woowahan.com/2553/)를 기본 전략으로 사용하고 있었다.

다만 개인프로젝트이고 실제 사용자가 없다는 조건 때문에 더 가벼운 [github flow](https://docs.github.com/en/get-started/using-github/github-flow)를 사용하기로 했다.

#### 결론

github flow를 사용한다. 

추가적으로 필요한 규칙을 더 명시했는데, 내용은 다음과 같다.
- `모든 브랜치`에서 `main(master)` merge 시에는 `Squash & Merge`를 사용한다.
- `main`에 merge 되었을 때는 CI/CD를 통해 즉시 배포되어야한다. 
	- (해당 작업이 이루어지기 전까지는 직접 배포한다.)
- 실제 운영 서버는 하나만 존재한다. 
- `main`에 머지된 작업에서 문제가 발생한 경우 `revert`를 수행한다.
	- 경우에 따라 hotfix 작업을 수행할 수도 있다.
- 짧고 설명이 포함된 브랜치 이름을 사용한다. 
	- ex: `increase-test-timeout`, `add-code-of-conduct`
	- `{이슈 번호}-{브랜치 이름}`을 사용한다.
		- f-lab 가이드 영상에서 새로운 기능을 추가할때는 `feature/{이슈 번호}`를 사용한다. 다만 이걸 지키지 않는 프로젝트도 있어서 필수는 아닌 것 같다.

## 참고한 자료
- [Inpa Dev 👨‍💻:티스토리 - [GIT] 📈 깃 브랜치 전략 정리 - Github Flow / Git Flow](https://inpa.tistory.com/entry/GIT-%E2%9A%A1%EF%B8%8F-github-flow-git-flow-%F0%9F%93%88-%EB%B8%8C%EB%9E%9C%EC%B9%98-%EC%A0%84%EB%9E%B5) 
- [hudi - Git의 다양한 브랜치 병합 방법 (Merge, Squash & Merge, Rebase & Merge)](https://hudi.blog/git-merge-squash-rebase/) 
- [Git Branch 전략 비교 - Git Flow vs GitHub Flow](https://devocean.sk.com/blog/techBoardDetail.do?ID=165571&boardType=techBlog)
