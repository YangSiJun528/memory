
![img](https://www.redhat.com/rhdc/managed-files/iaas-paas-saas-diagram5.1-1638x1046.png)

"OO as a Service"의 약자이다.    
구체적인 이름은 CSP(Cloud Service Provider)가 어느 영역까지 관리하는지에 따라 달라진다. (반대로 서비스 사용자가 어디까지 관리하는가로 볼 수도 있다.)

더 세분화하여 나누어지기도 하지만,  일반적으로 IaaS, PaaS, SaaS로 표현한다.

- #### IaaS
	- Infrastructure-as-a-Service
	- 인프라 수준의 클라우드 컴퓨팅을 제공
	- 장/단점
		- 장점: 가상화 덕분에 리소스 할당/해제가 자유로워 유연하고 탄력적인 인프라 운영이 가능하다.
		- 단점: 온프레미스환경과 비슷하게 많은 인프라를 신경써야 한다.
	- e.g. AWS EC2
- #### PaaS
	- Platform-as-a-Service
	- 개발 환경(Platform)을 구축하여 서비스로 제공하는 것
	- 장/단점
		- 장점: CSP에서 플랫폼을 구축하고 유지/관리 해준다(관리가 필요 없다). 서비스 외 적인 부분을 신경쓰지 않고, 제공된 서비스를 사용하면 된다.
		- 단점: 특정 플랫폼에 의존적이게 된다.
	- e.g. AWS Elastic Beanstalk, 관리형 DB(DBaaS라고도 부른다.)
- #### SaaS
	- Software-as-a-Service
	- 서비스 자체를 클라우드로 공유하는 것 (서비스까지 제공)
	- 장/단점
		- 장점: 소프트웨어 업데이트, 유지 관리 등을 공급자가 처리해준다.
		- 단점: 제공 업체의 소프트웨어를 사용하므로 기능/사용법을 의존한다.
	- e.g. Dropbox, Google Workspace, Slack

# References
- https://www.redhat.com/en/topics/cloud-computing/iaas-vs-paas-vs-saas
- https://kr.teradata.com/insights/data-architecture/intro-to-dbaas
- https://www.whatap.io/ko/blog/9/




