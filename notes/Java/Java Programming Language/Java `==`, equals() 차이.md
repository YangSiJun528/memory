- - -
# `==`

- 기본형 비교 시: 값 자체를 비교함
- 참조형 비교 시: 동일한 객체를 참조하는지 비교함

개인적인 추측이지만, JVM call stack에서 가지는 값끼리 비교해서 이런 결과가 나오는게 아닐까 싶다.   
참조형 타입이면 참조하는 객체의 주소값을 가지고 있으니까.

# equals()

객체의 경우, 특성에 따라 다른 객체더라도 동일하게 판단되어야 하는 경우가 있다.   
하지만 `==`는 이러한 기능을 제공하지 않으므로, `Object` 객체는 `equals()` 메서드를 제공한다.

기본적인 구현은 `==`과 동일하지만 override 하여 객체의 특성에 맞게 재정의하여 사용할 수 있다.

# Reference
- [Difference Between == and equals() in Java](https://www.linkedin.com/in/babar-shahzad-018035171/)
- 자바의 정석
- [Oracle Java SE 8 JLS 15.21](https://docs.oracle.com/javase/specs/jls/se8/html/jls-15.html)