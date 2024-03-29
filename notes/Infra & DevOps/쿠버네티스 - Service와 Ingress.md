
![](https://heffenvox.com/wp-content/uploads/2023/06/ingress-1024x781.webp)

### Service

> 쿠버네티스 환경에서 **서비스(Service)** 는 **파드들을 통해 실행되고 있는 애플리케이션을 네트워크에 노출(expose)시키는 가상의 컴포넌트** 다.
> 
> ... 파드는 생성될 때마다 새로운 내부 IP를 받게 되므로, 이것만으로 클러스터 내/외부와 통신을 계속 유지하기는 어렵다.
> 
> 따라서 쿠버네티스에서는 **파드가 외부와 통신할 수 있도록** 클러스터 내부에서 고정적인 IP를 갖는 서비스(Service)를 이용하도록 하고 있다. 서비스는 또한 디플로이먼트나 스테이트풀셋처럼 같은 애플리케이션을 구동하도록 구성된 여러 파드들에게 **단일한 네트워크 진입점**을 부여하는 역할도 한다.

서비스는 L4 스위치다. PORT를 노출하고, 로드밸런서 역할을 한다.

서비스를 구성할때는 기본적으로 2개의 Port가 필요하다.
- port
	- 클러스터 내부에서 사용할 Service의 포트
- targetPort
	- Service가 받은 요청을 전달한 포트(Pod로 전달)

- Service객체로 전달된 요청을 Pod(deployment)로 전달할때 사용하는 포트

#### 서비스의 유형
- ClusterIP(기본)
	- 파드들이 클러스터 내부의 다른 리소스들과 통신할 수 있도록 해주는 가상의 클러스터 전용 IP다.
	- 클러스터 외부에서 접근 불가능하다.
	- port, targetPort를 설정해야 한다.
- NodePort
	- 외부에서 노드 IP의 특정 포트로 들어오는 요청을 감지하여, 해당 포트와 연결된 파드로 트래픽을 전달하는 유형의 서비스다.
	- 내부의 연결을 특정 파드로 연결하기 위한 ClusterIP도 생성된다.
- LoadBalancer
	- 클라우드의 로드벨런서를 클러스터의 서비스로 사용
	- 서비스를 클라우드 제공자 측의 자체 로드 밸런서로 노출시킨다.
	- 필요한 NodePort와 ClusterIP도 생성된다.
- ExternalName
	- 서비스에 selector 대신 DNS name을 직접 명시할 수 있다.


### Ingress

인그레스는 L7 스위치다. url로 여러 서비스의 라우팅 역할을 담당한다.

MSA로 개발되어 서비스간의 라우팅이 필요한 경우 Ingress를 사용한다.


# References
- [쿠버네티스에서 반드시 알아야 할 서비스(Service) 유형](https://seongjin.me/kubernetes-service-types/)
- [Kubernetes 서비스와 인그레스 용도구분](https://danawalab.github.io/kubernetes/2020/01/23/kubernetes-service-ingress.html)

