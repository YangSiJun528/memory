- - -
## 개요

HTTP API 에러 응답 포멧 표준인 RFC9457를 알아보자.

RFC 7807는 RFC 9457로 대체되었지만, 큰 기능 면에서는 거의 동일하다.   
RFC 9457의 부록 D를 보면 차이점을 알 수 있다.

## RFC9457 설명

media type으로 `application/problem+json`, `application/problem+xml`를 사용한다.

모든 HTTP 상태 코드로 사용할 수 있으나 4xx, 5xx 응답에 가장 자연스럽다.

> This specification's aim is to define common error formats for applications that need one so that they aren't required to define their own or, worse, tempted to redefine the semantics of existing HTTP status codes. Even if an application chooses not to use it to convey errors, reviewing its design can help guide the design decisions faced when conveying errors in an existing format.
> 
> 이 사양의 목적은 애플리케이션이 자체적으로 오류 형식을 정의할 필요가 없도록, 또는 기존 HTTP 상태 코드의 의미를 재정의하고 싶지 않도록 일반적인 오류 형식을 정의하는 것입니다. 애플리케이션이 오류를 전달하기 위해 이 형식을 사용하지 않기로 선택하더라도 설계를 검토하면 기존 형식으로 오류를 전달할 때 직면하는 설계 결정을 안내하는 데 도움이 될 수 있습니다.

일반적인 어플리케이션에서 표준적으로 사용할 수 있는 오류 응답 형식을 정의하는 것을 목표로 한다.

다음과 같은 멤버를 가진다. (원문에서 member라고 부른다.)
- **type** : 문제 유형을 **식별**(id 역할)하는 URI 참조이다. URL이라면, 역참조(dereferenced, 원인 파악의 의미인가?) 시 문제에 대해 사람이 읽을 수 있는 문서를 제공해야 한다. 만약 없다면 `about:blank` 값을 가진다.
- **title** : 사람이 읽을 수 있는 문제 유형에 대한 간략한 요약이다. type 필드와 연결되어 사용된다. localization을 제외하고는 변경되지 않아야 한다. 필수 멤버는 아니다. 오프라인 등의 type을 읽을 수 없는 상황을 위해서 사용한다. (일종의 대체 식별자)
- **status** : 이 문제에 대해 서버가 생성한 HTTP 상태 코드. HTTP 응답의 상태 코드와 일치해야 한다. 다만, 일반적인 클라이언트는 HTTP응답의 상태 코드를 의존해야 한다. 이는 미들웨어로 인해 HTTP 응답이 변조되는 경우를 위함이다.
- **detail** : 이 문제 발생과 관련된 사람이 읽을 수 있는 설명. 문제를 진단하거나 해결하는 데 도움이 될 수 있는 보다 자세한 메시지를 제공한다. detail 멤버를 구문 분석 하면 안된다. (detail을 의존해서 코드를 작성하지 말라는 의미로 보임)
- **instance** : 문제의 특정 발생을 식별하는 URI 참조이다. (이해를 못 헀는데, 요청 시 사용 된 URI를 말하는걸까?)
- **Extension Members** : 필요에 의해서 추가 멤버를 정의하고 사용할 수 있다.

## Spring의 지원
Spring 6, Spring Boot 3부터 RFC7807를 지원한다. 
작성일(3. 15. 2024.) 기준으로 RFC9457를 지원한다는 말은 아직 없다.


# References
- [rfc9457](https://datatracker.ietf.org/doc/html/rfc9457), [rfc7807](https://datatracker.ietf.org/doc/html/rfc7807)
- [Problem Details for HTTP APIs - RFC 7807 is dead, long live RFC 9457](https://blog.frankel.ch/problem-details-http-apis/)
- [Charge your APIs Volume 19: Understanding Problem Details for HTTP APIs - A Deep Dive into RFC 7807 and RFC 9457](https://www.codecentric.de/wissens-hub/blog/charge-your-apis-volume-19-understanding-problem-details-for-http-apis-a-deep-dive-into-rfc-7807-and-rfc-9457)
- [Spring Web MVC - Error Responses](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-ann-rest-exceptions.html)