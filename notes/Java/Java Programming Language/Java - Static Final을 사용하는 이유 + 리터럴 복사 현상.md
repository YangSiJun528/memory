
주로 다음과 같은 이유이다.
- 상수는 모든 곳에서 하나의 값을 일관되게 사용할 수 있어야 하므로 Static Final을 사용해야 한다.
- 컴파일 시점에 모든 인스턴스가 공유하고, 불변인 값을 부여할 수 있다.

저번에 바이트코드 분석해보다가 알게 된 것이 있어서 정리하려고 한다. 
**리터럴 값을 상수로 사용하면, 컴파일 시 상수 자체를 복사한다.**

### 설명

예제 코드는 다음과 같다.

실제로는 별개의 파일이고, 설명을 위해 한 코드 블록으로 작성하였다.
```java
public class Main {  
  public static void main(String[] args) {  
    System.out.println(Utils.INT_A);  
    System.out.println(Utils.STRING);  
    System.out.println(Utils.SOMETHING);  
    System.out.println(Utils.LONG);  
    System.out.println(Test.A);  
    System.out.println(Test.B);  
  }  
}

public class Something {  
  public String hello() {  
    return "Hello";  
  }    
}

public enum Test {  
  A,B;  
}

public class Utils {  
  public static final Integer INT_A = 111;  
  public static final String STRING = "STRING_1";  
  public static final Something SOMETHING = new Something();  
  public static final long LONG = 1123L;   
}
```


바이트코드로 컴파일하면 다음과 같이 보여진다.

```java
public class Main {  
  public Main() {  
  }  
  
  public static void main(String[] args) {  
    System.out.println(Utils.INT_A);  
    System.out.println("STRING_1");  
    System.out.println(Utils.SOMETHING);  
    System.out.println(1123L);  
    System.out.println(Test.A);  
    System.out.println(Test.B);  
  }  
}
```

리터럴 값을 담고 있는 `String`과 `long`은 값 자체를 복사한다.

구체적인 이유까지는 잘 모르겠지만, 따로 `Utils`클래스를 통하지 않고, 바로 접근할 수 있어서 효율적인 것 같다.

# References
- 따로 참고한 자료 없음