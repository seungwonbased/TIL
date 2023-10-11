# Ingress

- 클러스터 내의 서비스에 대한 외부 접근을 관리하는 API 오브젝트이며, 일반적으로 HTTP를 관리
- 인그레스는 부하 분산, SSL 종료, 명칭 기반의 가상 호스팅을 제공
- 인그레스를 사용하려면 인그레스 컨트롤러가 필요

## 개요

![ing](https://github.com/seungwonbased/TIL/blob/main/K8s/assets/ing1.png)

### 주요 기능

- 외부 요청의 라우팅 
- 가상 호스트 기반의 요청 처리
- SSL/TLS 보안 연결 처리

### 인그레스 컨트롤러

- 인그레스는 단지 요청을 처리하는 규칙을 정의하는 선언적인 오브젝트이며, 외부 요청을 받아들일 수 있는 실제 서버가 아니므로, 인그레스를 생성했지만 아무런 변화도 일어나지 않음
- 인그레스는 인그레스 컨트롤러(Ingress Controller)라고 하는 특수한 서버에 적용해야만 규칙을 사용할 수 있음
- 인그레스 컨트롤러가 바인딩되어야 ADDRESS 정보가 출력됨
- 실제로 외부 요청을 받아들이는 것은 인그레스 컨트롤러 서버이며, 이 서버가 인그레스 규칙을 로드해서 사용
	- Nginx 인그레스 컨트롤러도 그 종류 중 하나임

## 용어

- 노드 (Node)
	- 클러스터의 일부이며, 쿠버네티스에 속한 워커 머신
- 클러스터 (Cluster)
	- 쿠버네티스에서 관리되는 컨테이너화 된 애플리케이션을 실행하는 노드 집합
	- 이 예시와 대부분의 일반적인 쿠버네티스 배포에서 클러스터에 속한 노드는 퍼블릭 인터넷의 일부가 아님
- 에지 라우터 (Edge router)
	- 클러스터에 방화벽 정책을 적용하는 라우터
	- 이것은 클라우드 공급자 또는 물리적 하드웨어의 일부에서 관리하는 게이트웨이일 수 있음
- 클러스터 네트워크 (Cluster network)
	- 쿠버네티스 [네트워킹 모델](https://kubernetes.io/ko/docs/concepts/cluster-administration/networking/)에 따라 클러스터 내부에서 통신을 용이하게 하는 논리적 또는 물리적 링크 집합
- 서비스
	- [레이블](https://kubernetes.io/ko/docs/concepts/overview/working-with-objects/labels) 셀렉터를 사용해서 파드 집합을 식별하는 쿠버네티스 [서비스](https://kubernetes.io/ko/docs/concepts/services-networking/service/)
	- 달리 언급하지 않으면 서비스는 클러스터 네트워크 내에서만 라우팅 가능한 가상 IP를 가지고 있다고 가정

## 인그레스란?

![ing](https://github.com/seungwonbased/TIL/blob/main/K8s/assets/ing2.png)

- 인그레스는 클러스터 외부에서 클러스터 내부 서비스로 HTTP와 HTTPS 경로를 노출
- 트래픽 라우팅은 인그레스 리소스에 정의된 규칙에 의해 컨트롤됨
- 인그레스는 외부에서 서비스로 접속이 가능한 URL, 로드 밸런스 트래픽, SSL / TLS 종료 그리고 이름-기반의 가상 호스팅을 제공하도록 구성할 수 있음
- 인그레스 컨트롤러는 일반적으로 로드 밸런서를 사용해서 인그레스를 수행할 책임이 있으며, 트래픽을 처리하는데 도움이 되도록 에지 라우터 또는 추가 프런트 엔드를 구성할 수도 있음
- 인그레스는 임의의 포트 또는 프로토콜을 노출시키지 않음
	- HTTP와 HTTPS 이외의 서비스를 인터넷에 노출하려면 보통 [Service.Type=NodePort](https://kubernetes.io/ko/docs/concepts/services-networking/service/#type-nodeport) 또는 [Service.Type=LoadBalancer](https://kubernetes.io/ko/docs/concepts/services-networking/service/#loadbalancer) 유형의 서비스를 사용

## Ingress Doc

> https://kubernetes.io/ko/docs/concepts/services-networking/ingress/