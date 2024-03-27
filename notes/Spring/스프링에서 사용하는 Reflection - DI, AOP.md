
스프링 프레임워크에서 Reflection은 핵심적인 역할을 한다.

여러 곳에 쓰인다고 알고 있지만, 대표적으로 DI와 AOP에 대해서 알아보자.

### DI(Dependency Injection)

의존성 주입은 객체 간의 의존 관계를 외부에서 주입하는 것을 말한다.
스프링은 Reflection을 사용하여 의존 객체를 주입하고 관리한다.

스프링의 [`AutowiredAnnotationBeanPostProcessor`](https://github.com/spring-projects/spring-framework/blob/main/spring-beans/src/main/java/org/springframework/beans/factory/annotation/AutowiredAnnotationBeanPostProcessor.java) 클래스는 `@Autowired` 어노테이션이 붙은 필드나 메서드를 찾아서 의존 객체를 주입한다.
이 과정에서 `ReflectionUtils`와 `ReflectionHelper` 클래스를 사용하여 Reflection 기능을 활용한다.


### AOP(Aspect Oriented Programming)

AOP는 횡단 관심사를 분리함으로서 모듈성을 증가시키는 것을 목적으로 하는 프로그래밍 패러다임이다.

대표적으로 `@Transactional`어노테이션을 처리할 때, AOP가 사용된다.
[`TransactionInterceptor`](https://github.com/spring-projects/spring-framework/blob/main/spring-tx/src/main/java/org/springframework/transaction/interceptor/TransactionInterceptor.java)가 호출되는데, 해당 클래스의 내부를 보면 `AopUtils`라는 걸 사용한다.

[`AopUtils`](https://github.com/spring-projects/spring-framework/blob/main/spring-aop/src/main/java/org/springframework/aop/support/AopUtils.java)의 코드를 보면 여러 리플렉션 기능을 사용하기 편하게 Util로 제공하는 모습을 볼 수 있다.

# References
- Spring 프레임워크 Github 소스코드