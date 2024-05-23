

- 참고할 자료
	- https://youtu.be/pL2Oi6fRGYs?si=4ogzlLCE4St1wxjn
	- https://youtube.com/playlist?list=PLq8wAnVUcTFVumZtWpBeZgK3iDEXmiMA1&si=SYjbcTSj6x6Eli7m << 여러 곳에 나뉘어져 있는데, 스트림, 입출력 관련 정리하기
	- https://hudi.blog/do-not-use-system-out-println-for-logging/
	- https://docs.oracle.com/javase/8/docs/api/java/lang/System.html
	- https://hudi.blog/java-inputstream-outputstream/
	- https://hudi.blog/java-filter-stream/


- 정리할 내용
	- 필수인 I/O Stream
	- I/O Stream을 확장하는 (데코레이터 패턴?) 다른 Stream - Buffer나 String 같은거
	- Scanner
	- Stream이 한번만 쓸 수 있는 이유 - 커서 바꾸면 되는거 같기도 한데, 잘 모르겠음
	- System.in, out 과 그 성능
		- 아마 표준 (콘솔) 입출력 스트림일 텐데, 이거 닫으면 sout 해도 출력 안되나?
	- nio는 굳이 안해도 될듯? 스트림이 주 내용이나까. 잠깐 소개하는 정도는 괜찮을지도?
	- C언어의 파일 입출력과 비교 - 크게 다르진 않은거 같은데