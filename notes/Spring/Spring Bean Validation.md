
Spring에서 [Java Bean Validation - JSR 303, JSR 380](notes/Java/Java%20Programming%20Language/Java%20Bean%20Validation%20-%20JSR%20303,%20JSR%20380.md)을 기반으로 여러 확장된 기능을 제공한다.


- ### @Valid와 @Validated 차이
	- ##### @Valid
		- 상황에 따라 구체적인 구현체는 다르지만, `ArgumentResolver` 의 구현체가 Validation을 호출한다.
			- `RequestResponseBodyMethodProcessor` 또는 `ModelAttributeMethodProcessor`
			- 이로 인해 컨트롤러 계층에서만 동작한다.
			- `DataBinder`가 Validation을 수행한다.
		- `MethodArgumentNotValidException` 예외가 발생한다. - 명세에도 `@Valid` 실패 시 발생한다고 나온다.
		- `DefaultHandlerExceptionResolver`에 의해 에외가 Catch되고, 400 BadRequest가 발생한다.
	- ##### @Validated
		- Spring 프레임워크에서 제공한다.
		- AOP 기반으로 동작하며, `MethodValidationInterceptor`가 클래스에 등록된다.
			- 이로 인해 계층에 무관하게 사용 가능하다.
			- 프록시 객체를 제공해 주는 것이므로 사용하려는 변수, 파라미터 등에 `@Valid`를 입력해야 한다.
		- `MethodValidationAdapter`가 Validation을 수행한다.
			- 이는 Spring의 `MethodValidator` 인터페이스의 구현체로, 기존 Bean Validation Spec의 `Validator`를 사용한다. (Adapter 패턴, 내부 구현으로는 `SpringValidatorAdapter`를 사용하는 것 같다.)
		- `ConstraintViolationException` 예외가 발생한다. 500이 발생한다.
		- Grouping 기능을 제공한다.


# 추천하는 자료
- [Validation 어디까지 해봤니?](https://meetup.nhncloud.com/posts/223) - Custom Validator 및 기타 사용법
- 아래 출처로 남긴 블로그도 둘 다 내용이 좋다.

# References
- [Spring Boot Bean Validation 제대로 알고 쓰자](https://kapentaz.github.io/spring/Spring-Boo-Bean-Validation-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EA%B3%A0-%EC%93%B0%EC%9E%90/#)
- [[Spring] @Valid와 @Validated를 이용한 유효성 검증의 동작 원리 및 사용법 예시 - (1/2)](https://mangkyu.tistory.com/174)
- Spring Boot 3.2 환경에서 실제 코드