
> DTO와 VO는 분명히 다른 개념이다. 그런데, 같은 개념으로 생각해서 사용하는 경우가 많다. 왜일까?  
> ⌜Core J2EE Patterns: Best Practices and Design Strategies⌟ 책의 초판에서는 데이터 전송용 객체를 `VO`로 정의했다. 그 이후 2판에서는 해당 객체를 `DTO`로 정정해서 작성했다. 이 때문에 `DTO`와 `VO`를 혼동하게 된 것 같다.

- DTO
	- 데이터를 전달하기 위한 객체
	- 주로 Layer간 데이터를 주고 받을 때 사용한다.
	- 목적 자체가 데이터를 전달하기 위함이므로 `getter/setter` 외의 기능은 **필요없다**. 
		- (필요없다고 해서 가지지 않아야 한다는 말은 아니다)
	- (내 생각) 결국 캡슐화를 위한거 아닌가? - (최소한의 필수 데이터만 제공, 본인 아래 의존성 숨기기)  
- VO
	- 어떠한 값 그 자체 (Long, BigDecimal, URL 같이 어떠한 "값")
	- 그러므로 특정한 규칙에 따라 같은 객체인지 판단한다. (`equals()`/`hashCode()` override)
		- 주로 모든 필드가 같은 값을 가지는지로 판단한다.
	- 필요에 따라 비즈니스 로직을 가질 수 있으나 꼭 가져야 하는건 아니다. (**상관없다**)

# References
- [DTO vs VO](https://velog.io/@taehee-kim-dev/DTO-vs-VO)
- [DTO vs VO vs Entity](https://tecoble.techcourse.co.kr/post/2021-05-16-dto-vs-vo-vs-entity/)