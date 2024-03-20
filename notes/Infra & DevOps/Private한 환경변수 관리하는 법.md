Spring이나 여러 프레임워크를 사용해서 개발하다보면, 외부로 노출되지 않아야 하는 값들이 있다. (ex: DB url, 여러 서비스 Token, Password 등)

이러한 값들을 어떻게 관리하는가?

1. yml, properties, .env 등 특정 파일에 하드코딩
	- 대부분 프레임워크에서 이러한 특정 파일을 읽는다.
2. 환경변수로 사용하고 외부에서 주입
	- 재사용 가능하다.
	- 외부에서 값을 주입하므로 특정 파일을 수정할 필요 없이 실행 환경의 환경변수만 수정해주면 된다.
3. AWS Secrets Manager같은 Secret 관리 서비스
	- AWS 같은 외부 환경에서 관리하는 환경변수를 사용할 수 있다.
	- 관리면에서 유용하다.
		- AWS의 다른 기능과 통합
		- RDS등 다른 인스턴스의 환경변수(비밀번호 등)를 주기적으로 Rotate
		- 값 별로 Group해서 재사용
		- Audit 및 Monitoring

실무에서는 주로 3번을 사용한다.

# References
- [What is AWS Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html)
- [AWS Secrets Manager로 안전하게 보안 정보 관리하기](https://www.youtube.com/watch?v=I-bgXqpuOqU)