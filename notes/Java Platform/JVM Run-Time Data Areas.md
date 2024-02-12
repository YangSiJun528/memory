- - -

![](notes/Java%20Platform/files/Run-Time_Data_Area.png)
JVM 런타임 데이터 영역을 다음과 같이 표시할 수 있다.

위 이미지에서 "단위"는 생성 주기를 의미한다.   
ex: Heap과 Method Area는 JVM이 시작할 때 시작하고, JVM이 종료될 때 소멸된다.

### Heap

모든 Java 객체가 저장되는 영역이다.

새 클래스 인스턴스나 배열을 만들 때마다 JVM은 힙에서 사용 가능한 메모리를 찾아 객체에 할당한다.     
생성된 객체는 사용자가 해제할 수 없고, Garbage Collector가 사용하지 않는 객체를 찾아 제거한다.  
(GC 알고리즘, 구현은 여러 종류가 있다.)

힙은 JVM 시작 시 생성하고 JVM 종료 시 소멸된다.

### Method Area

클래스 및 인터페이스 정의를 저장하는 영역이다.

클래스 로더가 JVM에 클래스를 로드한다. 이후 JVM은 클래스의 내부 표현(객체의 필드, 메서드, 생성자 정보를 포함)을 생성한다.  
이 표현은 런타임에 객체를 생성하고 메서드를 호출하는데 사용된다.

> The method area is created on virtual machine start-up. Although the method area is logically part of the heap, simple implementations may choose not to either garbage collect or compact it. This specification does not mandate the location of the method area or the policies used to manage compiled code. The method area may be of a fixed size or may be expanded as required by the computation and may be contracted if a larger method area becomes unnecessary. The memory for the method area does not need to be contiguous.
> 
> A Java Virtual Machine implementation may provide the programmer or the user control over the initial size of the method area, as well as, in the case of a varying-size method area, control over the maximum and minimum method.
> 
> (번역)
> 메서드 영역은 가상 머신이 시작될 때 생성됩니다. 메서드 영역은 논리적으로 힙의 일부이지만, 간단한 구현에서는 가비지 수집이나 압축을 하지 않을 수도 있습니다. 이 사양은 메서드 영역의 위치나 컴파일된 코드를 관리하는 데 사용되는 정책을 강제하지 않습니다. 메서드 영역은 고정된 크기일 수도 있고 계산에 따라 필요에 따라 확장될 수도 있으며, 더 큰 메서드 영역이 불필요해지면 축소될 수도 있습니다. 메서드 영역의 메모리는 연속적일 필요는 없습니다.
> 
> Java 가상 머신 구현은 프로그래머 또는 사용자에게 메서드 영역의 초기 크기에 대한 제어를 제공할 수 있으며, 크기가 다양한 메서드 영역의 경우 최대 및 최소 메서드에 대한 제어를 제공할 수 있습니다.

메서드 영역은 논리적으로 힙 영역의 일부이다.   
하지만 JVM 사양은 메소드 영역의 크기를 정의하지 않으며 JVM이 메모리 블록을 처리하는 방식도 정의하지 않는다.   

메서드 영역은 JVM 시작 시 생성하고 JVM 종료 시 소멸된다.

(확실하지 않음) 로드 된 클래스들은 클래스 로더 자체를 삭제하고 새로운 클래스로더를 사용해서 reload 할 수 있다. 아마 남아있는 클래스 로더에서 객체를 칮을 수 없어서 새로 로드하는 것 같다. 근데 클래스 로드를 삭제하면 기존 로드된 클래스가 unload(메모리 영역에서 해제) 되는지는 알 수 없다.

### Run-Time Constant Pool

클래스 및 인터페이스 이름, 필드 이름, 메서드 이름에 대한 기호 참조를 포함하는 메서드 영역 내의 영역이다.

컴파일 타임에 이미 알 수 있는 리터럴 값(상수)부터 런타임에 해석되는 메서드와 필드의 참조까지를 포괄하는 여러 종류의 상수가 포함된다.

