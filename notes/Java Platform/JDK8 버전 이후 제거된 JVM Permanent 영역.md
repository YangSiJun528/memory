- - -
Permanent: 영구적인
Native 메모리: OS가 관리하는 메모리

### 변화

```
<----------- Java Heap -----------> <--- Native Memory --->
+------+----+----+-----+-----------+--------+--------------+
| Eden | S0 | S1 | Old | Permanent | C Heap | Thread Stack |
+------+----+----+-----+-----------+--------+--------------+
                        <--------->
                       Permanent Heap
```

```
<----- Java Heap -----> <--------- Native Memory --------->
+------+----+----+-----+-----------+--------+--------------+
| Eden | S0 | S1 | Old | Metaspace | C Heap | Thread Stack |
+------+----+----+-----+-----------+--------+--------------+
```

Java SE 7 부터 PermGen 제거를 발표하였고, Java SE 8에서 공식적으로 제거되었다. ([JEP 122](https://openjdk.org/jeps/122))

이후 PermGen이 담당하는 기능은 Metaspace 영역이 맡게 되었다.

### Permanent Generation
JVM은 PermGen에 로드된 클래스 메타데이터를 추적한다.  
또한 PermGen에 모든 static(정적) 자료를 저장한다.

Java SE 7 이전에는 String Pool도 포함되었다.

PermGen의 최대 크기는 `-XX:MaxPermSize=size` 옵션을 사용하여 고정된다.    
이 할당된 크기를 초과할 수 없기 때문에 OutOfMemoryError가 발생하곤 했다.

### Metaspace
Java SE 8부터 시작되는 새로운 메모리 공간으로, PermGen을 대체하기 위해 등장했으며 동일한 역할을 한다.

Metaspace는 Heap이 아닌 기본 운영체제 메모리에 할당된다. (Native Memory를 사용한다고도 한다.)

운영 체제가 제공할 수 있는 범위까지 크기를 자동으로 늘릴 수 있다.   
(최대 크기를 설정할 수 있으나 기본 최대 크기는 무제한이다.)

(또한 [이 글](https://www.linkedin.com/pulse/java-memory-permgen-vs-metaspace-incus-data-pty-ltd-drxbe/)에 따르면 요즘 어플리케이션은 클래스 언로드가 종종 일어나고 해서 "Permanent"라는 표현을 사용하지 않는다고 하는데, 출처를 확인하진 않아서 확실하지는 않다.)
# References
- [Java Memory: PermGen vs MetaSpace](https://www.linkedin.com/pulse/java-memory-permgen-vs-metaspace-incus-data-pty-ltd-drxbe/)
- [JDK 8에서 Perm 영역은 왜 삭제됐을까](https://johngrib.github.io/wiki/java8-why-permgen-removed/)
- [Java 8: From PermGen to Metaspace](https://javaeesupportpatterns.blogspot.com/2013/02/java-8-from-permgen-to-metaspace.html)
- [Java 7 features – PermGen removal](https://javaeesupportpatterns.blogspot.com/2011/10/java-7-features-permgen-removal.html)
- [Baeldung - Permgen vs Metaspace in Java](https://www.baeldung.com/java-permgen-metaspace)
