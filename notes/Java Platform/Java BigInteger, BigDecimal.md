- - -

# 주의점
중요하다고 생각하는 일부 내용만 정리함.  

출처 보는 걸 추천

# 공통점
`java.math` 패키지에 속하는 **클래스**로, **불변**이다. 메서드에서 반환되는 본인 클래스는 전부 새로운 객체이다.

이러한 특징 때문에 멀티 스레드 환경에서 쉽게 사용 가능하지만, 기본형 타입에 비해 **느리다**.

자주 사용되는 숫자는 static final 변수로 제공한다.
# BigInteger

상한, 하한이 없는 거의 무한에 가까운 수를 표현해야 할 때 사용한다.

`long`보다 더 큰 수를 생성해야 하므로 `String`, `byte[]`로 생성한다. (`int`나 `long`을 못쓰는건 아님)

내부 구현은 중요한 부분만 말하면,   
긴 숫자를 표현할 수 있도록 `int[]`인 `mag`과 부호를 표현하는 `int`인 `signum`로 구성된다.

# BigDecimal

### 필요성

`float`와 `double`는 IEEE 754 부동소수점 방식을 사용한다.   
실제 값과 완벽하지 않고 근삿값을 가지지만, 표현의 범위가 크다.   
이런 점은 일반적으로 유용하지만, 계산이 중요한 금융같은 서비스에서 치명적일 수 있다.   

정확한 소수점 표현이 필요하다면 `BigDecimal` 클래스를 사용한다.   
`BigDecimal`은 `float`, `double`보다 정밀한 결과를 보장한다.  

### 구조

> Immutable, arbitrary-precision signed decimal numbers. A `BigDecimal` consists of an arbitrary precision integer _unscaled value_ and a 32-bit integer _scale_.
> 
> (번역) 불변의 임의의 정밀도로 부호화된 십진수. BigDecimal은 임의 정밀도 정수형인 스케일 없는 값과 32비트 정수 스케일로 구성됩니다.

(호스트 시스템의 사용 가능한 메모리에 의해서만 정밀도가 제한되는 사실상) 무제한 자릿수를 제공하는 정수열을 제공하는 걸 [임의 정밀도](https://en.wikipedia.org/wiki/Arbitrary-precision_arithmetic) 수 라고 한다.

> 내부적으로 살펴보면 `intVal`, `scale`, `percision`, `intCompact`로 구성되어 있습니다. intVal은 BigInteger 타입으로서 unscaled value를 저장하는 변수입니다. scale은 소수점 오른쪽의 자릿수, percision은 총 자릿수를 나타냅니다. 특이한 점은 intCompact가 long 타입으로 선언되어 있다는 것인데, 만약 값의 크기가 작아서 유효숫자의 절댓값이 long 타입으로 표현될 수 있다면 BigDecimal은 unscaled value를 intVal 대신 intCompact에 저장함으로써 메모리를 최적화합니다.

```java
package java.math;

public class BigDecimal extends Number implements Comparable<BigDecimal> {
    
    private final BigInteger intVal; // = unscaled value

    private final int scale;

    private transient int precision;

    private final transient long intCompact;
    
    ...
```

- `intVal` 변수는 BigInteger 타입으로서 unscaled value를 저장하는 역할
- `scale` 변수는 소수점 오른쪽의 자리수를 나타낸다.
- $\text{BigDecimal} = \text{unscaled value} \times 10^{-\text{scale}}$ 이다.
	- ex: $3.14 = 314 \times 10^{-2}$

### 특징

여러 연산에서, 결과가 정확하게 나누어 떨어지지 않는 경우 `java.lang.ArithmeticException`를 던진다.

따라서 여러 연산(주로 나눗셈) 시, 반올림 정책을 사용해야 에러를 방지할 수 있다.

`valueOf()`를 사용해서 생성하려는 수가 static final로 미리 정의된 수 인 경우, 해당 객체를 반환한다.(불변이라 ㄱㅊ)  
메모리를 아낄 수 있다.

아래는 코드 일부분

```java
public static BigDecimal valueOf(long val) {
	if (val >= 0 && val < ZERO_THROUGH_TEN.length)
		return ZERO_THROUGH_TEN[(int)val];
	else if (val != INFLATED)
		return new BigDecimal(null, val, 0, 0);
	return new BigDecimal(INFLATED_BIGINT, val, 0, 0);
}
```

```java
// Cache of common small BigDecimal values.
private static final BigDecimal ZERO_THROUGH_TEN[] = {
	new BigDecimal(BigInteger.ZERO,       0,  0, 1),
	new BigDecimal(BigInteger.ONE,        1,  0, 1),
	new BigDecimal(BigInteger.TWO,        2,  0, 1),
	new BigDecimal(BigInteger.valueOf(3), 3,  0, 1),
	new BigDecimal(BigInteger.valueOf(4), 4,  0, 1),
	new BigDecimal(BigInteger.valueOf(5), 5,  0, 1),
	new BigDecimal(BigInteger.valueOf(6), 6,  0, 1),
	new BigDecimal(BigInteger.valueOf(7), 7,  0, 1),
	new BigDecimal(BigInteger.valueOf(8), 8,  0, 1),
	new BigDecimal(BigInteger.valueOf(9), 9,  0, 1),
	new BigDecimal(BigInteger.TEN,        10, 0, 2),
};
```
# Reference
- [Baeldung - Guide to Java BigInteger](https://www.baeldung.com/java-biginteger)
- [자바 BigInteger: 매우 큰 정수 표현](https://madplay.github.io/post/biginteger-in-java)
- [OpenJDK 11 BigInteger.java](https://github.com/AdoptOpenJDK/openjdk-jdk11/blob/master/src/java.base/share/classes/java/math/BigInteger.java)

- [Java SE 8 API Docs - BigDecimal](https://docs.oracle.com/javase/8/docs/api/java/math/BigDecimal.html)
- [OpenJDK 11 BigDecimal.java](https://github.com/AdoptOpenJDK/openjdk-jdk11/blob/master/src/java.base/share/classes/java/math/BigDecimal.java)
- [지마켓 기술블로그 - BigDecimal A to Z: 정확한 계산을 위한 숫자 처리 클래스](https://dev.gmarket.com/75)
- [BigInteger & BigDecimal 사용법 총정리]([https://arc.net/l/quote/ohodgzfj](https://inpa.tistory.com/entry/JAVA-%E2%98%95-BigInteger-BigDecimal-%EC%9E%90%EB%A3%8C%ED%98%95-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%B4%9D%EC%A0%95%EB%A6%AC))