- - -
![Java-Collections](https://upload.wikimedia.org/wikipedia/commons/thumb/a/ab/Java.util.Collection_hierarchy.svg/2880px-Java.util.Collection_hierarchy.svg.png)
## Iterable 인터페이스
`java.lang.Iterable`

`List`, `Set`, `Queue` ⇒ `Collection` ⇒ `Iterable

- method
	- `Iterator<T>` `iterator()`
	- `default` `void` `forEach(Consumer<? super T> action)` 
	- `default` `Spliterator<T>` `spliterator()`
- https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html

- `Iterable`의 `foreach()`는 `for-each-loop`를 사용한다.
-  `for-each`는 `collection`을 가져오는 것만 가능하고 loop 도중 수정은 불가능하다.
- `for-each`는 가독성이 좋기 때문에 단순 읽기 용도라면 `for-each`를 사용하는 것이 좋다.


Iterable의 구현체들은 `forEach()` - `for (String str : list)`, `iterator()`를 사용할 수 있음을 보장

## Iterator 인터페이스

Collection과는 별개로 존재
`java.util.Iterator`

Collection을 Iterator로 만들어서 반복문을 사용할 수 있다.
 
Iterator의 내부 동작 방식 - https://brandpark.github.io/java/2021/01/24/iterator.html

- `Collect<T>`은 `Iterable<T>`을 구현한다.
- `Iterable#iterator` 사용 -> `Iterator<E>` 반환
- `Iterator<E>`을 사용해 loop 동작

- method 
	- `default` `void` `forEachRemaining(Consumer<? super E> action)`
	- `boolean` `hasNext()`
	- `E` `next()`
	- `default` `void` `remove()` - 마지막으로 반환한 element를 제거한다.
- https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html

정의 방법 : `Iterator<T> iterator = Collection.iterator();

예시
- `Collect<T>`은 `Iterable<T>`에 의해 `iterator()` 구현을 강제받는다.
- `iterator()` 호출 -> `Iterator<E>` 반환 
- `Iterator<E>`를 사용해서 loop
```java
Iterator<String> iterator = cars.iterator();  //cars = HashSet 

// while문을 사용한 경우
while(iterator.hasNext()) { 
	System.out.println("cars : " + iterator.next()); 
} 
// for-each문을 사용한 경우 
for (String car : cars) { 
	System.out.println("cars : " + car); 
	}
```

#### 고전적인 반복문(C Style)과의 차이
```java
for(int i=0; i<list.size(); i++) {
   Object o = list.get(i);
}

for (Iterator i=list.iterator(); i.hasNext(); )
         i.next();
```

## 번외

stackoverflow에서 C Style과 for-each의 성능에 대한 이야기가 계속되는걸 기록했었는데, 정리가 잘 안되어 있고, C Style과 for-each간의 성능차이가 전체 성능에 영향을 주지 못할꺼라 생각해서 지웠다.

# References

```embed
title: 'Iterable (Java Platform SE 8 )'
image: 'https://docs.oracle.com/favicon.ico'
description: 'Implementing this interface allows an object to be the target of the “for-each loop” statement. See For-each Loop'
url: 'https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html'
```

```embed
title: 'Iterator (Java Platform SE 8 )'
image: 'https://docs.oracle.com/favicon.ico'
description: 'An iterator over a collection. Iterator takes the place of Enumeration in the Java Collections Framework. Iterators differ from enumerations in two ways:'
url: 'https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html'
```

```embed
title: '[ JAVA ] Iterator의 내부동작 - 자바니또의 Tech선물'
image: 'https://brandpark.github.io/img/content-img/java/iterator1.png'
description: '개요'
url: 'https://brandpark.github.io/java/2021/01/24/iterator.html'
```

```embed
title: 'RandomAccess (Java Platform SE 8 )'
image: 'https://docs.oracle.com/favicon.ico'
description: 'Marker interface used by'
url: 'https://docs.oracle.com/javase/8/docs/api/java/util/RandomAccess.html'
```


