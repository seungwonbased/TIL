# Service

- Pod를 연결하고 외부에 노출

## Service 기능

- 여러 개의 파드에 쉽게 접근할 수 있도록 고유한 도메인 이름을 부여
- 여러 개의 파드에 접근할 때, 요청을 분산하는 로드 밸런서 기능을 수행     
- 클라우드 플랫폼의 로드 밸런서, 클러서 노드의 포트 등을 통해 파드를 외부에 노출

## 서비스를 생성하는 방법

- 매니페스트(YAML)를 이용해서 생성 
- kubectl expose 명령을 사용해서 생성

## Pod의 IP 주소를 기반으로 애플리케이션에 접근했을 때 문제점

- Pod가 수시로 삭제, 생성되는 과정에서 애플리케이션 Pod의 IP 주소가 변경될 수 있음
	- 클라이언트 Pod가 변경된 애플리케이션 Pod의 IP 주소를 알 수 없음
	- 이름을 기반으로 애플리케이션 Pod에 접근할 수 있는 방안이 필요 
- 클라이언트 Pod는 자신이 알고 있는 애플리케이션 Pod의 IP로만 접근
	- 부하가 특정 파드로 집중될 수 있음
	- 로드밸런싱 기능이 필요

## Service Types

### ClusterIP

![service](https://github.com/seungwonbased/TIL/blob/main/K8s/assets/service1.png)

- 디폴트
- 클러스터 내부에서 Pod들에 접근할 때 사용 
- 외부로 Pod를 노출하지 않기 때문에 클러스터 내부에서만 사용되는 Pod에 적합
- sessionAffinity 옵션
	- 클라이언트 IP 별로 전송 Pod를 고정시킴

### NodePort

![service](https://github.com/seungwonbased/TIL/blob/main/K8s/assets/service2.png)

- Pod에 접근할 수 있는 포트를 클러스터의 모든 노드에 동일하게 개방
- **외부에서 Pod에 접근할 수 있는 서비스 타입**
- 접근할 수 있는 포트는 랜덤으로 정해지지만, 특정 포트로 접근하도록 설정할 수 있음
- 노드의 공개 IP 주소에 공개 포트를 오픈
	- 클러스터 외부에서 클러스터 내부의 Pod로 요청을 전달하는 것이 가능
	- 공개 포트는 기본적으로 30,000 ~ 32,767 범위를 사용

### LoadBalancer

![service](https://github.com/seungwonbased/TIL/blob/main/K8s/assets/service3.png)

- 클라우드 플랫폼에서 제공하는 로드 밸런서를 동적으로 프로비저닝해 Pod에 연결
- 로드 밸런서에서 들어오는 트래픽을 클러스터 내의 Pod로 전달
- **NodePort 타입과 마찬가지로 외부에서 Pod에 접근할 수 있는 서비스 타입**
- 일반적으로 온프레미스 환경에서는 NodePort 타입의 서비스를 사용하고, 퍼블릭 클라우드 환경에서는 LoadBalancer 타입의 서비스를 사용해서 애플리케이션을 외부에 노출

#### L4 Load Balancer
- 네트워크 계층 또는 트랜스포트 계층의 정보를 기반으로 부하 분산
- IP 주소 또는 Port 주소를 이용
- 쿠버네티스에서 말하는 일반적인 로드 밸런서는 L4 로드 밸런서의 기능을 수행

#### L7 Load Balancer
- 애플리케이션 계층의 정보를 기반으로 부하 분산
- URL, HTTP Header, Cookie 등과 같은 사용자 요청 정보를 이용
- 쿠버네티스에서 L7 로드 밸런서 기능을 Ingress를 사용해 구현

#### On-Premise 환경에서 LoadBalancer 타입의 서비스 연동
- 클러스터 내부에 로드 밸런서 서비스를 받아주는 구성이 필요 -> MetalLB
- MetalLB
	- 쿠버네티스 클러스터 내에서 로드 밸런싱을 제공하기 위한 오픈 소스 프로젝트
	- 클라우드 공급자에서 실행되지 않는 클러스터에서 LoadBalancer 유형의 쿠버네티스 서비스 생성이 가능

### ExternalName

![service](https://github.com/seungwonbased/TIL/blob/main/K8s/assets/service4.png)

- 외부 서비스를 쿠버네티스 내부에서 호출하고자 할 때 사용
- 클러스터 내의 Pod에서 외부 IP 주소에 서비스의 이름으로 접근할 수 있음
- Pod에서 쿠버네티스 클러스터 외부의 엔드포인트에 접속하기 위한 이름을 해결해주는 역할
	- i.g., 퍼블릭 클라우드의 데이터베이스 또는 인공지능 API 서비스 등에 접근할 때 사용

