## Optional이 만들어진 이유와 의도

> … it was not to be a general purpose Maybe type, as much as many people would have liked us to do so. Our intention was to provide a limited mechanism for library method return types where there needed to be a clear way to represent “no result” …
> 
> `Optional`은 많은 사람들이 우리(자바 언어 설계자)에게 기대했던 범용적인 `Maybe` 타입과는 다르다. 라이브러리 메서드가 반환할 결과값이 ‘없음’을 명백하게 표현할 필요가 있는 곳에서 제한적으로 사용할 수 있는 메커니즘을 제공하는 것이 `Optional`을 만든 의도’였다.  
> -Brian Goetz-

Optional은 NPE(NullPointerException) 발생을 방지하기 위해 만들었고, 반환타입으로 사용해야 한다.  
그 외에는 의도와 다른 사용법이므로 지양하는게 좋다.

### 요약
1.  `isPresent()-get()` 대신 `orElse()/orElseGet()/orElseThrow()`
2.  `orElse(new ...)` 대신 `orElseGet(() -> new ...)`
	- `orElse`와 `orElseGet`는 동일하게 결과가 없으면 해당 메서드의 결과를 사용하도록 한다.
	- `T orElse(T other)`는 항상 자원이 생성된다. 그러므로 미리 생성된 값만 사용하는게 좋다.
	- `T orElseGet(Supplier<? extends T> supplier)`는 결과가 없을 때, `Supplier`를 호출하여 자원을 생성한다.
	- 즉, 이미 생생된 객체를 반환하는게 아니라면 `orElseGet`으로 Lazy하게 자원을 생성하도록 해야 한다.
3.  **단지 값을 얻을 목적**이라면 `Optional` 대신 **`null` 비교**
	- Optional은 상대적으로 비싼 자원이다.
	- `return Optional.ofNullable(status).orElse(READY);`
	- `return status != null ? status : READY;` << 이게 더 좋다.
4.  `Optional` 대신 **비어있는 컬렉션 반환**
5.  `Optional`을 **필드로 사용 금지**
6.  `Optional`을 생성자나 메서드 **인자로 사용 금지**
7.  `Optional`을 컬렉션의 **원소로 사용 금지**
8.  `of()`, `ofNullable()` 혼동 주의
9.  int, long, double에 대해선`OptionalInt`, `OptionalLong`, `OptionalDouble` 사용하기
	- Boxing/Unboxing이 발생하지 않는다.

# References
- - -

공식 API 문서
```embed
title: 'Optional (Java SE 9 & JDK 9 )'
image: 'https://docs.oracle.com/favicon.ico'
description: 'Returns an empty Optional instance.'
url: 'https://docs.oracle.com/javase/9/docs/api/java/util/Optional.html'
```

API 문서에서 주의사항만 따로 정리한 것 - 이 글 외에 더 자세한 설명이 있다. 
```embed
title: '26 Reasons Why Using Optional Correctly Is Not Optional - DZone'
image: 'https://dz2cdn1.dzone.com/storage/article-thumb/10768496-thumb.jpg'
description: 'We take a look at the top 25 most important concepts to keep in mind when working with Optionals in Java, focusing on null variables and more.'
url: 'https://dzone.com/articles/using-optional-correctly-is-not-optional'
```

```embed
title: 'Java Optional 바르게 쓰기'
image: 'https://www.gravatar.com/avatar/30f0244ab86396288cdb62c3591c0c30?s=640'
description: 'Java Optional 바르게 쓰기Brian Goetz는 스택오버플로우에서 Optional을 만든 의도에 대해 다음과 같이 말했다. … it was not to be a general purpose Maybe type, as much as many people would have liked us to do so. Our intention was to p'
url: 'https://homoefficio.github.io/2019/10/03/Java-Optional-%EB%B0%94%EB%A5%B4%EA%B2%8C-%EC%93%B0%EA%B8%B0/'
```
