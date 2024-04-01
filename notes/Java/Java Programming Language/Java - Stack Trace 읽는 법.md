
알고는 있었는데, 설명하려니 헷갈려서 정리

#### StackTrace란?
코드의 이동? 수행?의 기록. 주로 예외 발생 시 함께 출력함

예외의 스택트레이스는 그 예외가 발생한 시점을 기록하고 있음. (바깥으로 전파된다고 변하는거 아님)

#### 읽는 법
처음 줄에는 주로 예외의 이유를 설명.     
프레임워크를 사용중이라면 기본적으로 프레임워크가 Catch하므로 예외 설명과 스택트레이스.    
프레임워크를 사용하지 않으고 Catch하지 않으면 예외 설명, 해당 예외가 발생한 쓰레드와  스택트레이스.    

`at ~~` 부터는 Stack인 만큼 위가 가장 최근 위치(메서드)이고, 밑으로 내려갈수록 이전에 실행 된 것.    
즉, 아래쪽이 위 메서드를 호출하는 식이다. (JVM 런타임 메모리 영역 Call Stack을 생각해보라)

`at ex.패키지명(파일위치.확장자(주로 java):파일 열 번호)` 같은 구조를 가진다.

`Caused by:`는 Chained Exception이 발생한 경우 표시, 다른 예외로 발생한 예외를 wrap한 경우라고 볼 수 있다. (여러 번 반복될 수 있다.)





#### 에시
예시1: 프레임워크 X, catch 없음
```java
Enter value in Celsius to convert in fahrenheit: hero 
Exception in thread "main" java.util.InputMismatchException 
	at java.base/java.util.Scanner.throwFor(Scanner.java:939)
	at java.base/java.util.Scanner.next(Scanner.java:1594)
```

예시2: `main()`에서 catch, Chained Exception

```java
Caught IOException: Invalid file path
java.io.IOException: Invalid file path
    at Main.readFile(Main.java:24)
    at Main.doSomething(Main.java:15)
    at Main.main(Main.java:7)
Caused by: java.lang.NullPointerException
    at java.base/java.io.FileInputStream.<init>(FileInputStream.java:149)
    at java.base/java.io.FileInputStream.<init>(FileInputStream.java:111)
    at Main.readFile(Main.java:21)
    ... 2 more
```

1. `readFile()`에서 `IOException` 발생
	1. `readFile()`은 `doSomething()`에서 호출, `doSomething()`는 `main()`에서 호출
2. `IOException`의 원인은 `NullPointerException`
	1. `NullPointerException`는 `java.io.FileInputStream.<init>`에서 발생
	2. `java.io.FileInputStream.<init>`는 `readFile()`에서 호출
3. 결국 마지막의 `NullPointerException` 가 원인이며, 아래 코드의 `getFilePath()`이 nuil이 반환해서 이런 에러가 발생했음을 알 수 있다.

예시2의 코드
```java
import java.io.FileInputStream;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try {
            doSomething();
        } catch (IOException e) {
            System.out.println("Caught IOException: " + e.getMessage());
            e.printStackTrace();
        }
    }

    private static void doSomething() throws IOException {
        readFile();
    }

    private static void readFile() throws IOException {
        String filePath = getFilePath();
        try {
            FileInputStream fis = new FileInputStream(filePath);
            // ... file read logic
        } catch (NullPointerException e) {
            throw new IOException("Invalid file path", e);
        }
    }

    private static String getFilePath() {
        String path = null;
        // ... get file path logic
        return path;
    }
}
```

[replit에서 실행 가능](https://replit.com/join/hcqiavpqxv-yangsijun528)

예시3: [초보 개발자를 위한 스택트레이스 읽는 법](https://okky.kr/articles/338405) 

그냥 링크 타고 글 읽기

# References
- [초보 개발자를 위한 스택트레이스 읽는 법](https://okky.kr/articles/338405) 
- [내가 작성한 예시2 코드](https://replit.com/join/hcqiavpqxv-yangsijun528)