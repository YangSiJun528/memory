
### Conventional Commits

[Conventional Commits](https://www.conventionalcommits.org/ko/v1.0.0/)은 본인 사이트에서 본인을 커밋 메시지에 사용자와 기계 모두가 이해할 수 있는 의미를 부여하기 위한 스펙(A specification for adding human and machine readable meaning to commit messages)이라고 소개한다.

> Conventional Commits 스펙은 커밋 메시지에 곁들여진 가벼운 컨벤션으로 명확한 커밋 히스토리를 생성하기 위한 간단한 규칙을 제공합니다. 이렇게 만들어진 커밋 히스토리를 이용하여 더 쉽게 자동화된 도구를 만들 수 있습니다. 이 컨벤션은 커밋 메시지에 신규 기능 추가, 문제 수정, 커다란 변화가 있음을 기술함으로써 [유의적 버전(Sementic Versioning)](https://semver.org/lang/ko/)과 일맥상통한 면이 있습니다.

간단히 이해하자면 그냥 커밋 컨벤션 중 하나이다.

### Git Hooks
[Git Hooks](https://git-scm.com/book/ko/v2/Git%EB%A7%9E%EC%B6%A4-Git-Hooks)은 git(github이 아니다!)에서 어떤 이벤트가 생겼을 때 자동으로 특정 스크립트를 실행하도록 할 수 있게 하는 기능이다.

> Git도 다른 버전 관리 시스템처럼 어떤 이벤트가 생겼을 때 자동으로 특정 스크립트를 실행하도록 할 수 있다. 이 훅은 클라이언트 훅과 서버 훅으로 나눌 수 있다. 클라이언트 훅은 커밋이나 Merge 할 때 실행되고 서버 훅은 Push 할 때 서버에서 실행된다. 이 절에서는 어떤 훅이 있고 어떻게 사용하는지 배운다.


단점이 하나 있는데, 기본 훅 디렉토리는 `.git/hooks` 이고,`.git` 폴더는 무시되기 때문에 hook을 팀과 공유할 수 없다.    
[이 블로그 글](https://dev.to/florianbaba/a-githooks-example-and-how-to-share-it-with-the-team-42i0)처럼 명시적으로 경로를 설정해주어 해결하거나. 아래 설명할 Husky같은 라이브러리를 사용하여 해결할 수 있다. ~~(Husky를 추천한다. 여러 유명한 프로젝트도 사용하는 만큼 신뢰성도 보장되고 편리한데, 굳이 직접 githooks를 사용할 필요는 없어보인다.)~~ - JS 외 진영에서 사용하기 어렵다. Java나 Python같은 경우 직접 githook을 사용하기도 하는 듯?

그 외 설명은 공식적인 사이트(위에 Git Hooks에 링크된 부분)를 참고하자.

### Husky
[Husky](https://typicode.github.io/husky/)는 git hooks 기능을 쉽게 사용할 수 있도록 해준다.   
Github의 수많은 프로젝트에서 사용되고 있다.

설치를 위해서 npm같은 node 패키지 매니저가 필요하다.

JS 환경에서 사용된다. 

최소한 자바 진영에서는 Husky 같이 대중적으로 쓰이는 Git Hooks 관련 편의 기능을 제공하는 라이브러리는 없는 것 같다.

### JGit
[JGit](https://git-scm.com/book/ko/v2/%EB%B6%80%EB%A1%9D-B%3A-%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98%EC%97%90-Git-%EB%84%A3%EA%B8%B0-JGit)은 Git의 핵심 기능을 순수한 Java로 구현한 라이브러리로, Git의 기능을 Java 애플리케이션에서 쉽게 사용할 수 있도록 해준다. 이클립스 재단에서 관리하고 있다.



