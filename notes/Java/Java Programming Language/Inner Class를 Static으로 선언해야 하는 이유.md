
Inner Class에 Static을 사용하지 않으면 IDE에서 경고하기도 하고, 별로 권장하지 않는다.

왜일까?

`static`이 아닌 Inner 클래스의 인스턴스는 바깥 인스턴스 없이 생성될 수 없으므로, 암묵적으로 연결된다.

이로 인해서 Inner 클래스만을 사용하더라도 외부 클래스를 참조하므로,    
더 많은 자원을 사용하고 외부 클래스가 GC의 대상이 되지 못한다.

`OuterClass.this` 처럼 Inner 클래스에서 Outer 클래스를 호출해야만 하는 상황이 아니라면, static을 꼭 선언하자.
# References
- [내부 클래스는 static 으로 선언 안하면 큰일 난다](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%9E%90%EB%B0%94%EC%9D%98-%EB%82%B4%EB%B6%80-%ED%81%B4%EB%9E%98%EC%8A%A4%EB%8A%94-static-%EC%9C%BC%EB%A1%9C-%EC%84%A0%EC%96%B8%ED%95%98%EC%9E%90)
- [Java의 내부 클래스는 static으로 선언하자](https://johngrib.github.io/wiki/java/inner-class-may-be-static/)