기존의 프로그래밍 언어(ex: C언어)의 [심볼 테이블](https://en.wikipedia.org/wiki/Symbol_table)과 비슷한 역할을 한다. 다만 데이터의 범위가 더 넓은 편.

### PC Register

JVM의 스레드는 각각 자체 PC(Program Counter) 레지스터를 가지고 있다.

PC 레지스터는 해당 스레드의 실행 위치를 추적하는 데 사용된다.

JVM은 플랫폼 독립성을 위해서 하드웨어의 CPU를 사용할 수 없다.   
OS나 CPU의 입장에서 JVM은 하나의 프로세스이다.   
따라서 각 스레드의 다음 실행할 명령어 정보를 저장하기 위해서 PC 레지스터를 사용한다.

네이티브가 아닌 메서드의 경우, PC 레지스터는 현재 실행 중인 명령어의 주소를 저장한다.   
네이티브 메서드의 경우, PC 레지스터는 정의되지 않은 값을 갖는다.   
(Native Method Stack에서 따로 처리한다.)

PC 레지스터는 해당 스레드의 실행 위치를 추적하는 데 사용됩니다.

스레드와 동일한 생명 주기를 가진다.
### JVM Stack

JVM 스택은 메소드 호출 정보를 저장하는 데이터 구조이다.

JVM 스택은 프레임을 저장한다.

로컬 변수와 부분 결과를 보관하고 메서드 호출 및 반환에 일부 역할을 하는 등 C와 같은 기존 언어의 스택과 유사한 역할을 한다.

프레임은 힙에 할당될 수 있다. (JVM Spec은 그렇게 말하는데, 실제로 어떤지는 모르겠다.)

JVM 사양은 JVM 스택의 크기와 메모리 할당을 처리하는 방법을 자유롭게 결정할 수 있도록 한다.

JVM 스택 덕분에 JVM은 프로그램 실행을 추적하고 StackTrace를 기록할 수 있다.

#### Frame

프레임은 데이터 및 부분 결과(partial results)를 저장하는 것뿐만 아니라 동적 연결을 수행하고 메서드에 대한 값을 반환하며 예외를 전달하는 데 사용된다.

메소드가 호출될 때마다 새 프레임이 생성된다. 


정상적이든 갑작스럽든(throws uncaught exception) 관계없이 메서드 호출이 완료되면 프레임이 삭제된다.

프레임의 요소는 다음과 같다.
##### Local Variables(지역 변수)
- this, 메서드의 파라미터, 지역 변수 등의 값이 담긴다.
- 1개의 슬롯이 4byte 공간을 가지므로 `boolean`, `byte`, `char`, `short`, `int`, `float`, `reference`, `returnAddress`는 배열의 1개의 슬롯에 저장되고, `long`과 `double`은 2개의 슬롯에 걸쳐 저장된다.
- 로컬 변수 배열의 크기는 컴파일 타임에 결정되며 바이트코드의 `Code` 속성에 `locals`으로 표시된다.
- 인덱싱을 통해 접근한다.
- 메서드가 호출될 때 그 메서드의 파라미터 값은 로컬 변수 배열을 통해 넘겨진다.
	- 메서드가 클래스 메서드이면 첫 번째 파라미터는 0번 슬롯에 두 번째 파라미터는 1번 슬롯에 차례대로 저장되고,
	- 메서드가 인스턴스 메서드이면 this가 0번 슬롯에 먼저 저장되고, 첫 번째 파라미터는 1번 슬롯에, 두 번째 파라미터는 2번 슬롯에 차례대로 저장된다.
##### Operand Stacks(피연산자 스택)
- 실제 연산이 이루어지는 곳. 피연산자 스택과 지역 변수 배열 사이에서 데이터를 교환하고, 다른 메서드 호출 결과를 추가하거나(push) 꺼낸다(pop).
- 상수, 지역 변수(위에서 말하는), 등 값을 스택으로 로드하고 명령어를 실행하거나 다른 메서드의 인자로 제공하거나 메서드 리턴 결과를 반환하는 등의 용도로 사용된다. (바이트코드를 보면 이해하기 쉽다.)
- 오퍼랜드 스택의 최대 깊이는 컴파일 타임에 결정되며 바이트코드의 `Code` 속성에 `stack`으로 표시된다.
- 프레임이 생성되는 시점에 빈 상태를 가진다.
##### Reference to Run-Time Constant Pool(런타임 상수 풀에 대한 참조)
- 각 프레임에는 메서드 코드의 동적 연결을 지원하기 위해 현재 메서드 유형에 대한 런타임 상수 풀에 대한 참조가 포함되어 있다.
- 프레임에서 클래스의 런타임 상수 풀의 정보를 사용하기 위해 사용한다.

##### Dynamic linking
메서드의 클래스 파일 코드(바이트코드)는 심볼릭 레퍼런스를 통해 호출할 메서드와 액세스할 변수를 나타낸다.

동적 연결을 통해 이러한 메서드를 구체적인(direct) 참조로 변경하고 아직 정의되지 않은 심볼을 해결하기 위해 필요에 따라 클래스를 로드한다.

### Native Method Stack
native 메소드(Java 프로그래밍 언어 이외의 언어로 작성된 메소드)를 지원하기 위해 사용한다.

기본 메서드 스택은 일반적으로 각 스레드가 생성될 때 스레드별로 할당된다. (명세에서 '일반적'으로 라고 함)

Native Method를 수행할 때는 JVM을 거치지 않고 바로 수행하게 된다.   
어차피 Native Code는 Platform에 종속 될 수 밖에 없기 때문에 JVM을 경유할 필요가 없기 때문이다. 

(JVM 사양에선 기본 메서드 호출을 지원하지 않을 수도 있다고 힌다.)

# Reference
- [baeldung - The JVM Run-Time Data Areas](https://www.baeldung.com/java-jvm-run-time-data-areas)
- [Java SE 11 JVM Spec](https://docs.oracle.com/javase/specs/jvms/se11/html/jvms-2.html#jvms-2.5)
- [Back to the Essence - Java 컴파일에서 실행까지 - (2)](https://homoefficio.github.io/2019/01/31/Back-to-the-Essence-Java-%EC%BB%B4%ED%8C%8C%EC%9D%BC%EC%97%90%EC%84%9C-%EC%8B%A4%ED%96%89%EA%B9%8C%EC%A7%80-2/)
- [Java와 JVM -4](https://blog.embian.com/61)
	- Java performance Fundamental 책 정리한 내용