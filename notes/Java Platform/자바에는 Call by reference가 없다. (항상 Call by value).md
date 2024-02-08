- - -

# call by value VS call by reference

함수/메서드 호출 시 인수를 전달하는 방식에 따라서 call by "something" 이라고 부른다.

- **Call by Value**
	- 값의 복사본을 넘겨준다.
	- 원래(호출하는 쪽)의 값에 영향을 주지 않는다.
- **Call by Reference**
	- 값의 주소(Reference)를 넘긴다.
	- 원래(호출하는 쪽)의 값에 영향을 준다.
	- 호출한 쪽과 

# 자바는 call by value만 사용한다.

자바에서는 메서드 호출 시 인수로 전달되는 것이 값의 복사본이지만,   
참조 타입의 경우에는 객체의 주소(Reference)를 가리키기 때문에 객체의 상태를 변경할 수 있다.   

이런 점이 Call by Reference와 비슷하기 때문에 Call by Reference를 사용한다고 오해한다.
하지만 주소값의 복사본이 전달되는 것이므로 메모리 관점에서는 "Call by Value"이다.  
(Call By Reference라면 객체 자체(주소)를 전달해서 제어할 수 있어야 했다.)

자바는 메모리 주소 자체에 접근할 수 없도록 만들어졌다.   
(정확하지 않지만 Heap 영역을 직접 제어할 수 없다는 의미인 듯? Stack에서 _참조 타입_ 으로만 제어할 수 있다. Stack에서 참조 타입의 값을 null로 만들더라도 객체의 연결이 끊겼을 뿐 남아있다. 물론 나중에 GC 되겠지만)

## 예시

```java
class Person {
  public String name;
}

public class Main {
  public static void main(String[] args) {
    Person p = new Person();
    p.name = "홍길동";
    updatePersonName(p);
    System.out.println(p.name); // "이름변경됨" - update()에서 변경된 값이 출력됨

    setPersonToNull(p);
    System.out.println(p.name); // "이름변경됨" - NPE가 발생하지 않음

  }

  public static void updatePersonName(Person p) {
    p.name = "이름변경됨";
  }

  public static void setPersonToNull(Person p) {
    p = null;
  }
}
```
[해당 replit](https://replit.com/@YangSiJun528/CallByValue#src/main/java/Main.java)에서 직접 실행해 볼 수 있다.

`setPersonToNull(Person p)`에서 넘겨받은 `p`을 `null`로 변경했음에도 NPE가 발생하지 않았다.

자바에서 메서드를 호출할 때, call by value로 **주소값을 복사**해서 전달해주기 때문이다.  

파라미터로 받은 참조 변수를 `null`로 변경할 경우, 호출한 쪽에는 영향을 주지 않는다.   
(메서드 내부에서 변수의 값을 주소값에서 null로 변경하여 참조를 끊은 것 뿐이다.)

이는 참조 변수가 실제 객체를 직접 가리키는 것이 아니라 객체가 저장된 메모리의 주소(참조)를 가리키기 떄문이다.

### C++과 비교

ChatGTP를 사용해서 Call by Reference를 사용하는 c++ 예시로 변경했다.  
(c++ 좀 아는 친구에게 물어보니 `*&`가 약간 생소한 듯 했다. 예시 자체가 잘못된 건 아닌 것 같다고 했음)

```c++
#include <iostream>
using namespace std;

class Person {
public:
  string name;
};

void updatePersonName(Person &p) { p.name = "이름변경됨"; }

void setPersonToNull(Person *&p) {
  delete p;    // 객체 삭제
  p = nullptr; // 참조를 nullptr로 설정
}

int main() {
  Person *p = new Person(); // Person 객체를 동적으로 할당하여 포인터로 가리킴
  p->name = "홍길동";
  updatePersonName(*p); // 객체에 대한 참조를 전달하기 위해 포인터를 역참조
  cout << p->name << endl; // "이름변경됨" - update()에서 변경된 값이 출력됨

  setPersonToNull(p); // 참조 포인터 전달
  if (p == nullptr) {
    cout << "p는 이제 nullptr입니다." << endl;
  } else {
    cout << p->name
         << endl; // 오류 없음 - nullptr로 설정된 객체에 접근하지 않음
  }

  delete p; // 동적으로 할당된 객체를 삭제
  return 0;
}
```
[해당 replit](https://replit.com/@YangSiJun528/CallByTest#main.cpp)에서 직접 실행해 볼 수 있다.

`setPersonToNull()`가 객체를 `null` 로 삭제하는 대신 아예 지워버리는데,   
메모리 값 자체를 가지고 제어할 수 있기 때문이다.

자바는 언어 설계 상 메모리를 직접 참조할 수 없으므로 이러한 동작이 불가능하다.

# Reference
- ⭐️[개발 블로그 - Call by Value & Call by Reference](https://incheol-jung.gitbook.io/docs/q-and-a/java/call-by-value-and-call-by-reference)
- [Inpa Dev 👨‍💻:티스토리 - ☕ 자바는 Call by reference 개념이 없다❓](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%9E%90%EB%B0%94%EB%8A%94-Call-by-reference-%EA%B0%9C%EB%85%90%EC%9D%B4-%EC%97%86%EB%8B%A4-%E2%9D%93)
- [[Java] 자바가 언제나 Call By Value인 이유 (Call By Reference X)](https://loosie.tistory.com/486)
- [Java is Pass-by-Value, Dammit!](https://www.javadude.com/articles/passbyvalue.htm
- ⭐️[stackoverflow - Confused, whether java uses call by value or call by reference when an object reference is passed? [duplicate]](https://stackoverflow.com/questions/10750098/confused-whether-java-uses-call-by-value-or-call-by-reference-when-an-object-re)