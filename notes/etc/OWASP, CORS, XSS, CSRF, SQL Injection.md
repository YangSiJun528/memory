주로 같이 다루는 웹 취약점 공격이나 관련 내용이라 함께 정리하였다.

(여기 설명은 일부분이다. 더 정확하고 자세한 설명은 각 출처의 Cheat Sheet 참고)

### OWASP(Open Web Application Security Project)
오픈소스 웹 어플리케이션 보안 프로젝트, 보안과 관련된 취약점을 연구하는 비영리 커뮤니티.

### CORS(Cross Origin Resource Sharing)

CSRF 같은 웹 취약점 문제를 방지하기 위해 이제 모든 (최신) 브라우저에서 Same-Origin Policy을 지킨다.

몇 가지 예외 조항을 두고 이 조항에 해당되는 리소스 요청은 같은 Origin이 아니여도 허용하기로 했는데, 그 중 하나가 “CORS 정책을 지킨 리소스 요청”이다.

##### SOP(Same-Origin Policy)
"같은 출처에서만 리소스를 공유할 수 있다"는 정책

##### CORS 에러 발생 시 해결 방법
1. Access-Control-Allow-Origin 해더 설정
2. 프록시 서버 사용해 CORS 우회하기
	1. 우회하는거니까 별로 좋은 방식은 아닌 듯?

##### CORS 동작 방식
==여기는 걍 알고만 있기==

1. Preflight Request
	1. 요청을 보내려는 경우, 먼저 `Preflight` 요청을 보내 안전한 요청인지 확인한다.
		1. `OPTIONS`메서드를 사용한다.  - ([`OPTIONS`](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/OPTIONS)을 사용해서버가 특정 요청에 지원하는 옵션들을 알 수 있다.)
	2. 이후 안전한 요청이면 실제 요청을 보낸다.
2. Simple Request
	1. `Preflight` 없이 바로 보내고 결과에서 안전한 요청인지 확인
	2. 제한적인 경우에만 실행되고, 실제론 거의 보기 어렵다.
3. Credentialed Request
	1. 인증이 필요한 리소스에 접근할 때 사용됨
		1. 요청을 보내는 쪽에서 쿠키, HTTP 인증 자격 증명 등이 포함해서 보낸다.
			1. (원래는 포함 안함)
	2. 이 경우 규칙이 더 추가된다.
