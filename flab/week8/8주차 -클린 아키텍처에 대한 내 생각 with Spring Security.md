
이전에 고등학교를 다니면서 친구들, 팀에서 클린 아키텍처에 대한 이야기가 자주 나왔다.

실제로 클린 아키텍처, 나아가 헥사고날, 멀티모듈에 관심을 가지고 적용하는 팀도 있었다. (나도 그 중에 하나다. 나는 멀티모듈만 사용하긴 했지만...)

다만 F-Lab을 통해서 개인으로 프로젝트를 개발하면서 클린 아키텍처를 꼭 지켜야 할 필요가 없다는 생각이 들었다.

#### 왜 클린 아키텍처는 까다로운가?

여러 이유가 있지만, **외부 의존성의 배제**라는 핵심 목적을 가지고 있기 때문이라고 생각한다.

비즈니스 로직을 담은 코드에 외부 의존성을 배제하면 가장 핵심적인 코드만 필요하게 된다.

그럼 변경하기도 쉽고, 외부 의존성에 대한 영향을 받지 않는다. 또 테스트코드도 작성하기 쉽고, 응집도도 높아진다.

하지만 이는 반대로, 이미 잘 만들어진 기능을 사용할 수 없고 직접 구현해야 한다는 의미이다.

회사 단위의 규모가 크고, 변경 가능성이 높고, 비즈니스가 복잡한 서비스라면 모르겠지만, 개인 프로젝트에서는 이러한 구현 작업이 빠른 진행의 발목을 잡는다.

#### 이전 팀 프로젝트(Hello,GSM)에 대한 가벼운 회고

Hello,GSM이라는 팀 프로젝트를 개발할 때, 헥사고날을 참고해서 Spring Security는 구성, 설정과 관련된 클래스 외에 Web, Service, API, Repository 같은 모든 영역에서 의존하지 않도록 설계하였다.

왜나면 그게 외부 의존성을 배제하는 방법이니까... 

하지만 지금 생각해보면 꼭 그런 클린한 설계가 중요했을까? 하는 생각이 든다.

외부 구성(Request Based Authorization)으로 API에 대한 접근 권한 처리를 하고, 
또 서비스 객체에서도 유효성 검사를 한다. 왜? Spring Security는 웹이나 Spring에서 지원하는 통신만 호환되니까.
나중에 다른 곳에서 사용될수도 있기 때문이였다.

그런데 실제로 그럴 확률이 얼마나 될까. 애초에 서비스 확장이 어렵고 한계가 있는데다가, HTTP 통신 외에는 별 다른 기능을 추가하기도 애매하다. (해봐야 SMS 문자 알람 기능을 확장한다던가?)

게다가, 비즈니스 객체가 하는 일이라고 해봐야 CRUD + 권한체크 정도인데, 그럼 그냥 스프링 통합 테스트로 작성해서 비즈니스 로직(권한 + CRUD)를 확인하고, Repository 정도만 Mocking해도 테스트 코드가 복잡해지도 않을 것 같다. (아니면 권한은 통합, CRUD는 단위 테스트로 하던가.)

만약 Spring Security의 Method Based Authorization을 사용해서 서비스 객체 메서드에`@PreAuthorize`, `@PostAuthorize`로 검사했으면 코드 량도 줄이고, 더 신뢰성있는 코드가 되지 않았을까?

#### 개인 프로젝트를 진행하면서 바뀐 생각

내가 클린 코드에 대한 생각이 바뀌게 된건, 개인 프로젝트를 진행하면서 생산성이 떨어진다고 느꼈기 때문이다.

왜 확장 가능성도 적고, 외부 의존성을 변경하지도 않을 예정인 개인 프로젝트에서까지 클린한 코드,설계를 따라야 하는가?

차라리 그 시간에 Spring Security나 다른 모듈의 기능을 적극적으로 사용하면 더 빠른 개발이 가능한거 아닌가? (물론 테스트코드는 필수)

지금 프로젝트는 더 확장할 기능도 몇 개 없어서 기존에 작성한 코드를 굳이 엎을 생각은 없지만, 최소한 스프링이나 다른 신뢰할만한 기능을 사용할 수 있으면 사용할 생각이다. (아니면 나중에 개인 프로젝트에서나)

#### 정리: 클린 아키텍처는 항상 좋은가?

결론부터 말하자면 나는 그렇지 않다고 생각한다. 클린 아키텍처 도입으로 인한 장/단점을 고려해봐야 한다.   
특히 나는 프로젝트 규모(인원, 기간)가 작아질수록 클린 아키텍처가 가지는 장점이 줄어든다고 생각한다.

[[NHN FORWARD 22] 클린 아키텍처 애매한 부분 정해 드립니다.](https://youtu.be/g6Tg6_qpIVc?si=xE2c0P7wxspXdNQm)에서만 해도 소규모의 프로젝트에서는 사용하는게 좋지 않다고 한다.

또한 클린 아키텍처라는 용어를 만든 마틴 파울러의 발표에서도 비슷한 내용이 나온다. (O'Reilly에서 주관한거 같은데 이름은 잘 모르겠다.)

 요약하자면, 좋은 디자인의 설계는 초기 속도가 느리더라도 장기적으로는 빠른 개발을 가능하게 하고, 아키텍처 디자인이 없으면 그 반대라고 볼 수 있다. 따라서 장기적으로 비용을 아끼기 위해서는 좋은 디자인이 중요하다는 말을 한다.

하지만 벡엔드 1~3명 정도의 작은 사이트 프로젝트 정도에서 클린 아키텍처, 헥사고날 아키텍처를 사용한다고 하면, 그게 실제로 유용할까. 프로젝트의 외부 의존성을 변경하거나 확장될 수 있나. 클린 아키텍처를 위해 (당장은) 낮아지는 생산성보다 더 유용하다고 볼 수 있을까. (특히 그런 의존하는 프로젝트가 스프링 같이 높은 신뢰도의 도구라면 더더욱 변경 가능성이 떨어진다고 본다.)

결국 클린 아키텍처는 규모가 커질수록 떨어지는 개발 속도(확장성, 테스트, 복잡한 코드 등)로 인해 탄생하였고, 그 정도의 규모가 아닌 프로젝트라면 비효율적일 수 있다.


