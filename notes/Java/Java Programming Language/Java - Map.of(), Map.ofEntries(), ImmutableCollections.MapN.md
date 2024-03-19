
중요한건 아니고 그냥 신기해서 정리함

- ### `Map.of()`
	- `Map<K, V> of(K k1, V v1, K k2, V v2)` 처럼 k,v 10개까지 입력 가능
- ### `Map.ofEntries()`
	- `ofEntries(Entry<? extends K, ? extends V>... entries)` Entry를 받는데, 가변 인자를 사용해 개수 제한없이 입력 가능
- `ImmutableCollections.Map1` or `ImmutableCollections.MapN`
	- 뒤에 1 이랑 N 붙은게 맞다.
	- 불변 Map 객체를 표현하기 위해서 정의한 클래스
	- `UnmodifiableMap.class`이랑은 객체 상속 구조도 좀 다르다.
	- UnmodifiableMap 다이어그램 ![](/notes/Java/files/UnmodifiableMap.png)
	- AbstractImmutableMap 다이어그램 ![AbstractImmutableMap](/notes/Java/files/AbstractImmutableMap.png)

# References
- [Difference Between Map.ofEntries() and Map.of()](https://www.baeldung.com/map-ofentries-and-map-of)
- [openjdk - ImmutableCollections.java](https://github.com/openjdk/jdk/blob/master/src/java.base/share/classes/java/util/ImmutableCollections.java)
- [openjdk - Map.java](https://github.com/openjdk/jdk/blob/master/src/java.base/share/classes/java/util/Map.java)