#### References
- [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)
- [CORS는 왜 이렇게 우리를 힘들게 하는걸까?](https://evan-moon.github.io/2020/05/21/about-cors/#cors%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EB%8F%99%EC%9E%91%ED%95%98%EB%82%98%EC%9A%94)
- [CORS란 무엇인가요?](https://aws.amazon.com/ko/what-is/cross-origin-resource-sharing/)

### XSS(Cross Site Scripting)

악의적인 스크립트를 무해하고 신뢰할 수 있는 웹 사이트에 삽입하는 주입 유형이다.

다음과 같은 경우 발생
1. 데이터는 신뢰할 수 없는 소스(대부분 웹 요청)를 통해 웹 애플리케이션에 입력된다.
2. 데이터는 악성 콘텐츠에 대한 유효성을 검사하지 않고 웹 사용자에게 전송되는 동적 콘텐츠에 포함된다.

즉, 어떤 리소스 입력이 가능하고, 그에 대한 유효성 검증을 수행하지 않으면 발생한다.

최근에는 잘 발생하지 않는다지만, 발생하지 않는 건 아니다. [ChatGPT도 XSS](https://www.imperva.com/blog/xss-marks-the-spot-digging-up-vulnerabilities-in-chatgpt/)가 있었다.

##### 보호 방법

1. 입력 데이터의 유효성 검사를 수행해야 한다. (사용자로부터 어떤 입력을 받는 경우)
2. 악성 스크립트가 입력되어도 동작하지 않게 무효화(인코딩)해야 한다. (Output Encoding)
3. front-end 프레임워크의 보안 기능 사용

- 심층 방어 기술 - 하면 더 안전한데, 이것만 하면 안 안전함
	- Cookie Attributes: XSS 공격의 영향을 제한 가능
	- CSP(Content Security Policy): 추가 방어 계층으로 사용하는게 좋음
	- WAF(Web Application Firewalls): 알려진 공격 차단하기, 새로운 건 못 막음

#### References
- [owasp -Cross Site Scripting Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#sole-reliance-on-content-security-policy-csp-headers)
- [owasp -Cross Site Scripting (XSS)](https://owasp.org/www-community/attacks/xss/)

### CSRF(Cross-Site Request Forgery)
> CSRF는 피해자를 속여 악의적인 요청을 제출하도록 하는 공격입니다.

어떤 사이트에 인증된 상태인 브라우저에서 사용자가 의도하지 않는 특정 행동을 수행하게 하는 공격.

##### 보호 방법
(출처 사이트에 좋은 예방 방식, 의미 없는 예방 등 자세한 설명 있음, XSS가 뚫리면 CSRF 예방도 의미 없음, GET에 상태 변경이 있으면 보호 못함)

1. CSRF Token(Synchronizer Token Pattern) 사용 - Spring Security에서 제공하는 기능
2. (1을 사용할 수 없는 경우에 대한 대안) Signed Double-Submit Cookie

- 심층 방어 기술
	- Cookie SameSite 속성 지정: 속성 값에 따라 사이트 간 요청에서 도메인 확인, 브라우저가 제공 안하면 적용 안됨, 제어할 수 없는 하위 도메인이 있으면 문제
	- HTTP Header(Origin or Referer) 확인

##### 예시
은행 사이트에 인증된 브라우저(쿠키에 세션 값 저장 중)에서 악의적인 스크립트/사이트를 읽고 의도하지 않은 송금 요청이 자동으로 수행된다.

은행 사이트는 사용자의 세션 값을 보고 유효한 상태임을 확인하고 처리하였으나, 실제론 사용자가 의도한 행동이 아니였다.

##### XSS와의 차이점
(인터넷에 한국어로 된 블로그를 보면 다 비슷한 말을 하는데, 나는 잘 이해가 안 됐다.     
그냥 별개의 취약점으로 보는게 맞지 않나?)

CSRF는 XSS(신뢰하는 사이트의 악의적인 스크립트)를 통해 수행될 수도 있다.      
아니면 해커가 악성 스크립트를 숨긴 사이트를 만들거나 악성 스크립트가 담긴 메일을 보내거나.

#### References
- [spring security - Cross Site Request Forgery (CSRF)](https://docs.spring.io/spring-security/reference/features/exploits/csrf.html#csrf-explained)
- [owasp - Cross Site Request Forgery (CSRF)](https://owasp.org/www-community/attacks/csrf)
### SQL Injection + JPA

> 클라이언트에서 애플리케이션으로의 입력 데이터를 통해 SQL 쿼리를 삽입하거나 "주입"하는 것으로 구성됩니다.

최악의 경우 DB 전체를 탈취당하거나 데이터가 노출/변경될 수 있다.    
주로 기밀성(정보 노출), 인증, 권한 부여, 무결성(수정) 등에서 문제가 된다.

DB는 일반적으로 중요한 데이터를 포함하고 있기 때문에 공격자의 빈번한 표적이 된다.

##### 보호 방법

문자열 연결을 사용한 동적 쿼리 작성을 중지하거나 실행된 쿼리에 악의적인 SQL 입력이 포함되지 않도록 하면 보호할 수 있다.

1. Prepared Statements (with Parameterized Queries = 문자열 직접 합치지 파라미터 사용)
2. Allow-list Input Validation: 가능한 입력 값 미리 명시하기, 모든 경우에 해당 안됨

- 추가 방어
	- 최소 권한: 가능한 가장 적은 권한 부여하기 - OS, Application, DB 등...
	- 입력 값 유효성 검사

##### JPA의 경우
JPA도 안전한건 아닌게, 직접 쿼리 문자열(JPQL)을 더하면 문제가 발생할 수 있다.

마찬가지로 Prepared Statements를 사용해서 파라미터 값을 입력하는 것으로 해결할 수 있다.

아니면 Spring Data JPA나 QureyDSL 같은 툴을 사용해도 되고.

#### References
- [SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)
- [SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)