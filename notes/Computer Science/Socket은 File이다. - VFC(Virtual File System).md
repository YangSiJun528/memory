### 요약/정리

Socket은 통신은 내부적으로 SockFS(Socket FileSystem)을 사용하여 이루어진다.

운영체제는 소켓이나 파이프 같은 다양한 커널 객체까지 VFS를 통해 하나의 추상화된 파일 시스템으로 사용하게 한다.

그래서 사용자는 일반 파일을 쓰고 읽는 것과 Socket도 동일하게 VFS를 사용해 파일처럼 읽고 쓴다. (System Call 을 사용한다.)

### 개요
유튜브에 있는 널널한 개발자의 네트워크 강의 영상을 보다가 Socket이 사실 운영체제 입장에서 File이라는 말을 들었다.

조금 궁금해져서 구글링해보니 비슷한 내용의 [인프런 질문글](https://www.inflearn.com/questions/866611/file%EA%B3%BC-socket%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C)을 찾을 수 있었고, 해당 질문글을 Claude AI에게 물어보니 VFC(Virtual file system, [Wikipedia](https://en.wikipedia.org/wiki/Virtual_file_system))라는 기술을 사용하는 걸 알게 되었다.

해당 정보를 기반으로 더 자세히 찾아보기로 했다. 

### 정리

공신력있는 자료를 찾아보니 [File system drivers (Part 1)](https://linux-kernel-labs.github.io/refs/heads/master/labs/filesystems_part1.html#virtual-filesystem-vfs)와 [Overview of the Linux Virtual File System](https://www.kernel.org/doc/html/next/filesystems/vfs.html)정도가 괜찮아 보였다.

> VFS is a generic interface between the user and a particular file system. This abstraction simplifies the implementation of file systems and provides an easier integration of multiple file systems.
>
> VFS는 사용자와 특정 파일 시스템 간의 일반적인 인터페이스입니다. 이러한 추상화는 파일 시스템 구현을 단순화하고 여러 파일 시스템을 보다 쉽게 ​​통합할 수 있도록 해줍니다.
> [File system drivers (Part 1)](https://linux-kernel-labs.github.io/refs/heads/master/labs/filesystems_part1.html#virtual-filesystem-vfs)

VFS는 사용자 프로그램과 커널 구현 사이의 추상화 계층이다.      
다양한 커널 객체(여러 파일시스템, 소켓, 파이프 등)를 하나의 통합된 파일 시스템 계층으로 표현할 수 있게 해준다.

그 중에 다음과 같은 가상 파일 시스템(_virtual filesystems (procfs, sysfs, sockfs, pipefs, etc.)_) 도 지원한다고 하는데, 여기서 sockfs가 우리가 일반적으로 말하는 socket이라고 볼 수 있다.

[PipeFS, SockFS, DebugFS, and SecurityFS](https://www.linux.org/threads/pipefs-sockfs-debugfs-and-securityfs.9638/)와 해당 글을 한국어로 정리한 [[Linux] File System 종류- DebugFS, SecurityFS, PipeFS, SockFS](https://42jerrykim.github.io/linux/linux-filesystem/)

> SockFS is a RAM-located pseudo-filesystem for storing information about the sockets/ports as well as a compatibility layer. SockFS is also called the Socket FileSystem. Now, as for SockFS being a compatibility layer, the write() system-call writes data to sockets/ports (the same ports like port-21 for FTP). Now, it is important to know that these sockets use the TCP or UDP network protocol and write() is the same system-call for writing files on the hard-disk. So, the question is "how is this possible?". The answer - SockFS. When write() writes data on SockFS, the filesystem makes changes to the data to make it suitable for the sockets. So, write() is not interacting directly with the sockets. Instead, the SockFS filesystem is acting as a mediator between the system-call and the sockets.

요약하면, SockFS(Socket FileSystem)는 다음과 같은 역할을 한다:
1. RAM에 위치한 가상 파일시스템으로, 소켓/포트 정보를 저장한다.
2. 호환성 계층(compatibility layer)의 역할, 파일을 디스크에 쓰는 write() 시스템 호출을 소켓에 데이터를 쓰는 작업으로 변환한다.
3. write() 시스템 호출이 직접 소켓과 상호작용하지 않고, SockFS 파일시스템이 중재자 역할을 하여 데이터를 소켓에 맞게 변환하여 전달한다.


# References
- 본문에 링크 다 결려있음