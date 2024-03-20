
## SpringBoot의 환경 속성Configuration Properties) 오버라이딩

1. 기본 프로퍼티 (SpringApplication.setDefaultProperties에 의해 설정됨)
2. @Configuration 클래스의 [@PropertySource](https://docs.spring.io/spring-framework/docs/6.1.4/javadoc-api/org/springframework/context/annotation/PropertySource.html) 애노테이션 (주의: 이는 [ApplicationContext](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationContext.html)가 리프레시될 때까지 Environment에 추가되지 않음)
3. 설정 데이터 (application.properties 파일 등)
4. RandomValuePropertySource (random.* 프로퍼티만 포함)
5. OS 환경변수
6. 자바 시스템 프로퍼티 (System.getProperties())
7. JNDI java:comp/env 속성
8. ServletContext 초기화 매개변수
9. ServletConfig 초기화 매개변수
10. SPRING_APPLICATION_JSON (환경변수나 시스템 프로퍼티에 포함된 인라인 JSON)
11. 커맨드라인 인수
12. 테스트의 properties 속성 (@SpringBootTest와 특정 슬라이스 테스트 애노테이션에서 사용가능)
13. 테스트의 @DynamicPropertySource 애노테이션
14. 테스트의 @TestPropertySource 애노테이션
15. Devtools가 활성화된 경우 $HOME/.config/spring-boot 디렉토리의 전역 설정 프로퍼티

숫자가 작은 순부터 큰 순으로 읽으며, 이후 오는 값들이 이전 값을 오버라이딩한다.

## Profile 별 파일
[Spring Profiles](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.profiles)을 사용해 구성의 일부를 분리하고 특정 환경에서만 실행되도록 할 수 있다.

`application-{profile}.yml`은 해당 `profile`을 사용하는 경우에만 활성화된다. (`properties`도 동일)
단, `application.yml`은 항상 실행된다. (대신 `application-{profile}.yml`에 의해 오버라이드 될 수 있다.)

`---`를 사용해 한 파일에서도 논리적으로 문서를 분리할 수 있다.
이 경우, `spring.config.activate.*`속성을 사용해서 어느 경우에 사용할 것인지 지정해야 한다.


## 커스텀 환경 속성 사용하기
`@Value` 어노테이션을 사용해 값을 가져올 수 있다.    
개인적으로 추천하지 않는다. (공식문서도 권장하지는 않는다.) 등록된 속성을 문자열 주소(`@Value("my.proper.one")`)으로 가져오는데, 값이 없을 수도 있고, 코드 상으로 어떤 값이 필요한지 알기 어렵다.

`@ConfigurationProperties`를 사용하는 것을 권장한다.  
 JavaBean Properties Binding방식으로 지정하고, `Type-safe`하게 사용할 수 있다. (구체적인 자바 객체로 매핑하는 법은 공식문서를 참고)    
 
 또, `spring-boot-configuration-processor`를 사용해 직접  [메타데이터](https://docs.spring.io/spring-boot/docs/current/reference/html/configuration-metadata.html)정보를 제공할 수 있다. (IDE에서 자동완성이나 Hint 등 기능을 제공할 수 있다.)



[공식문서 - 2.8.10. @ConfigurationProperties vs. @Value](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.external-config.typesafe-configuration-properties.vs-value-annotation)

## 그 외 읽어보면 좋을 자료
- https://www.baeldung.com/properties-with-spring
- https://devocean.sk.com/blog/techBoardDetail.do?ID=163415

# References
- [Core Features- 2. Externalized Configuration](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.external-config)
- [Configuration Metadata](https://docs.spring.io/spring-boot/docs/current/reference/html/configuration-metadata.html#appendix.configuration-metadata.annotation-processor)
- [[Spring] Profile: yml 파일 하나로 프로퍼티 관리하기](https://bugoverdose.github.io/development/spring-profile-and-environment-variables-tutorial/)