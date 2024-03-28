
### HandlerExceptionResolver
[HandlerExceptionResolver](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/HandlerExceptionResolver.html)는 핸들러 매핑, 실행 중에 던져지는 예외를 처리하는 역할을 한다. 

`DispatcherServlet.initHandlerExceptionResolvers()`에서 `HandlerExceptionResolver`들을 등록한다.

![Spring MVC 예외처리 flow](https://parkmuhyeun.github.io/assets/img/blog/woowacourse/exc_5.png)

HandlerExceptionResolver와 그 구현체들 다이어그램 (아래 설명할 3가지 말고 나머지는 무슨역할 하는지 모르겠다. 실제로 사용되고 있긴 하나?)
![HandlerExceptionResolver와 그 구현체들 다이어그램](notes/Spring/files/HandlerExceptionResolver%20구현체%20다이어그램.png)

`HandlerExceptionResolverComposite`에서 우선순위를 지정하는데, 기본 설정은 다음과 같다. (실제로 코드 상에서는 순서를 명시적으로 지정을 안 해서 확실하진 않은데, 아마 맞을듯)

1. `ExceptionHandlerExceptionResolver`
	- `@ExceptionHandler`가 붙은 메소드를 통해 예외를 해결한다.
2. `ResponseStatusExceptionResolver`
	- `@ResponseStatus`가 붙거나 `ResponseStatusException` 예외를 해결한다.
3. `DefaultHandlerExceptionResolver`
	- 표준 Spring MVC 예외를 해결하고 이를 해당 HTTP 상태 코드로 변환한다.
숫자가 낮을수록 우선순위가 낮다.

하지만 실제론 HandlerExceptionResolver를 직접 정의하지는 않고, 스프링에서 제공하는 `@ExceptionHandler`, `@ControllerAdvice`를 사용한다. 


# References
- Spring Boot 3.2.x 기준 실제 코드 + JavaDoc
- [[API 예외 처리] 스프링이 제공하는 ExceptionResolver (ResponseStatus, DefaultHandler)](https://develop-writing.tistory.com/101)