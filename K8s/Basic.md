# 쿠버네티스 (k8s, kubernetes)

> - 컨테이너 기반의 애플리케이션을 개발하고 배포할 수 있도록 설계된 오픈 소스 플랫폼
> - 컨테이너 오케스트레이션 도구의 사실상 표준

## 쿠버네티스가 제공하는 것

|쿠버네티스가 하는 것|사용자가 하는 / 설정하는 것|
|---|---|
|- 오브젝트(e.g., Pods)의 생성과 관리|- 클러스터와 노드 생성|
|- Pods를 모니터링하고 재생성, 스케일링 등|- 노드에 API 서버, kubelet, 그 외 다른 쿠버네티스 서비스와 소프트웨어 설치|
|- 쿠버네티스에 제공된 리소스를 설정에 맞게 활용|- 노드에 필요한 추가 리소스(로드 밸런서, 파일 시스템 등)을 생성|

- **서비스 디스커버리와 로드 밸런싱**
	- 쿠버네티스는 DNS 이름을 사용하거나 자체 IP 주소를 사용하여 컨테이너를 노출할 수 있음
	- 컨테이너에 대한 트래픽이 많으면, 쿠버네티스는 네트워크 트래픽을 로드밸런싱하고 배포하여 배포가 안정적으로 이루어질 수 있음
- **스토리지 오케스트레이션**
	- 쿠버네티스를 사용하면 로컬 저장소, 공용 클라우드 공급자 등과 같이 원하는 저장소 시스템을 자동으로 탑재할 수 있음
- **자동화된 롤아웃과 롤백**
	- 쿠버네티스를 사용하여 배포된 컨테이너의 원하는 상태를 서술할 수 있으며 현재 상태를 원하는 상태로 설정한 속도에 따라 변경할 수 있음
	- 예를 들어 쿠버네티스를 자동화해서 배포용 새 컨테이너를 만들고, 기존 컨테이너를 제거하고, 모든 리소스를 새 컨테이너에 적용할 수 있음
- **자동화된 빈 패킹 (Bin packing)**
	- 컨테이너화된 작업을 실행하는데 사용할 수 있는 쿠버네티스 클러스터 노드를 제공
	- 각 컨테이너가 필요로 하는 CPU와 메모리(RAM)를 쿠버네티스에게 지시
	- 쿠버네티스는 컨테이너를 노드에 맞추어서 리소스를 가장 잘 사용할 수 있도록 해줌
- **자동화된 복구 (Self-healing)**
	- 쿠버네티스는 실패한 컨테이너를 다시 시작하고, 컨테이너를 교체하며, '사용자 정의 상태 검사'에 응답하지 않는 컨테이너를 죽이고, 서비스 준비가 끝날 때까지 그러한 과정을 클라이언트에 보여주지 않음
- **시크릿과 구성 관리**
	- 쿠버네티스를 사용하면 암호, OAuth 토큰 및 SSH 키와 같은 중요한 정보를 저장하고 관리할 수 있음
	- 컨테이너 이미지를 재구성하지 않고 스택 구성에 시크릿을 노출하지 않고도 시크릿 및 애플리케이션 구성을 배포 및 업데이트할 수 있음

## 쿠버네티스 컴포넌트

