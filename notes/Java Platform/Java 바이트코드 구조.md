- - -
### 글의 목적
Java 바이트코드를 가볍게 이해하는 것  

[Java 바이트코드 구조](notes/Java%20Platform/Java%20바이트코드%20구조.md)에서 설명하는 바이트코드를 이해하는 걸 목적으로 작성하였다.  
자세한 형식을 알아야 할 필요는 없으므로 생략된 부분이 많다.

### Java 바이트코드 보는법
컴파일러(`javac`)를 사용해 바이트코드로 컴파일 할 수 있다.

이러한 컴파일 된 바이트코드는 **바이너리 파일 형식**이다. 그래서 사람이 읽기 어렵다.

컴파일 된 바이트코드를 역어셈블러(`javap`)를 사용해서 디컴파일이 가능하다.  
-> 자세한 `javap` 사용법은 [Oracle 문서](https://docs.oracle.com/en/java/javase/21/docs/specs/man/javap.html) 참고

디컴파일된 바이트코드는 사람이 읽기 좋게 변환되어 있다.


### Java 바이트코드 예시

```java
package homo.efficio.jvm.sample;  
  
public interface Greeting {  
  
    String sayHello(String name);  
}
```

> javap -v -l -p homo/efficio/jvm/sample/Greeting.class

상수 풀(Constant pool) 일부 텍스트는 특정 옵션을 사용해야 볼 수 있다.

```
Classfile /C:/gitrepo/scratchpad/plain-java-scratchpad/out/production/classes/homo/efficio/jvm/sample/Greeting.class  
  Last modified 2019. 1. 13.; size 181 bytes  
  MD5 checksum 8f7ae541e0a64f511d820930f739d4ac  
  Compiled from "Greeting.java"  
public interface homo.efficio.jvm.sample.Greeting  
  minor version: 0  
  major version: 53  
  flags: (0x0601) ACC_PUBLIC, ACC_INTERFACE, ACC_ABSTRACT  
  this_class: #1                          // homo/efficio/jvm/sample/Greeting  
  super_class: #2                         // java/lang/Object  
  interfaces: 0, fields: 0, methods: 1, attributes: 1  
Constant pool:  
  #1 = Class              #7              // homo/efficio/jvm/sample/Greeting  
  #2 = Class              #8              // java/lang/Object  
  #3 = Utf8               sayHello  
  #4 = Utf8               (Ljava/lang/String;)Ljava/lang/String;  
  #5 = Utf8               SourceFile  
  #6 = Utf8               Greeting.java  
  #7 = Utf8               homo/efficio/jvm/sample/Greeting  
  #8 = Utf8               java/lang/Object  
{  
  public abstract java.lang.String sayHello(java.lang.String);  
    descriptor: (Ljava/lang/String;)Ljava/lang/String;  
    flags: (0x0401) ACC_PUBLIC, ACC_ABSTRACT  
}  
SourceFile: "Greeting.java"
```

### 구성요소 설명
구체적인 정보는 공식문서 참고

#### 파일 및 클래스 구성 

1. **Magic Number:** 클래스 파일이 자바 클래스 파일임을 나타내는 일련의 숫자(`0xCAFEBABE`). 이는 4바이트로 구성되며, 항상 클래스 파일의 시작 부분에 위치한다.
2. **Minor Version, Major Version**: 클래스 파일이 작성된 자바 버전을 나타낸다.
3. **Constant Pool:** 클래스 및 인터페이스의 중요한 상수들을 저장하는 테이블. 
	- 클래스 파일 내의 다양한 구성 요소에서 사용한다. 
	- 여러 유형이 존재한다.
		- Utf8: 상수 값이 선언되어 있다, 나미저 모든 타입은 Utf8를 참조한다.
		- Class, NameAndType, Methodref, String 등...
	- 해당 상수 풀을 사용하여 링크
4. **Access Flags:** 클래스 또는 인터페이스의 접근 제어 정보. 클래스나 인터페이스의 속성.
5. **This Class:** 클래스 또는 인터페이스의 이름을 가리키는 인덱스.
6. **Super Class:** 상위 클래스의 이름을 가리키는 인덱스.
7. **Interfaces:** 인터페이스들의 리스트.
8. **Fields:** 클래스 또는 인터페이스에 선언된 필드들.
9. **Methods:** 클래스 또는 인터페이스에 선언된 메서드들.
10. **Attributes:** 클래스 파일 및 클래스 파일 내의 구성 요소에 대한 추가 정보.

#### Constant Pool
해시(#)는 참조하고 있는 Constant Pool의 인덱스를 표시한다.

클래스의 이름, 필드 및 메서드의 심볼릭 참조, 리터럴 값 등을 포함한다.
Constant Pool은 클래스 파일의 로드 시점에 메모리에 로드되며, 클래스 파일에 포함된 모든 상수 값들을 저장한다.


#### {} 내부 구성

아래와 같은 구조를 가진다.

- **필드나 메서드 선언부** - ex: `public void add(java.lang.String);`
    - **descriptor**: 필드의 타입이나 메서드의 파라미터 및 반환 타입
    - **flags**: 접근 지정자
    - **Code**
        - stack, locals, args_size: 스택 높이, 로컬 변수 갯수, 인자 갯수
            - 실제 구현 코드: 코드 위치, 바이트코드 명령어(instruction), 오퍼랜드(operand, 피연산자)
        - **LineNumberTable**: 자바 코드의 행 번호와 바이트코드의 위치 매핑 테이블
        - **LocalVariableTable**: 로컬 변수 테이블

#### 바이트코드 명령어

`invoke~`는 전부 결과를 operand stack에 올린다.

| 명령어 이름 | 하는 일 |
| ---- | ---- |
| invokeinterface | 인터페이스에 정의된 메서드 호출 |
| invokespecial | 생성자 호출이나 현재 클래스의 private 메서드, 수퍼클래스의 메서드를 호출 |
| invokestatic | 정적 메서드 호출 |
| invokevirtual | 자바 메서드 호출의 기본 방식이며, 객체 참조(`obj.`)를 붙여서 호출되는 일반적인 메서드 호출 |
| invokedynamic | JVM에서 실행되는 동적 타입 언어를 위해 Java 7에 추가된 명령어. 람다식도 invokedynamic을 이용해서 구현되었다. 자세한 내용은 [오라클 문서](https://docs.oracle.com/javase/8/docs/technotes/guides/vm/multiple-language-support.html)나 [네이버 문서](https://d2.naver.com/helloworld/4911107) 또는 [DZone 문서](https://dzone.com/articles/dismantling-invokedynamic)를 참고하자. |
| aload | 특정 local variables를 operand stack에 push.<br>ex: `aload_0`은 constant pool `#0`을 operand stack에 로드 |
| ldc | constant pool에서부터 `#index` 에 해당하는 데이터를 가져옴 |
| astore | 특정 operand stack의 값이나 참조를 local variables에 값을 저장 |
| new | 새로운 객체를 생성 |
| dup | operand stack top의 값이나 참조를 복사하여 operand stack에 push |
| return | 메서드의 반환 값을 처리. 반환 값이 있을 경우 operand stack에서 값을 꺼내어 반환, 반환 값이 없는 경우 void를 반환 |

### 더 구체적인 바이트코드 예시

```java
package homo.efficio.jvm.sample;  
  
public class KoreanGreeting implements Greeting {  
  
    private String hello = "안녕 ";  
  
    @Override  
    public String sayHello(String name) {  
        return getHello() + name;  
    }  
  
    private String getHello() {  
        return this.hello;  
    }  
}
```

> javap -v -l -p homo/efficio/jvm/sample/KoreanGreeting.class

```
Classfile /C:/gitrepo/scratchpad/plain-java-scratchpad/out/production/classes/homo/efficio/jvm/sample/KoreanGreeting.class  
  Last modified 2019. 1. 12.; size 1132 bytes  
  MD5 checksum d7ac2a6fd38c67407480720ca730d987  
  Compiled from "KoreanGreeting.java"  
public class homo.efficio.jvm.sample.KoreanGreeting implements homo.efficio.jvm.sample.Greeting  
  minor version: 0  
  major version: 53  
  flags: (0x0021) ACC_PUBLIC, ACC_SUPER  
  this_class: #6                          // homo/efficio/jvm/sample/KoreanGreeting  
  super_class: #7                         // java/lang/Object  
  interfaces: 1, fields: 1, methods: 3, attributes: 3  
Constant pool:  
   #1 = Methodref          #7.#25         // java/lang/Object."<init>":()V  
   #2 = String             #26            // ▒?▒  
   #3 = Fieldref           #6.#27         // homo/efficio/jvm/sample/KoreanGreeting.hello:Ljava/lang/String;  
   #4 = Methodref          #6.#28         // homo/efficio/jvm/sample/KoreanGreeting.getHello:()Ljava/lang/String;  
   #5 = InvokeDynamic      #0:#32         // #0:makeConcatWithConstants:(Ljava/lang/String;Ljava/lang/String;)Ljava/lang/String;  
   #6 = Class              #33            // homo/efficio/jvm/sample/KoreanGreeting  
   #7 = Class              #34            // java/lang/Object  
   #8 = Class              #35            // homo/efficio/jvm/sample/Greeting  
   #9 = Utf8               hello  
  #10 = Utf8               Ljava/lang/String;  
  #11 = Utf8               <init>  
  #12 = Utf8               ()V  
  #13 = Utf8               Code  
  #14 = Utf8               LineNumberTable  
  #15 = Utf8               LocalVariableTable  
  #16 = Utf8               this  
  #17 = Utf8               Lhomo/efficio/jvm/sample/KoreanGreeting;  
  #18 = Utf8               sayHello  
  #19 = Utf8               (Ljava/lang/String;)Ljava/lang/String;  
  #20 = Utf8               name  
  #21 = Utf8               getHello  
  #22 = Utf8               ()Ljava/lang/String;  
  #23 = Utf8               SourceFile  
  #24 = Utf8               KoreanGreeting.java  
  #25 = NameAndType        #11:#12        // "<init>":()V  
  #26 = Utf8               ▒?▒  
  #27 = NameAndType        #9:#10         // hello:Ljava/lang/String;  
  #28 = NameAndType        #21:#22        // getHello:()Ljava/lang/String;  
  #29 = Utf8               BootstrapMethods  
  #30 = MethodHandle       6:#36          // REF_invokeStatic java/lang/invoke/StringConcatFactory.makeConcatWithConstants:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/String;[Ljava/lang/Object;)Ljava/lang/invoke/CallSite;  
  #31 = String             #37            // \u0001\u0001  
  #32 = NameAndType        #38:#39        // makeConcatWithConstants:(Ljava/lang/String;Ljava/lang/String;)Ljava/lang/String;  
  #33 = Utf8               homo/efficio/jvm/sample/KoreanGreeting  
  #34 = Utf8               java/lang/Object  
  #35 = Utf8               homo/efficio/jvm/sample/Greeting  
  #36 = Methodref          #40.#41        // java/lang/invoke/StringConcatFactory.makeConcatWithConstants:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/String;[Ljava/lang/Object;)Ljava/lang/invoke/CallSite;  
  #37 = Utf8               \u0001\u0001  
  #38 = Utf8               makeConcatWithConstants  
  #39 = Utf8               (Ljava/lang/String;Ljava/lang/String;)Ljava/lang/String;  
  #40 = Class              #42            // java/lang/invoke/StringConcatFactory  
  #41 = NameAndType        #38:#46        // makeConcatWithConstants:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/String;[Ljava/lang/Object;)Ljava/lang/invoke/CallSite;  
  #42 = Utf8               java/lang/invoke/StringConcatFactory  
  #43 = Class              #48            // java/lang/invoke/MethodHandles$Lookup  
  #44 = Utf8               Lookup  
  #45 = Utf8               InnerClasses  
  #46 = Utf8               (Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/String;[Ljava/lang/Object;)Ljava/lang/invoke/CallSite;  
  #47 = Class              #49            // java/lang/invoke/MethodHandles  
  #48 = Utf8               java/lang/invoke/MethodHandles$Lookup  
  #49 = Utf8               java/lang/invoke/MethodHandles  
{  
  private java.lang.String hello;  
    descriptor: Ljava/lang/String;  
    flags: (0x0002) ACC_PRIVATE  
  
  public homo.efficio.jvm.sample.KoreanGreeting();  
    descriptor: ()V  
    flags: (0x0001) ACC_PUBLIC  
    Code:  
      stack=2, locals=1, args_size=1  
         0: aload_0  
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V  
         4: aload_0  
         5: ldc           #2                  // String ▒?▒  
         7: putfield      #3                  // Field hello:Ljava/lang/String;  
        10: return  
      LineNumberTable:  
        line 3: 0  
        line 5: 4  
      LocalVariableTable:  
        Start  Length  Slot  Name   Signature  
            0      11     0  this   Lhomo/efficio/jvm/sample/KoreanGreeting;  
  
  public java.lang.String sayHello(java.lang.String);  
    descriptor: (Ljava/lang/String;)Ljava/lang/String;  
    flags: (0x0001) ACC_PUBLIC  
    Code:  
      stack=2, locals=2, args_size=2  
         0: aload_0  
         1: invokespecial #4                  // Method getHello:()Ljava/lang/String;  
         4: aload_1  
         5: invokedynamic #5,  0              // InvokeDynamic #0:makeConcatWithConstants:(Ljava/lang/String;Ljava/lang/String;)Ljava/lang/String;  
        10: areturn  
      LineNumberTable:  
        line 9: 0  
      LocalVariableTable:  
        Start  Length  Slot  Name   Signature  
            0      11     0  this   Lhomo/efficio/jvm/sample/KoreanGreeting;  
            0      11     1  name   Ljava/lang/String;  
  
  private java.lang.String getHello();  
    descriptor: ()Ljava/lang/String;  
    flags: (0x0002) ACC_PRIVATE  
    Code:  
      stack=1, locals=1, args_size=1  
         0: aload_0  
         1: getfield      #3                  // Field hello:Ljava/lang/String;  
         4: areturn  
      LineNumberTable:  
        line 13: 0  
      LocalVariableTable:  
        Start  Length  Slot  Name   Signature  
            0       5     0  this   Lhomo/efficio/jvm/sample/KoreanGreeting;  
}  
SourceFile: "KoreanGreeting.java"  
InnerClasses:  
  public static final #44= #43 of #47;    // Lookup=class java/lang/invoke/MethodHandles$Lookup of class java/lang/invoke/MethodHandles  
BootstrapMethods:  
  0: #30 REF_invokeStatic java/lang/invoke/StringConcatFactory.makeConcatWithConstants:(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/String;[Ljava/lang/Object;)Ljava/lang/invoke/CallSite;  
    Method arguments:  
      #31 \u0001\u0001
```

# Reference
- [Oracle - Java SE 8 Spec](https://docs.oracle.com/javase/specs/jvms/se8/html/)
- [Oracle - The javap Command](https://docs.oracle.com/en/java/javase/21/docs/specs/man/javap.html)
- [Oracle Blog - Understanding the constant pool inside a Java class file](https://blogs.oracle.com/javamagazine/post/java-class-file-constant-pool?source=:em:nw:mt)
- [블로그 - Back to the Essence - Java 컴파일에서 실행까지](https://homoefficio.github.io/2019/01/31/Back-to-the-Essence-Java-%EC%BB%B4%ED%8C%8C%EC%9D%BC%EC%97%90%EC%84%9C-%EC%8B%A4%ED%96%89%EA%B9%8C%EC%A7%80-1/)
- [블로그 - JVM stack과 frame](https://johngrib.github.io/wiki/jvm-stack/)