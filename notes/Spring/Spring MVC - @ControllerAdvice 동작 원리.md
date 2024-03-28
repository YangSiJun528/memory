
### @ControllerAdvice란?

> Specialization of @Component for classes that declare @ExceptionHandler, @InitBinder, or @ModelAttribute methods to be shared across multiple @Controller classes.
> 
> 여러 @Controller 클래스에서 공유할 @ExceptionHandler, @InitBinder 또는 @ModelAttribute 메서드를 선언하는 클래스에 대한 @Component의 특수화입니다.
> [Spring API Docs](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ControllerAdvice.html)

여러 `@Controller` 클래스에서 어떤 로직(주로 예외처리)을 한 곳에서 수행할 수 있다.

### @ControllerAdvice 생성/사용 과정

- ##### 생성
	- `@ControllerAdvice`가 붙은 빈이 컴포넌트 스캔을 통해 등록 될 떄, `ControllerAdviceBean`으로 등록된다.
	- `ExceptionHandlerExceptionResolver`가 필드로 `exceptionHandlerAdviceCache`를 가진다.
	- `ExceptionHandlerExceptionResolver.initExceptionHandlerAdviceCache()`에서 생성할 때, `List<ControllerAdviceBean>`을 가져와 등록한다.
- ##### 사용
	- `ExceptionHandlerExceptionResolver`가 `exceptionHandlerCache`(Controller에 있는 `ExceptionHandler`)를 먼저 확인하고 있으면 해당 객체에 예외 해결을 위임한다.
	- 없다면 `exceptionHandlerAdviceCache`에 등록된 `ExceptionHandler`을 읽고 처리한다.

### @ControllerAdvice는 AOP인가?
`@ControllerAdvice`의 Advice는 AOP에서 사용되는 용어이다.
CGLIB나 Proxy를 사용하지 않지만, 횡단 관심사(예외처리 등)을 분리하고 모듈화 한다는 점에서 AOP라고 볼 수 있다.

AOP는 어떤 구현 방식이라기보다 패러다임이기 때문이다.


# 관련 메모
- [Spring MVC - 예외처리 HandlerExceptionResolver](notes/Spring/Spring%20MVC%20-%20예외처리%20HandlerExceptionResolver.md)

# References
- [[Spring] ControllerAdvice는 AOP로 구현되어 있을까? ControllerAdvice의 동작 과정 살펴보기](https://mangkyu.tistory.com/246)