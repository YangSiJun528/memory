- - -

```java
/**  
 * A class with an {@code @ExceptionHandler} method that handles all Spring MVC  
 * raised exceptions by returning a {@link ResponseEntity} with RFC 7807  
 * formatted error details in the body. * * <p>Convenient as a base class of an {@link ControllerAdvice @ControllerAdvice}  
 * for global exception handling in an application. Subclasses can override  
 * individual methods that handle a specific exception, override * {@link #handleExceptionInternal} to override common handling of all exceptions,  
 * or override {@link #createResponseEntity} to intercept the final step of creating  
 * the {@link ResponseEntity} from the selected HTTP status code, headers, and body.  
 * * @author Rossen Stoyanchev  
 * @since 3.2  
 */
```
작성일 기준 JavaDoc 내용은 위와 같다.

> 본문에 RFC 7807 형식의 오류 세부정보가 포함된 ResponseEntity를 반환하여 Spring MVC에서 발생한 모든 예외를 처리하는 @ExceptionHandler 메서드가 있는 클래스입니다. 
> 애플리케이션에서 전역 예외 처리를 위한 @ControllerAdvice의 베이스 클래스로 편리하게 사용할 수 있습니다. 서브클래스는 특정 예외를 처리하는 개별 메서드를 재정의하거나, handleExceptionInternal을 재정의하여 모든 예외의 공통 처리를 재정의하거나, createResponseEntity를 재정의하여 선택한 HTTP 상태 코드, 헤더 및 본문에서 ResponseEntity를 생성하는 마지막 단계를 가로챌 수 있습니다.

정리해보면,
- Spring MVC 에러를 처리하는 기본 클래스이다.
- 상속하여 기존 MVC의 에러 처리 방식을 편리하게 재정의할 수 있다. (= ResponseEntityExceptionHandler를 상속해야 하는 이유)
- RFC 7807 형식을 지킨다. - 아마 스프링 부트 3 올라가면서 반영되었을 것이다. 예전 인터넷 자료는 다른 형식을 가진다.

# References
- [지마켓 기술블로그 - 일관적인 에러응답을 달라!](https://dev.gmarket.com/83)
- [docs.spring.io - ResponseEntityExceptionHandler](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/mvc/method/annotation/ResponseEntityExceptionHandler.html)

