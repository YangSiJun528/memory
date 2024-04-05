
작성일 기준(2024/04/02)Github Actions에서 제공해주는 workflow인 `Java with Gradle`를 분석하였다.

```yml
# This workflow uses actions that are not certified by GitHub.
# They are provided by a third-party and are governed by
# separate terms of service, privacy policy, and support
# documentation.
# This workflow will build a Java project with Gradle and cache/restore any dependencies to improve the workflow execution time
# For more information see: https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-java-with-gradle

name: Java CI with Gradle

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:

    runs-on: ubuntu-latest
    permissions:
      contents: read

    steps:
    - uses: actions/checkout@v4
    - name: Set up JDK 17
      uses: actions/setup-java@v4
      with:
        java-version: '17'
        distribution: 'temurin'

    # Configure Gradle for optimal use in GiHub Actions, including caching of downloaded dependencies.
    # See: https://github.com/gradle/actions/blob/main/setup-gradle/README.md
    - name: Setup Gradle
      uses: gradle/actions/setup-gradle@417ae3ccd767c252f5661f1ace9f835f9654f2b5 # v3.1.0

    - name: Build with Gradle Wrapper
      run: ./gradlew build

    # NOTE: The Gradle Wrapper is the default and recommended way to run Gradle (https://docs.gradle.org/current/userguide/gradle_wrapper.html).
    # If your project does not have the Gradle Wrapper configured, you can use the following configuration to run Gradle with a specified version.
    #
    # - name: Setup Gradle
    #   uses: gradle/actions/setup-gradle@417ae3ccd767c252f5661f1ace9f835f9654f2b5 # v3.1.0
    #   with:
    #     gradle-version: '8.5'
    #
    # - name: Build with Gradle 8.5
    #   run: gradle build

  dependency-submission:

    runs-on: ubuntu-latest
    permissions:
      contents: write

    steps:
    - uses: actions/checkout@v4
    - name: Set up JDK 17
      uses: actions/setup-java@v4
      with:
        java-version: '17'
        distribution: 'temurin'

    # Generates and submits a dependency graph, enabling Dependabot Alerts for all project dependencies.
    # See: https://github.com/gradle/actions/blob/main/dependency-submission/README.md
    - name: Generate and submit dependency graph
      uses: gradle/actions/dependency-submission@417ae3ccd767c252f5661f1ace9f835f9654f2b5 # v3.1.0
```


- on: 이벤트
- jobs: build와 dependency-submission로 나뉜다.
	- build
		- runs-on: 실행 환경(Runner)을 정의한다.
		- permissions: Job에 대한 권한을 설정한다.
		- steps
			- actions/checkout@v4: 소스코드를 가져온다.
			- actions/setup-java@v4: with까지 추가로 작성해서 JDK 17을 설정한다.
			- gradle/actions/setup-gradle@(생략): gradle을 설정한다.
			- run: ./gradlew build: 해당 명령어를 실행한다. 프로젝트를 빌드한다.
	- dependency-submission
		- (이전과 같은 부분은 생략)
		- gradle/actions/dependency-submission@(생략): 의존성에 대한 그래프를 깃헙에 제출하여 Dependabot Alerts을 활성화 한다.

중간 주석은 Gradle Wrapper가 없는 경우 Gradle을 설정하는 부분

### Dependabot이란?

[Dependabot](https://github.com/dependabot)은 소스코드의 종속성들을 확인해 보안 이슈가 발생하는 것 알려주고, 자동으로 버전을 올려주는 기능도 가지고 있다.

Repo의 Security Tab에서도 설정 가능한데, 굳이 CI에서 추가할 필요까지 있나 싶지만 CI 때마다 명시적으로 수행하므로 있어서 나쁠 것 없는 것 같다.


# References
- [Java with Gradle By GitHub Actions](https://github.com/f-lab-edu/celog/new/main?filename=.github%2Fworkflows%2Fgradle.yml&workflow_template=ci%2Fgradle)