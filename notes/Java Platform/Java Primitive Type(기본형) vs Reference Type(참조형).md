- - -
# Primitive Type(기본형)

> 기본 유형은 Java 프로그래밍 언어로 사전 정의되고 예약된 키워드( [§3.9](https://docs.oracle.com/javase/specs/jls/se10/html/jls-3.html#jls-3.9 "3.9. 키워드") )로 이름이 지정됩니다.   
> [Oracle SE 10 JLS Spec](https://docs.oracle.com/javase/specs/jls/se10/html/jls-4.html)


기본형으로 논리형 (boolean),  문자형 (char),  정수형 (byte, short, int, long)  실수형 (float, double)이 존재한다.

이러한 기본형의 타입 이름은 자바 언어의 예약어로 설정되어 있다.

특징은 다음과 같다.
- 소문자로 시작
- 기본값이 존재하며, null을 가질 수 없다.
- 불변이며, 다른 기본값과 상태를 공유하지 않는다.


# Reference Type(참조형)

> 참조 값(종종 그냥 _reference_ )은 이러한 개체에 대한 포인터이자 개체를 참조하지 않는 특수한 null 참조입니다.  
> [Oracle SE 10 JLS Spec](https://docs.oracle.com/javase/specs/jls/se10/html/jls-4.html)

기본형을 제외한 모든 타입을 의미하며, 모두 `Object`를 상속한다. (Interface, Array 제외)

Class, Interface, Enum, Array, null 유형이 존재한다.

특징은 다음과 같다.
- 실제 값이 아닌, 객체가 저장된 주소 값을 가진다. (객체는 Heap에 저장된다.)
	- 객체와 참조형 변수는 별개이다.
- null 표현을 사용해 주소 값을 가지지 않는 상태를 표현할 수 있다.

# Reference
- [Oracle SE 10 JLS Spec](https://docs.oracle.com/javase/specs/jls/se10/html/jls-4.html)
- [JAVA 변수의 기본형 & 참조형 타입 차이 이해하기](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EB%B3%80%EC%88%98%EC%9D%98-%EA%B8%B0%EB%B3%B8%ED%98%95-%EC%B0%B8%EC%A1%B0%ED%98%95-%ED%83%80%EC%9E%85)