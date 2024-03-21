Gradle에서 Plugin으로 지원한다.

`finalizedBy`와 `dependsOn`을 사용해서 test task 이후에 실행되도록 권장하는데, 굳이 할 필요는 없다.   

해당 작업을 묶어서 새 Task로 만들어도 되고, test 만 실행해도 되게 설정해도 되고... 마음대로 해도 상관 없다.

대신 `test` -> `jacocoTestReport` -> `jacocoTestCoverageVerification` 순서로 실행되는 것만 지키면 된다.

- `jacocoTestReport`: 리포트 생성
- `jacocoTestCoverageVerification`: 테스트 커버리지 검증 수행, 설정한 기준치 이하면 task가 실패한다.

#### Github Action 통합

Github Action용 [Actions 템플릿](https://github.com/Madrapps/jacoco-report)이 있다. 이거 말고도 더 있는데, 해당 Action이 가장 Star가 많다.

PR에서 `jacocoTestReport`를 실행하고 나오는 xml 파일을 읽어 설정한 커버리지 이하면 Action이 실패한다.

# References
- [Gradle Docs - The JaCoCo Plugin](https://docs.gradle.org/current/userguide/jacoco_plugin.html)
- [우아한기술블로그 - Gradle 프로젝트에 JaCoCo 설정하기](https://techblog.woowahan.com/2661/)