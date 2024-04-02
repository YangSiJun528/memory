
### 깊은 복사 / 얕은 복사

- 얕은 복사(shallow copy)
	- 복사 시 참조값(메모리 주소)를 복사하는것. 
	- 복사한 값의 변경이 원본에 영향을 준다.
	- `clone()`은 기본적으로 얕은 복사

- 깊은 복사(deep copy)
	- 복사 시 데이터를 복사하는 것. 
	- 별도의 메모리(heap) 영역에 저장되므로 복사한 값의 변경이 원본에 영향을 주지 않는다.

Object가 `clone()`을 정의 - 기본은 얕은 복사
`Cloneable`(마커 인터페이스, 로직을 가지지 않음) 인터페이스를 사용해야만 `clone()` 재정의 가능

불변 객체라면 얕은 복사, 가변 객체라면 깊은 복사를 수행해야 한다.

그러나 Effective Java 3e Item 13에서는 `Cloneable`을 사용하지 않는 걸 권장한다.

`Cloneable`을 구현한 클래스를 확장한다면, 매 번 적절한 복사 타입으로 재정의해줘야 하기 때문이다. 그렇지 않으면 부모의 타입으로 복사가 된다.

그래서 책에서는 북제 기능은 생성자와 팩터리를 사용하는게 좋고, 배열만 예외적으로 clone이 더 낫다고 주장함.


# References
- [자바 clone 메서드 재정의 (얕은 복사 & 깊은 복사)](https://inpa.tistory.com/entry/JAVA-%E2%98%95-Object-%ED%81%B4%EB%9E%98%EC%8A%A4-clone-%EB%A9%94%EC%84%9C%EB%93%9C-%EC%96%95%EC%9D%80-%EB%B3%B5%EC%82%AC-%EA%B9%8A%EC%9D%80-%EB%B3%B5%EC%82%AC)
- [Cloneable 인터페이스와 clone 메소드](https://catsbi.oopy.io/16109e87-3c7e-4c6e-9816-c86e6b343cdd)
- [아이템13 clone 재정의는 주의해서 진행해라](https://incheol-jung.gitbook.io/docs/study/effective-java/undefined-1/2020-03-20-effective-13item)

