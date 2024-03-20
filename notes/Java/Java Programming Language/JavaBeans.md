자세히는 아직도 잘 모르겠고, 지금 널리 쓰이는 개념인지도 모르겠다.
탄생 이유도 Swing 때문에 탄생했다거나, 그게 아니라 다른 이유 때문이라고 하고...    
자료도 많이 없어서 나중에 공식문서나 튜토리얼을 제대로 봐야 이해할 거 같은데, 그럴 일이 있을까?

## Java Bean이란?

JavaBeans 라고 붙여서 부르기도 한다. Spring의 Bean과는 다른 개념이다. (연관이 없는건 아닌데, 아래에서 설명한다.)

[JavaBeans Specification](https://www.oracle.com/java/technologies/javase/javabeans-spec.html)을 지키는 클래스이다.

다음과 같은 조건이 있다.

1. 직렬화 가능해야 한다. 즉, `Serializable`를 구현해야 한다.
2. `no-arg constructor`를 가져야 한다.
3. properties가 있어야 한다. public `setter/getter`를 가져야 한다. (Property에 대한 조건은 아주 조금 더 구체적인데, 일반적으로 사용하는 `setter/getter`를 생각하면 상관없을듯)
4. 모든 인스턴스 변수(static = 클래스 변수는 포함 안됨)는 private해야 한다.

왜 필요한가?

> This standardization allows the beans to be handled in a more generic fashion, allowing easier [code reuse](https://en.wikipedia.org/wiki/Code_reuse "Code reuse") and [introspection](https://en.wikipedia.org/wiki/Type_introspection "Type introspection"). This in turn allows the beans to be treated as [software components](https://en.wikipedia.org/wiki/Component-based_software_engineering "Component-based software engineering"), and to be manipulated visually by [editors and IDEs](https://en.wikipedia.org/wiki/Integrated_development_environment "Integrated development environment") without needing any initial configuration, or to know any internal implementation details.
> 
> 이러한 표준화를 통해 빈을 보다 일반적인 방식으로 처리할 수 있으므로 코드 재사용과 내성 검사가 더 쉬워집니다. 이를 통해 빈을 소프트웨어 구성 요소로 취급하고 초기 구성이나 내부 구현 세부 사항을 알 필요 없이 편집자와 IDE에서 시각적으로 조작할 수 있습니다.  
> < wikipedia >

> Beans are reusable software programs that you can develop and assemble easily to create sophisticated applications.
> 
> Bean은 재사용 가능한 소프트웨어 프로그램으로, 쉽게 개발 및 조립하여 정교한 애플리케이션을 만들 수 있습니다.  
> < openjdk >

> JavaBeans™ makes it easy to reuse software components. Developers can use software components written by others without having to understand their inner workings.
> 
> JavaBeans™를 사용하면 소프트웨어 컴포넌트를 쉽게 재사용할 수 있습니다. 개발자는 다른 사람이 작성한 소프트웨어 구성 요소를 내부 작동 방식을 이해하지 않고도 사용할 수 있습니다.  
> < orcacle >

즉, 다른 소프트웨어가 쉽게 사용할 수 있게 이러한 직렬화, `getter/setter`, 인수 없는 생성자 등 조건의 표준을 만들고, 지키도록 한 것이다.

이를 지키면 DB 저장, 외부 프레임워크에서 사용, JavaBeans를 지키는 다른 모듈을 쉽게 확장, 등 자유롭게 사용될 수 있다.

- ##### 예시
    - 내부 구현을 몰라도 리플랙션 등으로 쉽게 생성가능
    - 필요한 시점에 외부에서 값을 입력받고 `setter`로 주입 가능
    - 명세를 지키는 여러 모듈을 합쳐서(get/set 등으로 쉽게 변경/수정 가능) 더 넒은 범위의 기능을 제공하는 모듈로 만들기

#### Spring Bean과의 연관성

Spring Bean은 JavaBeans 명세를 지키고, 추가적인 기능을 제공한다.

그래서 스프링이 구현하는 여러 클래스를 보면 전부 인수 없는 생성자, `getter/setter`를 사용하여 객체를 생성하고 관리하는 모습을 볼 수 있다.

#  References
- [https://openjdk.org/groups/swing/beans/](https://openjdk.org/groups/swing/beans/)
- [https://imasoftwareengineer.tistory.com/101](https://imasoftwareengineer.tistory.com/101)
- [https://stackoverflow.com/questions/3295496/what-is-a-javabean-exactly](https://stackoverflow.com/questions/3295496/what-is-a-javabean-exactly)
- [https://en.wikipedia.org/wiki/JavaBeans](https://en.wikipedia.org/wiki/JavaBeans)
