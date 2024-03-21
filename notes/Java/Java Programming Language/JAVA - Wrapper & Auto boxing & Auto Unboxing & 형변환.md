- - -

## Wrapper 클래스
기본 자료형(Private Type)를 감싸는 클래스

타입 명 맨 앞 글자의 대문자 여부로 구분 가능하다.

- Wrapper <-> 기본 자료형 변환
```java
Byte bt = new Byte((byte) 0);
byte bt1 = bt.byteValue();

Short sho = new Short((short) 10);
short sho1 = sho.shortValue();
```

## Auto boxing & Auto Unboxing
매번 위의 예시처럼 `new` 를 사용하는 건 번거롭고, 코드도 길어진다.

그래서 자바에는 내부적으로 Wrapper와 기본 자료형 간의 변환을 지원한다.

```java
// 기본 자료형 -> 참조 자료형
Double dou = 1.2;
```

```java
//참조 자료형 -> 기본 자료형
Integer integer = 10;

int integer1 = integer;
```

## 명시적 형변환
대부분 Wrapper Class에 static으로 있는 `valueOf()` 또는 `parse~()`를 사용하면 리터럴 변수와 래퍼 클래스끼리 형 변환이 가능하다.
- **{Wrapper Class 이름}.valueOf({변수 이름})** -> Wrapper Class 반환
- **{Wrapper Class 이름}.`parse{Wrapper Class 이름}`({변수 이름})** -> 리터럴 변수 반환


### `String` <-> `char[]`
`char`과 `char[]` 모두 사용 가능한 것만 정리
#### `Char[]` -> `String`
- **valueOf()**
```java
char[] arrCh = {'a', 'b', 'c' }; 
String str = String.valueOf(arrCh);
```
#### `String` ->  `Char[]`
- **toCharArray()**
```java
String str = "ABC";
char[] chs = str.toCharArray();
```

### int <-> String
- **String.valueOf(`temp`)** -> String 반환
- **Integer.toString(`int`)** -> String 반환
- **Integer.parseInt(`Sting`)** -> int 반환
- **Interger.valueOf(`String`)** -> Integer

### Char -> int
- **Character.getNumericValue(`int`)** -> int

### 형변환 추가 자료
![[Archive - 필요없음/old_obsidian/Unlimited Note/ect/문자열과 기본형 사이의 형변환.png]]

## 기본 자료형과 참조 자료형의 성능 차이 비교
기본 자료형과 참조 자료형을 사용하거나 변환하는 연산이 포함된 연산과,
기본 자료형만을 사용한 연산을 비교한다면,
기본 자료형만을 사용한 연산이 더 빠른 성능을 보여준다.

또한, Wrapper Class는 기본 자료형을 객체로 감싸기 때문에 더 많은 메모리를 소비하며, (감싸고 있는) 기본 값을 불러오는 것에도 메모리 탐색을 요구한다. 

# References

```embed
title: '[자바 무료 강의] Wrapper'
image: 'https://www.codelatte.io/meta_image4.jpg'
description: '특수 목적에 의해 만들어진 Wrapper 클래스에 대해서 배우는 강의입니다.'
url: 'https://www.codelatte.io/courses/java_programming_basic/VPIT4LE3T9WP1AAT#head1'
```

모던 자바 인 액션 p.104

```embed
title: '자바 String을 char 배열로 변환 - 제타위키'
image: 'https://storage.googleapis.com/zpub/logo.png'
description: ''
url: 'https://zetawiki.com/wiki/%EC%9E%90%EB%B0%94_String%EC%9D%84_char_%EB%B0%B0%EC%97%B4%EB%A1%9C_%EB%B3%80%ED%99%98'
```

```embed
title: '자바에서 문자열을 int로 바꾸기 - 문자열(string)을 정수(integer)로 바꾸는 법'
image: 'https://www.freecodecamp.org/korean/news/content/images/2022/07/Untitled-design.png'
description: '문자열 객체(object)들은 문자들의 열로 표현된다. 스윙(Java Swing)을 써 본 사람이라면, 거기에 있는 그래픽 사용자 인터페이스에서 입력 값을 가져오기 위해 JTextField나 JTextArea같은 컴포넌트가 쓰이는 것을 봤을 것이다. 이런 컴포넌트들은 우리가 필요한 입력 값을 문자열 형태로 가져온다. 스윙을 이용해서 간단한 계산기를 만든다고 하자. 그럼 문자열(string)을 정수(integer)로 바꾸는 법을 알아야한다. 그러면 물음표가 떠오른다'
url: 'https://www.freecodecamp.org/korean/news/java-string-to-int-how-to-convert-a-string-to-an-integer/'
```

```embed
title: 'Java - Char를 Int로 변환하기'
image: 'https://frhyme.github.io/assets/favicon.ico'
description: 'Java - Char를 Int로 바꾸기'
url: 'https://frhyme.github.io/java/java_basic02_char_to_int/'
```

