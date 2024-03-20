Java Bean Validation, 말 그대로 Java Bean 유효성 검사 API이다. ([JavaBeans](notes/Java/Java%20Programming%20Language/JavaBeans.md) 참고)

- #### 버전
	- JSR 303가 Java Bean Validation 1.0
	- JSR 380가 Java Bean Validation 2.0
		- 일부 어노테이션이 추가되었다.
	- 이후 JavaEE가 이클립스 재단으로 이관되서 Jakarta Bean Validation 3.0
		- (패키지 이름이 바뀌었다는 것 외에는 변경사항이 없는 것 같다.)

Bean Validation 자체는 Specification이므로 구현체가 필요한데, 대표적으로 `hibernate-validator`가 있다. (persistence 관점의 Hibernate와는 완전히 별개이다. `hibernate-validator`에서만 제공하는 기능도 있다.)

여러 validation annotations를 제공한다. 이 어노테이션을 보고 `Validator`이 유효성 검사를 한다.    
(Spring에서는 AOP나 이미 구현된 부분에서 의존성 검사를 수행해서 `ValidatorFactory`를 직접 사용할 일은 없을 것이다. [Spring Bean Validation](notes/Spring/Spring%20Bean%20Validation.md)은 따로 정리하였다.)

`ValidatorFactory`를 통해 생성된 `Validator`를 사용해서 검사를 진행한다. 

#### @Valid 어노테이션
[@Valid](https://jakarta.ee/specifications/platform/9/apidocs/jakarta/validation/valid)은 명세가 간단해서 명세를 보는게 좋다.

> Marks a property, method parameter or method return type for validation cascading.
> Constraints defined on the object and its properties are be validated when the property, method parameter or method return type is validated.
> This behavior is applied recursively.

기본적으로 nested class나 field단위로 validation을 수행하기 때문에, cascading하게 동작하고 싶으면 `@Valid`를 붙여야 한다. 이 때 재귀적으로 동작한다.

### JPA에서?
Hibernate인지 JPA의 옵션인지는 잘 모르겠지만 (아마 Hibernate 같은데, 정확한 출처를 찾지 못함, 어쩌면 Spring Data JPA일 수도?), 영속성 관리의 대상이 되는 객체의 경우, bean validation annotations가 DB의 컬럼에도 적용이 된다. 

# References
[Java Bean Validation 제대로 알고 쓰자](https://kapentaz.github.io/java/Java-Bean-Validation-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EA%B3%A0-%EC%93%B0%EC%9E%90/#) - 잘 설명해주는 한국어로 된 블로그. 추천한다.
[baeldung - Java Bean Validation Basics](https://www.baeldung.com/java-validation) - 간단한 어노테이션들 정보와 소개
[Jakarta Bean Validation 3.0](https://beanvalidation.org/3.0/)