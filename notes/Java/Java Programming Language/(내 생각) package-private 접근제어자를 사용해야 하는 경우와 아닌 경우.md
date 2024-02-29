
개인적인 생각이므로 올바르지 않을 수 있고, 변할 수도 있는 의견임을 주의하고 봐주세요.

### 요약
- package-private는 Visibility나 Privacy의 영역을 패키지 단위로 확장시킨다.
- 일반적인 프로젝트에서는 package-private를 사용하지 말아야 한다.
- 프레임워크같은 코드를 제공하는 프로젝트는 package-private를 사용해야 한다.

### 배경
_이펙티브 자바 3의 15. 클래스와 멤버의 접근 권한을 최소화하라_ 에서 접근 권한을 최소화하라고 한다. 이 책 뿐만이 아니라 일반적으로 접근에 제한을 두고, 필요하다면 허용해야 한다. 이 말은 규모가 있는 프로젝트를 유지보수 해본 사람이라면 누구나 공감할 것이다.

package-private는 여러 객체와 함께 동작하는 객체에서 유일하게 다른 패키지의 접근을 막을 수 있는 방법이다. 따라서 구체적인 구현을 외부 패키지에 제공하지 않기 위해 사용할 수 있다.

하지만 실제로 자바를 사용하다보면 package-private은 거의 사용하지 않는다. 이유가 무엇일까?

이 [블로그](https://hyeon9mak.github.io/Java-dont-use-package-private/)에서 어느정도 답을 찾았다. 정확히는 글의 출처 란에 있는 [Don’t use package private in java](https://hummusandmagnets.tumblr.com/post/52285020299/dont-use-package-private-in-java)이다.

### 실무 프로젝트는 package-private가 유용하지 않다.

결론부터 말하자면, 실무 프로젝트에서 package-private는 사용하지 말아야 한다. 왜 그럴까? 

> Your project may have a well-designed package structure but it’ll be one you’ve made up yourself, there is no canonical and established way to do that. And unusually you won’t have one, you’ll have just split you types into several vaguely meaningful chunks without necessarily giving it too much though.

패키지의 구조에 대해선 확립된 기준이 없다. 회사, 팀, 사람에 따라 제각각으로 패키지를 구성할 것이다.

그런 패키지 자체는 아무 문제 없다. 단, 패키지가 Visibility나 Privacy의 대상이 되면 달라진다. 개념적으로 이 패키지 내부에서만 객체를 사용하는게 올바른지를 답할 수 있어야 한다. 그리고 만약, 서비스가 확장되어 클래스가 너무 많아 패키지를 분리해야 한다면 어떻게 할 것인가? 테스트용 패키지가 따로 있다면 어떻게 패키지 클래스에서 객체를 import 할 것인가?

실무에서 이러한 것을 신경쓰기에는 너무나 벅차다.

package-private이 지금 당장은 적절해 보일지 모른다. 하지만 패키지 구조에 대한 규칙이 있고, 코드 품질을 위해 심도있게 고민하는 팀이 아니라면 언젠가는 문제가 된다.

정리하자면, package-private를 사용하게 된다면 패키지 구조를 신경써야 하나, 일반적으로 패키지 구조에 대한 기준이 존재하지 않아 피로도만 늘리기 때문에 package-private를 사용하지 않는다.

### package-private를 사용해야 하는 경우

외부에 라이브러리, 프레임워크를 제공하는 프로젝트의 경우에는 오히려 package-private을 꼭 사용해야 한다.

우선 외부 사용자가 있는 코드의 경우에, 패키지 변경을 포함한 모든 변경은 쉽지 않다.   
그리고 외부 사용자가 사용할 수 없게 캡슐화 된 클래스가 필요하다.   

이러한 점 때문에 프레임워크 같이 코드를 제공하는 프로젝트를 제공하는 팀은 사용자가 서비스를 잘못 사용하지 않도록 막아야 한다. 그래서 처음부터 패키지 구조, 캡슐화, 확장성 등 여러 가지를 고려해야 하고, 패키지 단위의 Visibility나 Privacy도 신경써야 한다.

자바 `java.util.*` 패키지나 spring 프로젝트의 내부 구현만 봐도 외부에서 사용을 원치 않는 클래스의 경우에 package-private를 사용하는 모습을 볼 수 있다. (주로 final, (내부에서만 사용하는) abstract, Utils 등)

# References
- [Java package-private 은 안쓰나요?](https://hyeon9mak.github.io/Java-dont-use-package-private/)
- [Don’t use package private in java](https://hummusandmagnets.tumblr.com/post/52285020299/dont-use-package-private-in-java)
- [Is Java package level scope useful?](https://softwareengineering.stackexchange.com/questions/294640/is-java-package-level-scope-useful)