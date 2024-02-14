- - -

## 클래스 로드란?
JVM은 필요한 객체를 미리 올려놓지 않고, 런타임 시점에 동적으로 로드하여 사용한다.   
ClassLoader를 사용해서 객체를 로드한다.

JVM은 자바 애플리케이션을 클래스 로더(Class Loader)를 통해 읽어 들여서 자바 API와 함께 실행하는 역할을 가진다.  

(정확하지 않음) WORA(Write Once Run Anywhere)를 구현하기 위해서 이러한 방법을 채택하였다.   
(하드웨어와 가상머신을 분리하기 위해서)

## ClassLoader 종류

![class_loader](notes/Java%20Platform/files/class_loader.png)

- 부트스트랩 클래스 로더(Bootstrap Class Loader): JVM을 실행하기 위한 핵심 자바 API를 로드한다. 네이티브 코드로 구현되어 있고, parent를 가지지 않으며 null로 표현된다.
    - Java 9 이전
        - `rt.jar`에 있는 JVM을 실행하기 위한 핵심 자바 API를 로드한다.
    - Java 9 부터
        - `rt.jar`가 모듈화 되어 작은 단위로 나뉘면서 `java.base`와 같은 중요한 모듈의 클래스 로딩만 다루게 역할이 축소되었다. (필요한 일부 API 만 로드 할 수 있다.)
        - 기존에 기본 자바 API를 위해서 Bootstrap Class Loader를 사용한 경우, Child를 사용하도록 변경해야 한다. 기존 자바 API가 포함되지 않을 수 있기 때문
