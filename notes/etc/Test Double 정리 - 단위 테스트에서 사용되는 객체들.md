### Context
테스트에서 사용되는 객체를 정의하는 용어가 잘 이해가지 않아 찾아보았다.

### 어디서 나온 용어인가?
_xUnit 테스트 패턴_ 의 저자인 Meszaros가 해당 책에서 단위 테스트에서 사용되는 가상 객체를 **Test Double**이라고 정의한다.  (영화 촬영 시 배우의 역할을 대신하는 Stunt Double에서 유래되었다.)

마틴 파울러의 [Mocks Aren't Stubs](https://martinfowler.com/articles/mocksArentStubs.html)나 책이나 블로그에서 사용하는 용어를 보았을 때, 일반적으로 사용 되는 것 같다.

### 설명
- **Dummy (더미)**: 전달되지만 실제로는 사용되지 않는다.
	- 일반적으로 매개변수 목록을 채우는 데 사용한다.
	- 테스트에 필요한 객체지만, 기능은 필요하지 않은 경우 사용한다.
- **Fake (페이크)**: 실제로 작동하는 구현이 있지만 실제 production 환경에서는 적합하지 않은 일종의 편법(shortcut)을 사용한다.
	- ex: 메모리 데이터베이스
- **Stubs (스텁)**: 테스트에 사용되는 호출에 대해 미리 준비된 답변을 제공한다. 
	- 일반적으로 테스트를 위해 프로그래밍된 것 외에는 작동하지 않는다.
- **Spies (스파이)**: 실체 객체를 부분적으로 Stubbing 하면서 약간의 정보를 기록한다.
	- ex: 전송된 메시지 수를 기록하는 이메일 서비스
	- 테스트 시, 기록한 정보를 기반으로 협력 객체의 상태를 검증한다. (= 상태 검증을 한다.)
- **Mocks (목)**: 함수가 호출될 때 인수에 제공되는 값을 기록한다.
	- 테스트 대상 코드 가 의존성을 통해 제공되는 함수를 호출하는지 검증하기 위해 목을 사용할 수 있다.
	- Test Double 중 유일하게 행위 검증을 한다. (협력 객체를 호출하는지 확인하는 것을 말함)

> 마이크로소프트 문서에서는 여러 테스트 더블이 이론상 구별되는 것으로 보이지만, 실제로는 그 차이가 흐려진다고 말한다. **즉, 테스트 더블의 경계는 명확한 것이 아니므로 스펙트럼으로 바라봐야 바람직하다.**
> [출처](https://hudi.blog/test-double/)

![unit-testing-exploring-the-continuum-of-test-doubles](https://learn.microsoft.com/en-us/archive/msdn-magazine/2007/september/images/cc163358.fig02.gif)

### Reference
- [Mocks Aren't Stubs](https://martinfowler.com/articles/mocksArentStubs.html)
- [목은 스텁이 아닙니다 (마틴 파울러 - Mocks Aren't Stubs 번역)](https://jonghoonpark.com/2023/10/26/mocks-arent-stubs)
- [Test Double(xUnit Patterns.com)](http://xunitpatterns.com/Test%20Double.html)
- [Hudi - 테스트 더블 (Test Double)](https://hudi.blog/test-double/)
- 책 _좋은 코드 나쁜 코드 - 10.4장 테스트 더블_