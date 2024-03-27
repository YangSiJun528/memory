## 개념

자바는 소스코드와 문서를 하나로 관리한다.    
(JavaDoc을 소스코드에 작성하고 API Docs로 만드는 프로세스가 기본적으로 포함되어 있다.)

어노테이션은 구성 정보나 여러 메타데이터를 코드에 작성할 수 있게 해준다.  

이러한 어노테이션을 필요로 하지 않는 곳에서는 아무 기능도 수행하지 않는다. 

## 종류

- 표준 어노테이션
	- 주로 컴파일러에게 어떤 정보를 주기 위해서 사용한다.
- 커스텀 어노테이션: 자바에서 표준으로 제공하지 않는 어노테이션
	- Junit의 `@Test`나 Spring의 여러 어노테이션 등이 있다. (이 때는 주로 Reflection API와 함께 사용된다.)
- 메타 어노테이션: 표준/커스텀 상관없이, 다른 어노테이션에 사용되는 어노테이션을 의미한다.

![표준 어노테이션 목록](https://velog.velcdn.com/images/asyooniverse/post/6e458912-945b-41dc-bdef-5ae247e252c6/image.png)


### 그외 설명

##### @Retention
`@Retention`은 `CLASS`가 기본값이다. (소스코드/클래스 파일에 존재하나, JVM이 무시함)    
따라서 특정 프레임워크 등에서 읽어지기 위해선 `RUNTIME`으로 값을 변경해야 한다.

##### @Taget
어노테이션 적용 대상의 종류는 다음과 같다.

![@Taget 타입 목록](https://velog.velcdn.com/images%2Fneity16%2Fpost%2F71ef79b5-40d0-4d68-8538-29b0cca9013d%2Fimage.png)

# References
- 자바의 정석 3판 12-3 
- [wikipedia - Java annotation](https://en.wikipedia.org/wiki/Java_annotation)