- 익스텐션 클래스 로더(Extension Class Loader): 기본 자바 API를 제외한 확장 클래스들을 로드한다. 다양한 보안 확장 기능 등을 여기에서 로드하게 된다.
    - Java 9 이전
        - `URLClassLoader`를 상속한다.
        - `jre/lib/ext` 내의 클래스를 로드한다.
    - Java 9 부터
        - Platform ClassLoader로 이름이 변경되었다.
        - Java SE의 모든 클래스를 포함한다.
            - 일부 클래스는 부트스트랩에서 선언되나, 모든 Java SE가 포함되도록 보장하는 건 플랫폼 클래스로더이다.
        - `BuiltinClassLoader`를 상속한다. Inner Static 클래스로 선언된다.
        - [ClassLoaders.java](https://github.com/AdoptOpenJDK/openjdk-jdk11/blob/master/src/java.base/share/classes/jdk/internal/loader/ClassLoaders.java) 참고
- 시스템 클래스 로더(System Class Loader): 사용자가 지정한 `$CLASSPATH` 내의 클래스들을 로드한다. Application Class Loader라고 부르기도 한다.
    - Java 9 이전
        - `URLClassLoader`를 상속한다.
    - Java 9 부터
        - `BuiltinClassLoader`를 상속한다. Inner Static 클래스로 선언된다.
- 사용자 정의 클래스 로더(User-Defined Class Loader): 애플리케이션 사용자가 직접 코드 상에서 생성해서 사용하는 클래스 로더이다.
    - 자바 API로 제공하는 `URLClassLoader`를 사용하거나, 직접 구현하여 사용한다.
        - 외부의 `.jar`(객체)를 읽을 때 사용한다. - [Java SE 8 APIDoc](https://docs.oracle.com/javase/8/docs/api/java/net/URLClassLoader.html)
        - ex: Spring이 jar을 실행할 때 사용한다. - [공식문서](https://docs.spring.io/spring-boot/docs/current/reference/html/executable-jar.html), [설명 잘된 글](https://velog.io/@qudtjs0753/%EC%8A%A4%ED%94%84%EB%A7%81%EC%9D%B4-%EC%99%B8%EB%B6%80-jar%EB%A5%BC-%EB%B6%88%EB%9F%AC%EC%98%A4%EB%8A%94-%EB%B0%A9%EB%B2%95)

## ClassLoad 과정

- 로드: 클래스를 파일에서 가져와서 JVM의 메모리에 로드한다.
	- 로드하려는 객체에 따라서 다른 클래스 로더가 사용된다.
- 링킹: 사용하려는 객체를 검증하고 준비한다. 
	- 다음 규칙을 지키는 한 자유롭게 로드 시점을 결정할 수 있다.
		- 클래스나 인터페이스는 링킹되기 전에 완전히 로드된다.
		- 클래스나 인터페이스는 초기화되기 전에 완전히 검증되고 준비된다
	- 3가지 하위 단계로 나뉜다.
		- 검증(Verifying): 읽어 들인 클래스가 자바 언어 명세(Java Language Specification) 및 JVM 명세에 명시된 대로 잘 구성되어 있는지 확인한다.
		- 준비(Preparing): 클래스가 필요로 하는 메모리를 할당하고, 클래스에서 정의된 필드, 메서드, 인터페이스들을 나타내는 데이터 구조를 준비한다. (메모리 할당 + static 필드를 생성하고 기본값(실제 값이 아니다.)으로 설정하는 과정)
		- 해석(Resolving): 클래스의 상수 풀 내 모든 심볼릭 레퍼런스를 다이렉트 레퍼런스로 변경한다.
			- 지연 링크를 사용하면 해석 과정은 클래스나 인터페이스가 초기화 된 후에 실행될 수도 있다.
- 초기화: 클래스 변수들을 적절한 값으로 초기화한다. 즉, static initializer(static 블록)들을 수행하고, static 필드들을 설정된 값으로 초기화한다.

### 클래스 로드 과정 시작 시점

정확한 설명은 공식문서를 참고하자.

#### 로드

> Creation of a class or interface C denoted by the name `N` consists of the construction in the method area of the Java Virtual Machine ([§2.5.4](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.5.4 "2.5.4. Method Area")) of an implementation-specific internal representation of C. Class or interface creation is triggered by another class or interface D, which references C through its run-time constant pool. Class or interface creation may also be triggered by D invoking methods in certain Java SE platform class libraries ([§2.12](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.12 "2.12. Class Libraries")) such as reflection.
> 클래스 또는 인터페이스 C의 생성은 Java 가상 머신(§2.5.4)의 메서드 영역에 C의 구현별 내부 표현의 구성으로 이루어집니다. 클래스 또는 인터페이스 생성은 다른 클래스 또는 인터페이스 D에 의해 트리거됩니다. 이때 D는 런타임 상수 풀을 통해 C를 참조합니다. 클래스 또는 인터페이스 생성은 또한 D가 리플렉션과 같은 특정 Java SE 플랫폼 클래스 라이브러리(§2.12)의 메서드를 호출함으로써 트리거될 수 있습니다.

다른 객체에서 런타임 상수 풀에서 객체를 참조하거나, 리플렉션과 같은 Java SE에서 제공하는 클래스 관련 라이브러리 사용 시 로드가 시작된다.

다른 JVM Stack의 프레임에서 객체를 사용하려고 할 때, Dynamic linking(동적 연결)을 한다. 이 때, 클래스가 로드되지 않은 경우 사용한다.  

(아마 정확한 시점은 어떤 메서드에서 로드하려는 클래스를 사용하는 시점인 것 같다. 아래 테스트 항목 참고)

#### 링킹

몇 규칙을 지키는 한 자유롭게 로드 시점을 결정할 수 있다. (이외에도 규칙이 더 있다, 자세한 건 공식문서 참고) 
- 클래스나 인터페이스는 링킹되기 전에 완전히 로드된다.
- 클래스나 인터페이스는 초기화되기 전에 완전히 검증되고 준비된다

#### 초기화

> A class or interface type T will be initialized immediately before the first occurrence of any one of the following:
> - T is a class and an instance of T is created.
> - A `static` method declared by T is invoked.
> - A `static` field declared by T is assigned.
> - A `static` field declared by T is used and the field is not a constant variable ([§4.12.4](https://docs.oracle.com/javase/specs/jls/se8/html/jls-4.html#jls-4.12.4 "4.12.4. final Variables")).
> - T is a top level class ([§7.6](https://docs.oracle.com/javase/specs/jls/se8/html/jls-7.html#jls-7.6 "7.6. Top Level Type Declarations")) and an `assert` statement ([§14.10](https://docs.oracle.com/javase/specs/jls/se8/html/jls-14.html#jls-14.10 "14.10. The assert Statement")) lexically nested within T ([§8.1.3](https://docs.oracle.com/javase/specs/jls/se8/html/jls-8.html#jls-8.1.3 "8.1.3. Inner Classes and Enclosing Instances")) is executed.

클래스 또는 인터페이스 유형 T는 다음 중 하나가 처음 발생하기 **직전**에 초기화된다.
- T 는 클래스이고 T 의 인스턴스가 생성된다.
- T`static` 가 선언한 메서드가 호출된다.
- T`static` 가 선언한 필드 가 할당된다.
- T`static` 에 의해 선언된 필드가 사용되고 필드가 상수 변수가 아니다( [§4.12.4](https://docs.oracle.com/javase/specs/jls/se8/html/jls-4.html#jls-4.12.4 "4.12.4. 최종 변수") ).
	- 기본형 타입 `staic final`인 경우, 컴파일 시점에 값이 고정되어 있으므로 해당 값을 사용하는 클래스에서 값을 복사한다. (자세한 내용은 아래 따로 정리)
- T 는 최상위 클래스( [§7.6](https://docs.oracle.com/javase/specs/jls/se8/html/jls-7.html#jls-7.6 "7.6. 최상위 유형 선언") )이며 T ( [§8.1.3](https://docs.oracle.com/javase/specs/jls/se8/html/jls-8.html#jls-8.1.3 "8.1.3. 내부 클래스 및 둘러싸는 인스턴스") ) 내에 어휘적으로 중첩된 `assert`명령문( [§14.10](https://docs.oracle.com/javase/specs/jls/se8/html/jls-14.html#jls-14.10 "14.10. 주장문") )이 실행된다.
	- Nested Class가 아니면 최상위 클래스이다.

## 테스트

### Static Final 테스트

```java
public class Main {
  public static void main(String[] args) {
    System.out.println(Outer.VALUE); // 정적 final 상수 호출

    System.out.println(Outer.value); // 정적 상수 호출
  }
}

class Outer {

  // static 변수
  static String value = "> Outer 클래스의 static 필드 입니다.";

  // static 블록
  static {
    System.out.println("> Outer Static 블록");
  }

  // static final 상수
  static final String VALUE = "> Outer 클래스의 static final 필드 입니다.";

  Outer() {
    System.out.println("> Outer 생성자 초기화");
  }

  // static 메서드
  static void getInstance() {
    System.out.println("> Outer 클래스의 static 메서드 호출");
  }

  // inner 클래스
  class Inner {
    Inner() {
      System.out.println("> Inner 생성자 초기화");
    }
  }

  // static inner 클래스
  static class Holder {
    static String value = "> Holder 클래스의 static 필드 입니다.";
    static final String VALUE = "> Holder 클래스의 static final 필드 입니다.";

    Holder() {
      System.out.println("> Holder 생성자 초기화");
    }
  }
}
```

```
~/ClassLoad$ javap -v -l -p Main.class 
Classfile /home/runner/ClassLoad/Main.class
  Last modified Feb 8, 2024; size 516 bytes
  SHA-256 checksum 686fb60f624e5d85137c81092ddd56621d036a8ce156af9de6650e297c5dfef2
  Compiled from "Main.java"
public class Main
  minor version: 0
  major version: 61
  flags: (0x0021) ACC_PUBLIC, ACC_SUPER
  this_class: #27                         // Main
  super_class: #2                         // java/lang/Object
  interfaces: 0, fields: 0, methods: 2, attributes: 1
Constant pool:
   #1 = Methodref          #2.#3          // java/lang/Object."<init>":()V
   #2 = Class              #4             // java/lang/Object
   #3 = NameAndType        #5:#6          // "<init>":()V
   #4 = Utf8               java/lang/Object
   #5 = Utf8               <init>
   #6 = Utf8               ()V
   #7 = Fieldref           #8.#9          // java/lang/System.out:Ljava/io/PrintStream;
   #8 = Class              #10            // java/lang/System
   #9 = NameAndType        #11:#12        // out:Ljava/io/PrintStream;
  #10 = Utf8               java/lang/System
  #11 = Utf8               out
  #12 = Utf8               Ljava/io/PrintStream;
  #13 = Class              #14            // Outer
  #14 = Utf8               Outer
  #15 = String             #16            // > Outer 클래스의 static final 필드 입니다.
  #16 = Utf8               > Outer 클래스의 static final 필드 입니다.
  #17 = Methodref          #18.#19        // java/io/PrintStream.println:(Ljava/lang/String;)V
  #18 = Class              #20            // java/io/PrintStream
  #19 = NameAndType        #21:#22        // println:(Ljava/lang/String;)V
  #20 = Utf8               java/io/PrintStream
  #21 = Utf8               println
  #22 = Utf8               (Ljava/lang/String;)V
  #23 = Fieldref           #13.#24        // Outer.value:Ljava/lang/String;
  #24 = NameAndType        #25:#26        // value:Ljava/lang/String;
  #25 = Utf8               value
  #26 = Utf8               Ljava/lang/String;
  #27 = Class              #28            // Main
  #28 = Utf8               Main
  #29 = Utf8               Code
  #30 = Utf8               LineNumberTable
  #31 = Utf8               main
  #32 = Utf8               ([Ljava/lang/String;)V
  #33 = Utf8               SourceFile
  #34 = Utf8               Main.java
{
  public Main();
    descriptor: ()V
    flags: (0x0001) ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 1: 0

  public static void main(java.lang.String[]);
    descriptor: ([Ljava/lang/String;)V
    flags: (0x0009) ACC_PUBLIC, ACC_STATIC
    Code:
      stack=2, locals=1, args_size=1
         0: getstatic     #7                  // Field java/lang/System.out:Ljava/io/PrintStream;
         3: ldc           #15                 // String > Outer 클래스의 static final 필드 입니다.
         5: invokevirtual #17                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
         8: getstatic     #7                  // Field java/lang/System.out:Ljava/io/PrintStream;
        11: getstatic     #23                 // Field Outer.value:Ljava/lang/String;
        14: invokevirtual #17                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
        17: return
      LineNumberTable:
        line 3: 0
        line 5: 8
        line 6: 17
}
SourceFile: "Main.java"
```

Constant pool #16 을 보면 그냥 상수값 자체를 가져와서 사용하는 걸 알 수 있다. 

### 클래스 로드 시점 테스트

자바 공식문서를 보면 다음 경우에서 로드한다고 한다.
- 다른 객체에서 런타임 상수 풀에서 객체를 참조
- Java SE 클래스 관련 API 사용

근데 문제는 첫 번째 조건 "참조" 라는게 정확히 뭔지 모르겠다는 것.   
(런타임 상수 풀을 잘 몰라서 그런 것 같긴 하지만)

자바 바이트코드의 상수 풀에서 다른 클래스 정보를 가지고 있는 걸 "참조" 라고 표현하는지,    
직접 해당 객체의 코드를 실행 하는게 "참조" 인지 모르겠어서 직접 실행해보기로 했다.

```java
package dev.yangsijun;  
  
import java.time.LocalTime;  
  
public class Main {  
  public static void main(String[] args) throws InterruptedException {  
    System.out.println(LocalTime.now());  
    Thread.sleep(1000*5);  
    System.out.println(Outer.value);  
    System.out.println(LocalTime.now());  
  }  
}  
  
class Outer {  
  static String value = "> Outer 클래스의 static 필드 입니다.";  
}
```

만약 다른 클래스를 상수 풀에서 참조하는 것으로 클래스가 로드된다면 바로 바로 로드 되고, 직접 객체를 사용해야 로드 된다면 5초 뒤에 로드 될 것이다.

`> java -cp /Users/sijunyang/Documents/GitHub/testing_java/java/build/libs/java-1.0-SNAPSHOT.jar -verbose:class dev.yangsijun.Main`

위 명령어를 사용해서 빌드했다. `-verbose:class`를 사용하면 클래스 로드 시 로그가 남는다.

결과

```
[0.095s][info][class,load] java.nio.HeapCharBuffer source: shared objects file
[0.095s][info][class,load] java.nio.charset.CoderResult source: shared objects file
23:50:24.067470
[5.102s][info][class,load] dev.yangsijun.Outer source: file:/Users/sijunyang/Documents/GitHub/testing_java/java/build/libs/java-1.0-SNAPSHOT.jar
> Outer 클래스의 static 필드 입니다.
23:50:29.074803
[5.102s][info][class,load] java.util.IdentityHashMap$IdentityHashMapIterator source: shared objects file
[5.102s][info][class,load] java.util.IdentityHashMap$KeyIterator source: shared objects file
```

후자인 직접 객체를 사용하는 경우 로드를 시작한다는 걸 알 수 있다.

생각해보면, 당연한 결과 같기도 하다.   
상수 풀에 존재해서 사용할 수 있다는 것 만으로 로드하면 결국 사용하는 모든 클래스를 로드해야 할 테니까.


##### 추가

다른 메서드에서 호출하는 경우는 어떻게 되는가?

```java
package dev.yangsijun;  
  
import java.time.LocalTime;  
  
public class Main {  
  public static void main(String[] args) throws InterruptedException {  
    System.out.println(LocalTime.now());  
    System.out.println("Start main method!");  
    Thread.sleep(1000*5);  
    System.out.println("Call another method!");  
    another();  
    System.out.println("End main method!");  
    System.out.println(LocalTime.now());  
  }  
  
  
  public static void another() throws InterruptedException {  
    System.out.println("Here Is another method!");  
    System.out.println(LocalTime.now());  
    Thread.sleep(1000*5);  
    System.out.println(Outer.value);  
  }  
}  
  
class Outer {  
  static String value = "> Outer 클래스의 static 필드 입니다.";  
}
```

```
22:16:41.911743
Start main method!
Call another method!
Here Is another method!
22:16:46.917113
[10.150s][info][class,load] dev.yangsijun.Outer source: file:/Users/sijunyang/Documents/GitHub/testing_java/java/build/libs/java-1.0-SNAPSHOT.jar
> Outer 클래스의 static 필드 입니다.
End main method!
22:16:51.921849
```

결과를 보면 `Outer`를 사용하는 `another()` 메서드에서 로드한다.

또한 사용하기 직전에 로드된다.

(추측: 일반적으론 클래스 초기화 시점과 동일한 것 같다.)

[2.6.3. Dynamic linking](https://docs.oracle.com/javase/specs/jvms/se11/html/jvms-2.html#jvms-2.5.5)를 보면 프레임 단위로 연결하면서 로드하기도 한다고 하긴 함.

# Reference

- [Java SE 8 JVM Spec](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-5.html)
- [Java SE 8 JLS](https://docs.oracle.com/javase/specs/jls/se8/html/jls-12.html#jls-12.4.1)
- [JVM Internal](https://d2.naver.com/helloworld/1230)
- [클래스는 언제 메모리에 로딩 & 초기화 되는가?](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%ED%81%B4%EB%9E%98%EC%8A%A4%EB%8A%94-%EC%96%B8%EC%A0%9C-%EB%A9%94%EB%AA%A8%EB%A6%AC%EC%97%90-%EB%A1%9C%EB%94%A9-%EC%B4%88%EA%B8%B0%ED%99%94-%EB%90%98%EB%8A%94%EA%B0%80-%E2%9D%93)
- [Back to the Essence - Java 컴파일에서 실행까지 - (2)](https://homoefficio.github.io/2019/01/31/Back-to-the-Essence-Java-%EC%BB%B4%ED%8C%8C%EC%9D%BC%EC%97%90%EC%84%9C-%EC%8B%A4%ED%96%89%EA%B9%8C%EC%A7%80-2/)
