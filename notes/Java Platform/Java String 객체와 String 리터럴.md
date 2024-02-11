- - -

자바에서 문자열을 선언하는 방법은 String 리터럴과 String 객체 두 가지 방법이 있다.

## 객체 선언 방법

```java
String str1 = new String("hello world"); // 객체 생성
String str2 = "hello world"; // 리터럴 생성
```

두 방법은 메모리 할당 영역이 다르다.

## String 리터럴

> A _string literal_ consists of zero or more characters enclosed in double quotes.
> (번역) _문자열 리터럴_은 큰 따옴표로 둘러싸인 0자 이상의 문자로 구성됩니다.

> A string literal is a reference to an instance of class `String` ([§4.3.1](https://papago.naver.net/apis/site/proxy?url=https%3A%2F%2Fdocs.oracle.com%2Fjavase%2Fspecs%2Fjls%2Fse11%2Fhtml%2Fjls-4.html%23jls-4.3.1 "4.3.1. Objects"), [§4.3.3](https://papago.naver.net/apis/site/proxy?url=https%3A%2F%2Fdocs.oracle.com%2Fjavase%2Fspecs%2Fjls%2Fse11%2Fhtml%2Fjls-4.html%23jls-4.3.3 "4.3.3. The Class String")).
> 
> Moreover, a string literal always refers to the _same_ instance of class `String`. This is because string literals - or, more generally, strings that are the values of constant expressions ([§15.28](https://papago.naver.net/apis/site/proxy?url=https%3A%2F%2Fdocs.oracle.com%2Fjavase%2Fspecs%2Fjls%2Fse11%2Fhtml%2Fjls-15.html%23jls-15.28 "15.28. Constant Expressions")) - are "interned" so as to share unique instances, using the method `String.intern` ([§12.5](https://papago.naver.net/apis/site/proxy?url=https%3A%2F%2Fdocs.oracle.com%2Fjavase%2Fspecs%2Fjls%2Fse11%2Fhtml%2Fjls-12.html%23jls-12.5 "12.5. Creation of New Class Instances")).
> 
> (번역) 
> 문자열 리터럴은 클래스 인스턴스에 대한 참조입니다 `String` ( [§4.3.1](https://docs.oracle.com/javase/specs/jls/se11/html/jls-4.html#jls-4.3.1 "4.3.1. 사물") , [§4.3.3](https://docs.oracle.com/javase/specs/jls/se11/html/jls-4.html#jls-4.3.3 "4.3.3. 클래스 문자열") ).
> 
> 게다가 문자열 리터럴은 항상 class 의 _동일한_`String` 인스턴스를 참조합니다 . 이는 문자열 리터럴(더 일반적으로는 상수 표현식의 값인 문자열( [§15.28](https://docs.oracle.com/javase/specs/jls/se11/html/jls-15.html#jls-15.28 "15.28. 상수 표현식") ))이 메서드 `String.intern` ( [§12.5](https://docs.oracle.com/javase/specs/jls/se11/html/jls-12.html#jls-12.5 "12.5. 새로운 클래스 인스턴스 생성") )를 사용하여 고유한 인스턴스를 공유하도록 "인턴"되기 때문입니다.


다음과 같은 특징을 가진다.
- Heap영역 내부의 String Constant Pool에 저장된다.
	- JLS, JavaDoc에서는 pool 이라고 부를 뿐, 실제 공식 명칭인지는 확실하지 않다.
- 한 String 리터럴은 여러 객체에 참조 될 수 있다. (String 리터럴은 불변이므로 수정 문제에서 자유롭다.)
- 내부적으로 String Constant Pool에 저장하는 `String#intern`을 사용했을 뿐 String객체와 동일히다.
- 같은 값을 가지는 String 리터럴은 같은 객체이므로 `==`를 통과한다.

## String 객체

String 객체는 `new` 키워드를 사용해 String을 생성하면 생긴다.   
(외부에서 받아서 String을 생성하는 경우도 String 객체로 생성된다. ex:Main 메서드 파라미터, I/O)

Heap 영역애 저장되며, 같은 값을 가지더라도 다른 객체이므로 `==` 가 통하지 않는다.

## `String.intern()`
`String.intern()` 을 사용해 String 객체를 String Constant Pool에 저장할 수 있다.  

사용하면 문자열을 저장하고 객체의 참조를 반환한다.
이미 저장된 문자열이면 저장 된 객체의 참조를 반환한다.


# 기타 자료

### OpenJDK 11 `String#intern` 코드

```java
    /**
     * Returns a canonical representation for the string object.
     * <p>
     * A pool of strings, initially empty, is maintained privately by the
     * class {@code String}.
     * <p>
     * When the intern method is invoked, if the pool already contains a
     * string equal to this {@code String} object as determined by
     * the {@link #equals(Object)} method, then the string from the pool is
     * returned. Otherwise, this {@code String} object is added to the
     * pool and a reference to this {@code String} object is returned.
     * <p>
     * It follows that for any two strings {@code s} and {@code t},
     * {@code s.intern() == t.intern()} is {@code true}
     * if and only if {@code s.equals(t)} is {@code true}.
     * <p>
     * All literal strings and string-valued constant expressions are
     * interned. String literals are defined in section 3.10.5 of the
     * <cite>The Java&trade; Language Specification</cite>.
     *
     * @return  a string that has the same contents as this string, but is
     *          guaranteed to be from a pool of unique strings.
     * @jls 3.10.5 String Literals
     */
    public native String intern();
```

```java
/**
 * 문자열 객체에 대한 규범적 표현을 반환합니다.
 * <p>
 * {@code String} 클래스에 의해 비공개로 유지되는 초기에는 빈 문자열 풀이 유지됩니다.
 * <p>
 * intern 메서드가 호출되면, 풀이 이미 이 {@code String} 객체와 동일한 문자열을 포함하고 있는 경우
 * {@link #equals(Object)} 메서드에 의해 결정된대로, 풀에서 해당 문자열이 반환됩니다.
 * 그렇지 않으면, 이 {@code String} 객체가 풀에 추가되고, 이 {@code String} 객체에 대한 참조가 반환됩니다.
 * <p>
 * 따라서 모든 두 문자열 {@code s}와 {@code t}에 대해,
 * {@code s.intern() == t.intern()}이 {@code true}인 경우에만 {@code s.equals(t)}이 {@code true}입니다.
 * <p>
 * 모든 리터럴 문자열과 문자열 값 상수 표현식이 인터닝됩니다. 문자열 리터럴은
 * <cite>Java&trade; 언어 명세서</cite>의 섹션 3.10.5에서 정의됩니다.
 *
 * @return  이 문자열과 동일한 내용을 가진 문자열이지만 고유한 문자열 풀에서 보장됩니다.
 * @jls 3.10.5 문자열 리터럴
 */
public native String intern();
```

## Spring Constant Pool 의 버전 별 변화

아래는 [자바의 String 객체와 String 리터럴](https://madplay.github.io/post/java-string-literal-vs-string-object) 내용의 일부이다.

상수풀(String Constant Pool)의 위치는 `Java 7`부터 `Perm` 영역에서 `Heap` 영역으로 옮겨졌다. Perm 영역은 실행 시간(Runtime)에 가변적으로 변경할 수 없는 고정된 사이즈이기 때문에 intern 메서드의 호출은 저장할 공간이 부족하게 만들 수 있었다. 즉 OOM(Out Of Memory) 문제가 발생할 수 있는 위험이 있었던 것이다.

Heap 영역으로 변경된 이후에는 상수풀에 들어간 문자열도 Garbage Collection 대상이 된다.

[관련 링크) JDK-6962931 : move interned strings out of the perm gen(Oracle Java Bug Database)](https://bugs.java.com/view_bug.do?bug_id=6962931)

`Java 7`버전에서 상수풀의 위치가 Perm 영역(정확히 풀어서 써보면 Permanent Generation)에서 Heap으로 옮겨지고 이후에 `Java 8` 버전에서는 Perm 영역은 완전히 사라지고 이를 MetaSpace라는 영역이 대신하고 있다.

# Reference
- [Java BigDecimal 및 BigInteger](https://dejavuhyo.github.io/posts/java-bigdecimal-biginteger/)
- [자바의 String 객체와 String 리터럴](https://madplay.github.io/post/java-string-literal-vs-string-object)
- [Github - OpenJDK 11](https://github.com/AdoptOpenJDK/openjdk-jdk11/blob/master/src/java.base/share/classes/java/lang/String.java)
- [Oracle Java SE 11 JLS 3.10.5](https://docs.oracle.com/javase/specs/jls/se11/html/jls-3.html#jls-3.10.4)