![basic](https://github.com/seungwonbased/TIL/blob/main/K8s/assets/basic1.png)

- 쿠버네티스를 배포하면 클러스터를 얻음
- 쿠버네티스 클러스터는 컨테이너화된 애플리케이션을 실행하는 **노드**라고 하는 워커 머신의 집합
- 모든 클러스터는 최소 한 개의 워커 노드를 가짐
- 워커 노드는 애플리케이션의 구성요소인 파드를 호스트
- 컨트롤 플레인은 워커 노드와 클러스터 내의 파드를 관리
- 프로덕션 환경에서는 일반적으로 컨트롤 플레인이 여러 컴퓨터에 걸쳐 실행되고, 클러스터는 일반적으로 여러 노드를 실행하므로 내결함성과 고가용성이 제공됨

### 컨트롤 플레인 컴포넌트 - 마스터 노드에 속함

- 컨트롤 플레인 컴포넌트는 클러스터에 관한 전반적인 결정(예를 들어, 스케줄링)을 수행
- 클러스터 이벤트(예를 들어, 디플로이먼트의 `replicas` 필드에 대한 요구 조건이 충족되지 않을 경우 새로운 파드를 구동시키는 것)를 감지하고 반응
- 컨트롤 플레인 컴포넌트는 클러스터 내 어떠한 머신에서든지 동작할 수 있음
	- 그러나 간결성을 위하여, 구성 스크립트는 보통 동일 머신 상에 모든 컨트롤 플레인 컴포넌트를 구동시키고, 사용자 컨테이너는 해당 머신 상에 동작시키지 않음

#### kube-apiserver
- API 서버는 쿠버네티스 API를 노출하는 쿠버네티스 [컨트롤 플레인](https://kubernetes.io/ko/docs/reference/glossary/?all=true#term-control-plane) 컴포넌트
- API 서버는 쿠버네티스 컨트롤 플레인의 프론트 엔드
- 쿠버네티스 API 서버의 주요 구현은 [kube-apiserver](https://kubernetes.io/docs/reference/generated/kube-apiserver/) 
- kube-apiserver는 수평으로 확장되도록 디자인됨
	- 즉, 더 많은 인스턴스를 배포해서 확장할 수 있음
	- 여러 kube-apiserver 인스턴스를 실행하고, 인스턴스간의 트래픽을 균형있게 조절할 수 있음

#### etcd
- 모든 클러스터 데이터를 담는 쿠버네티스 뒷단의 저장소로 사용되는 일관성·고가용성 키-값 저장소
- 쿠버네티스 클러스터에서 etcd를 뒷단의 저장소로 사용한다면, 이 데이터를 [백업](https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#backing-up-an-etcd-cluster)하는 것은 필수임

#### kube-scheduler
- [노드](https://kubernetes.io/ko/docs/concepts/architecture/nodes/)가 배정되지 않은 새로 생성된 [파드](https://kubernetes.io/ko/docs/concepts/workloads/pods/) 를 감지하고, 실행할 노드를 선택하는 컨트롤 플레인 컴포넌트
- 스케줄링 결정을 위해서 고려되는 요소
	- 리소스에 대한 개별 및 총체적 요구 사항
	- 하드웨어 / 소프트웨어 / 정책적 제약
	- 어피니티(affinity) 및 안티-어피니티(anti-affinity) 명세
	- 데이터 지역성
	- 워크로드-간 간섭
	- 데드라인

#### kube-controller-manager
- [컨트롤러](https://kubernetes.io/ko/docs/concepts/architecture/controller/) 프로세스를 실행하는 컨트롤 플레인 컴포넌트
- 논리적으로 각 [컨트롤러](https://kubernetes.io/ko/docs/concepts/architecture/controller/)는 분리된 프로세스이지만, 복잡성을 낮추기 위해 모두 단일 바이너리로 컴파일되고 단일 프로세스 내에서 실행됨
- 컨트롤러는 다음을 포함
	- 노드 컨트롤러: 노드가 다운되었을 때 통지와 대응에 관한 책임을 가짐
	- 잡 컨트롤러: 일회성 작업을 나타내는 잡 오브젝트를 감시한 다음, 해당 작업을 완료할 때까지 동작하는 파드를 생성
	- 엔드포인트 슬라이스 컨트롤러: (서비스와 파드 사이의 연결고리를 제공하기 위해) 엔드포인트 슬라이스(EndpointSlice) 오브젝트를 채움
	- 서비스 어카운트 컨트롤러: 새로운 네임스페이스에 대한 기본 서비스 어카운트(ServiceAccount)를 생성

#### cloud-controller-manager
- 클라우드별 컨트롤 로직을 포함하는 쿠버네티스 [컨트롤 플레인](https://kubernetes.io/ko/docs/reference/glossary/?all=true#term-control-plane) 컴포넌트
- 클라우드 컨트롤러 매니저를 통해 클러스터를 클라우드 공급자의 API에 연결하고, 해당 클라우드 플랫폼과 상호 작용하는 컴포넌트와 클러스터와만 상호 작용하는 컴포넌트를 구분할 수 있게 해 줌
- cloud-controller-manager는 클라우드 제공자 전용 컨트롤러만 실행
	- 자신의 사내 또는 PC 내부의 학습 환경에서 쿠버네티스를 실행 중인 경우 클러스터에는 클라우드 컨트롤러 매니저가 없음
- kube-controller-manager와 마찬가지로 cloud-controller-manager는 논리적으로 독립적인 여러 컨트롤 루프를 단일 프로세스로 실행하는 단일 바이너리로 결합
- 수평으로 확장(두 개 이상의 복제 실행)해서 성능을 향상시키거나 장애를 견딜 수 있음
- 다음 컨트롤러들은 클라우드 제공 사업자의 의존성을 가질 수 있음
	- 노드 컨트롤러: 노드가 응답을 멈춘 후 클라우드 상에서 삭제되었는지 판별하기 위해 클라우드 제공 사업자에게 확인하는 것
	- 라우트 컨트롤러: 기본 클라우드 인프라에 경로를 구성하는 것
	- 서비스 컨트롤러: 클라우드 제공 사업자 로드밸런서를 생성, 업데이트 그리고 삭제하는 것

### 노드 컴포넌트

- 노드 컴포넌트는 동작 중인 파드를 유지시키고 쿠버네티스 런타임 환경을 제공하며, 모든 노드 상에서 동작

#### kubelet
- 클러스터의 각 [노드](https://kubernetes.io/ko/docs/concepts/architecture/nodes/)에서 실행되는 에이전트
- Kubelet은 [파드](https://kubernetes.io/ko/docs/concepts/workloads/pods/)에서 [컨테이너](https://kubernetes.io/ko/docs/concepts/containers/)가 확실하게 동작하도록 관리
- Kubelet은 다양한 메커니즘을 통해 제공된 파드 스펙(PodSpec)의 집합을 받아서 컨테이너가 해당 파드 스펙에 따라 건강하게 동작하는 것을 확실히 함
- Kubelet은 쿠버네티스를 통해 생성되지 않는 컨테이너는 관리하지 않음

#### kube-proxy
- kube-proxy는 클러스터의 각 [노드](https://kubernetes.io/ko/docs/concepts/architecture/nodes/)에서 실행되는 네트워크 프록시로, 쿠버네티스의 [서비스](https://kubernetes.io/ko/docs/concepts/services-networking/service/) 개념의 구현부
- [kube-proxy](https://kubernetes.io/ko/docs/reference/command-line-tools-reference/kube-proxy/)는 노드의 네트워크 규칙을 유지 관리
- 이 네트워크 규칙이 내부 네트워크 세션이나 클러스터 바깥에서 파드로 네트워크 통신을 할 수 있도록 해줌
- kube-proxy는 운영 체제에 가용한 패킷 필터링 계층이 있는 경우, 이를 사용
	- 그렇지 않으면, kube-proxy는 트래픽 자체를 포워드

#### 컨테이너 런타임
- 컨테이너 런타임은 컨테이너 실행을 담당하는 소프트웨어
- 쿠버네티스는 [containerd](https://containerd.io/docs/), [CRI-O](https://cri-o.io/#what-is-cri-o)와 같은 컨테이너 런타임 및 모든 [Kubernetes CRI (컨테이너 런타임 인터페이스)](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-node/container-runtime-interface.md) 구현체를 지원

### 애드온

- 애드온은 쿠버네티스 리소스([데몬셋](https://kubernetes.io/ko/docs/concepts/workloads/controllers/daemonset), [디플로이먼트](https://kubernetes.io/ko/docs/concepts/workloads/controllers/deployment/) 등)를 이용하여 클러스터 기능을 구현
- 이들은 클러스터 단위의 기능을 제공하기 때문에 애드온에 대한 네임스페이스 리소스는 `kube-system` 네임스페이스에 속함

#### DNS
- 여타 애드온들이 절대적으로 요구되지 않지만, 많은 예시에서 필요로 하기 때문에 모든 쿠버네티스 클러스터는 [클러스터 DNS](https://kubernetes.io/ko/docs/concepts/services-networking/dns-pod-service/)를 갖추어야만 함
- 클러스터 DNS는 구성환경 내 다른 DNS 서버와 더불어, 쿠버네티스 서비스를 위해 DNS 레코드를 제공해주는 DNS 서버
- 쿠버네티스에 의해 구동되는 컨테이너는 DNS 검색에서 이 DNS 서버를 자동으로 포함

#### 웹 UI (대시보드)
- [대시보드](https://kubernetes.io/ko/docs/tasks/access-application-cluster/web-ui-dashboard/)는 쿠버네티스 클러스터를 위한 범용의 웹 기반 UI
- 사용자가 클러스터 자체뿐만 아니라, 클러스터에서 동작하는 애플리케이션에 대한 관리와 문제 해결을 할 수 있도록 해줌

#### 컨테이너 리소스 모니터링
- [컨테이너 리소스 모니터링](https://kubernetes.io/ko/docs/tasks/debug/debug-cluster/resource-usage-monitoring/)은 중앙 데이터베이스 내의 컨테이너들에 대한 포괄적인 시계열 매트릭스를 기록하고 그 데이터를 열람하기 위한 UI를 제공해줌

#### 클러스터-레벨 로깅
- [클러스터-레벨 로깅](https://kubernetes.io/ko/docs/concepts/cluster-administration/logging/) 메커니즘은 검색/열람 인터페이스와 함께 중앙 로그 저장소에 컨테이너 로그를 저장하는 책임을 짐

## Cluster

- **쿠버네티스 내 가장 큰 단위**로, **가상 서버들이 속한 클라우드**를 의미
- 쿠버네티스에서 서버는 노드라는 단위로 불리므로, 클러스터란 **마스터노드와 워커노드를 합친 것**이라고 해석할 수도 있음

## Nodes

- 노드 클러스터 내 가상 서버 즉, **컴퓨팅 엔진 단위**
- 클러스터 다음으로 큰 단위이며, **마스터 노드**와 **워커 노드**로 분리되어 있음
- **마스터 노드**
	- 전체 쿠버네티스 시스템을 관리 및 통제하는 쿠버네티스 컨트롤 플레인을 관장
- **워커 노드**
	- 배포하고자 하는 어플리케이션의 실제 실행을 수행
- 마스터 노드가 죽으면 클러스터를 관리할 노드가 없기에, 일반적으로 3개 정도의 마스터 노드를 띄워 관리하며, 워커 노드도 여러 개 구성할 수 있음

## Pods

- 파드란 **쿠버네티스에서 생성하고 관리할 수 있는 배포 가능한 가장 작은 컴퓨팅 단위**
- 그리고 이와 동시에, **하나 이상의 컨테이너 그룹**을 뜻함

### Pods vs Containers

- 컨테이너는 애플리케이션을 말함
	- 이는 완전한 어플리케이션의 일부 기능일 수도 있고, 완전한 어플리케이션 그 자체일 수 있음
- 만약 컨테이너가 완전한 어플리케이션의 일부 기능이라면
    - 여러 개의 컨테이너들이 모여 하나의 완전한 어플리케이션을 구성할 것
    - 이때 여러 개의 컨테이너가 모여 하나의 파드가 됨
	    - 그럼 이 파드가 완전한 어플리케이션이 됨
- 만약 컨테이너 1개가 완전한 어플리케이션 기능을 갖추고 있다면
    - 하나의 컨테이너로도 하나의 파드를 구성할 수 있음
- 즉, 파드에 정의된 여러 개의 컨테이너는 하나의 완전한 애플리케이션으로서 동작

## Services

- 쿠버네티스 환경에서 서비스(Service)는 **파드들을 통해 실행되고 있는 애플리케이션을 네트워크에 노출(expose)시키는 가상의 컴포넌트**
- 쿠버네티스 내부의 다양한 객체들이 애플리케이션과, 그리고 애플리케이션이 다른 외부의 애플리케이션이나 사용자와 연결될 수 있도록 도와주는 역할

## kubectl

- 쿠버네티스 클러스터를 관리하는 명령줄 도구 (CLI)
- 로컬 환경, 매니지드 환경 모두 사용 가능
- 쿠버네티스 자원을 생성, 업데이트, 삭제 (create, update, delete)
- 디버그, 모니터링, 트러블 슈팅 (log, exec, cp, top, attach, ...)
- 클러스터 관리 (cordon, top, drain, taint, ...)

### 설치

> https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/

### 사용법

> https://kubernetes.io/docs/reference/kubectl/conventions/

> https://kubernetes.io/docs/reference/kubectl/cheatsheet/

```bash
kubectl [COMMAND] [TYPE] [NAME] [FLAG]
```

#### COMMAND
- 하나 이상의 리소스를 조작하기 위한 명령어
- 예: create, get, describe, delete

#### TYPE
- 리소스 타입
- 대소문자를 구분, 단수, 복수, 단축형으로 명시 가능

#### NAME
- 리소스 이름
- 대소문자를 구분
- kubectl get nodes 와 같이 리소스 이름을 생략하면 모든 리소스 정보를 출력

#### FLAG
- 부가적으로 설정하는 옵션

