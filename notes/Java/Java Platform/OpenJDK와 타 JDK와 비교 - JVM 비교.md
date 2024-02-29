- - -

### 비교표

OSS = Open Source Software

| Community/Vendor   | Product Name | OSS/Commercial | Architecture | Description                                                                                                                                                                                                                                                                               |
|---------------------|--------------|-----------------|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ORACLE              | Oracle JDK   | Commercial      | Hotspot      | · Java 8 2019년 1월 이후 License 필요 <br>· 검토 시 기준 값으로 사용                                                                                                                                                                                                                             |
| AZUL                | Zing         | Commercial      | Zing         | · Azul의 자체 아키텍처를 가진 JVM. <br>· 상용 버전 제품만 존재<br>· 고성능 및 확장성에 중점                                                                                                                                                                                   |
| OpenJDK.org         | OpenJDK      | OSS             | Hotspot      | · 썬 마이크로시스템즈 시절에 만들어진 커뮤니티 그룹으로 우리가 알고 있는 OpenJDK가 만들어지는 곳. <br>· 커뮤니티에 집중되어 있고 Source는 배포하지만 Binary 배포는 https://jdk.java.net을 통하여 이루어 잔다. <br>· https://jdk.java.net에서 배포되는 Binary는 참조용으로 사용할 것을 권장.  |
| AZUL                | Zulu         | OSS             | Hotspot      | · Azul에서 오픈소스로 제공하는 JDK. <br>Zing과는 아키텍처 자체가 다르다. <br>· OpenJDK를 기반으로 개발, 별도의 계약으로 지원을 받을 수 있다.                                                                                                                           |
| Red Hat/CentOS      | OpenJDK      | OSS             | Hotspot      | · 레드햇에서 제공. <br>· RHEL(Red Hat Enterprise Linux) 또는 CentOS yum repository를 통하여 rpm(binary)으로 배포된다. <br>· OS 버전에 따라서 지원되는 버전이 조금씩 다르다.                                                                  |
| AdoptOpenJDK.net    | OpenJDK      | OSS             | Hotspot, OpenJ9(IBM) | · OpenJDK 배포를 주 목적으로 하며, 여러 기업의 후원을 받아 개발자들이 운영하는 커뮤니티이다. <br>· 오라클의 지원으로 OpenJDK.org가 주도하는 Hotspot(openjdk)과, IBM의 지원으로 Eclipse가 주도하는 OpenJ9 VM의 Binary를 모두 제공한다. <br>· 현 상황에서 가장 다양한 플랫폼과 VM 아키텍처의 Binary를 제공 받을 수 있다. |
| Eclipse.org         | OpenJ9       | OSS             | OpenJ9(IBM)  | ·IBM이 자체 JDK를 Eclipse에 기부하면서 시작된 프로젝트로 VM 아키텍처가 Hotspot과 다르다. <br>· 기존의 Hotspot 계열의 JVM과 설정 및 아키텍처에서 다수의 차이가 있다.     |

### Java 11 기준 JDK 선택에 도움을 주는 플로우차트

상용 서비스 제공 여부, OpenJDK 기반 JDK 여부, 특수 목적의 선택 기준 등 여러 조건이 있다.    
여려 JDK 구현체의 종류와 특징을 알기 쉬워서 가져옴.  

![java_vender_guide_flowchart](notes/Java/files/java_vender_guide_flowchart.png)

# Reference
[LINE의 OpenJDK 적용기: 호환성 확인부터 주의 사항까지](https://engineering.linecorp.com/en/blog/line-open-jdk)     
[위키피디아 - List of Java virtual machines](https://en.wikipedia.org/wiki/List_of_Java_virtual_machines)
[stackoverflow - Difference between OpenJDK and Adoptium/AdoptOpenJDK](https://stackoverflow.com/questions/52431764/difference-between-openjdk-and-adoptium-adoptopenjdk) 