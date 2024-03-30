
사실 별로 사용할 일은 없을거 같긴 한데, 프레임워크 뜯어보다보니 종종 보여서 나중에 자료 찾기 쉽게 기록함.

> 앞에서 설명한 것처럼, 원래 GC 대상 여부는 reachable인가 unreachable인가로만 구분하였고 이를 사용자 코드에서는 관여할 수 없었다. 그러나 java.lang.ref 패키지를 이용하여 reachable 객체들을 strongly reachable, softly reachable, weakly reachable, phantomly reachable로 더 자세히 구별하여 GC 때의 동작을 다르게 지정할 수 있게 되었다. 다시 말해, GC 대상 여부를 판별하는 부분에 사용자 코드가 개입할 수 있게 되었다.
> 
> ...
> 
> 앞에서 설명한 것처럼 reachability는 총 5종류가 있고 이는 GC가 객체를 처리하는 기준이 된다. Java 스펙에서는 이들 5종류의 reachability를 "Strengths of Reachability"라 부른다.
> 
> [Java Reference와 GC](https://d2.naver.com/helloworld/329631)

reachability를 설정해줘서 GC의 동적을 유도할 수 있다.

대충 보니까 프레임워크에서는 `WeakReference<T>`(`T`를 참조하는게 WeakReference거나 더 약하면? 참조하고 있어도(weakly reachable 상태) GC가 제거함)을 주로 사용해서 케싱이나 메모리 관리에 사용하는 듯.

자세한 건 그림하고 같이 잘 설명해주는 [이거](https://d2.naver.com/helloworld/329631) 참고

# 괜찮은 자료 & 참고자료
- https://medium.com/@ramtop/weak-soft-and-phantom-references-in-java-and-why-they-matter-c04bfc9dc792
- https://tourspace.tistory.com/42
- https://lion-king.tistory.com/entry/Java-%EC%B0%B8%EC%A1%B0-%EC%9C%A0%ED%98%95-Strong-Reference-Soft-Reference-Weak-Reference-Phantom-References
- https://d2.naver.com/helloworld/329631
- https://www.baeldung.com/java-weak-reference
- https://jaime-note.tistory.com/461