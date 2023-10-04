# Kubernetes Resources

- 쿠버네티스 오브젝트를 생성할 때 사용하는 틀
- 쿠버네티스 리소스 리스트는 "kubectl api-resources"로 확인할 수 있음

## 주요 리소스
### 컨테이너 관련 리소스

- node
	- 컨테이너를 배치하는 서버
- namespace
	- 쿠버네티스 클러스터 안의 가상 클러스터 
- pod
	- 컨테이너 집합 중 가장 작은 단위로 컨테이너의 실행 방법을 정의 
- replicaset
	- 같은 스펙을 갖는 파드를 여러 개 생성하고 관리하는 역할
- deployment
	- replicaset의 리비전을 관리

### 네트워크 관련 리소스

- service
	- 파드의 집합에 접근하기 위한 경로를 정의 
- ingress
	- 서비스를 쿠버네티스 클러스 외부로 노출

### 정보 저장 관련 리소스 

- configmap
	- 설정 정보를 정의하고 파드에 전달
- secret
	- 인증 정보와 같은 기밀 데이터를 정의

### 데이터 저장 관련 리소스

- persistentvolume
	- 파드가 사용할 스토리지의 크기 및 종류를 정의
- persistentvolumeclaim
	- 퍼시스턴트 볼륨을 동적으로 확보

### 배치 잡 관련 리소스

- job
	- 상주 실행을 목적으로 하지 않는 파드를 여러 개 생성하고 정상적인 종료를 보장
- cronjob
	- cron 문법으로 스케줄링되는 job

# Kubernetes Objects

- 오브젝트는 지정된 상태가 유지되도록 쿠버네티스에 의해 제어됨
- 오브젝트는 Spec과 Status라는 필드를 가짐
	- Spec: 사용자가 기대하는 상태
	- Status: 기대되는 값에 대비한 현재의 상태
- 오브젝트는 메타데이터에 기술된 이름에 의해 식별됨
	- 메타데이터는 주로 yaml 파일로 기술됨
- 같은 종류의 오브젝트 이름은 하나의 네임스페이스에서 반드시 유일해야 함

## 기본 오브젝트

- 쿠버네티스에 의해 배포 및 관리되는 가장 기본적인 오브젝트
- Pod, Service, Volume, Namespace 네 개가 있음

### Pod

- **Pod는 쿠버네티스에서 컨테이너의 기본 단위**로, **가장 기본적인 배포 단위**
- **Pod는** **1개 이상의 컨테이너로 구성된 컨테이너의 집합**
- 다음과 같은 특징을 지니고 있음
	- **각 Pod에는 고유한 IP 주소가 할당**됨
	- **같은 Pod내의 컨테이너는 IP와 Port 공간을 공유**
		- (정확하게는 네트워크 네임스페이스를 공유하며 여기에 IP와 Port가 포함됨)
	- **따라서 파드에 속한 컨테이너는 localhost를 사용하여 서로 통신**할 수 있음
	- **Pod내의 컨테이너간에는 디스크 볼륨을 공유**할 수 있음
		- (파일과 같은 데이터 공유 가능)

### Volume

- Pod가 생성될 때 컨테이너가 실행되면서 로컬 디스크를 생성
	- 이 로컬 디스크의 경우 컨테이너가 재실행되거나 종료되면 로컬 디스크안에 있는 파일도 같이 손실된다는 단점이 있음
- 따라서 파일을 영구적으로 저장해야할 필요가 스토리지가 필요
	- 쿠버네티스는 이를 위해 **영구적인 스토리지**를 제공하는데 이를 **볼륨**이라고 함
- **볼륨은 Pod가 실행될 때 컨테이너에 마운트되어 사용되며 쉽게 컨테이너의 외장 디스크**라고 생각하면 됨
- 볼륨은 **NFS,  Cinder, CephFS** 등과 같은 **네트워크 스토리지**부터  **AWS EBS(Elastic Block Store), Google PD(Persistent Disk) , Azure Disk Storage** 등과 같은 **클라우드 스토리지**까지 **다양한 유형의 볼륨들을 지원**
- **유형에 따라 볼륨이 Pod 내부에 존재할 수도 있고 Pod 외부에 존재**할 수도 있음

### Service

- 클러스터 내부에서 실행되는 Pod들은 언제든지 삭제됬다 생성될 수 있는 반 영속적인 특성을 지니고 있음
	- Pod가 생성될 때마다 새로운 내부 IP를 할당하게 되므로, 클러스터 내/외부와 통신을 계속 유지하기 어려움
- 이를 위해 쿠버네티스에서는 **Service**라는 기능을 제공
	- 이는 **라벨링을 통해  같은 라벨을 가진 Pod를 묶어 단일 엔드포인트(ClusterIP, NodePort, LoadBalancer, ExternalName)를 제공**
 - **Service는 클러스터 내부에서 고정적인 IP를 갖고 있으며**, **Pod가 변경되어도 서비스의 IP는 변하지 않기 때문에 클러스터 내/외부와의 통신을 계속 유지**할 수 있음
- 또한 **Service는 Pod간의 로드밸런싱을 지원**하고, **멀티 포트도 지원**

### Namespace

- **Namespace는 쿠버네티스 클러스터 내의 리소스들을 논리적으로 구분할 수 있게 해주는 하나의 단위**
- 예시
	- 모니터링을 위한 리소스는 **monitoring**이라는 Namespace에서 생성할 수 있고, 테스트를 위한 리소스들은 **test**라는 Namespace에서 생성할 수 있음
	- 여러 팀에서 하나의 쿠버네티스 클러스터를 공유해 사용해야 한다면 팀별로 Namespace를 사용하도록 구성할 수도 있음
- Namespace의 또 다른 사용방법은 **Namespace별로 리소스(CPU, GPU, Memory 등등..)와 **접근 권한을 부여**할 수도 있음
- 다만 **Namespace는 논리적으로 구분해놓은 것이지 물리적으로 구분해놓은것이 아니기 때문에 다른 Namespace간의 Pod라도 통신은 가능**
	- 네트워크 정책을 통해 Namespace간의 통신을 막을 수는 있지만 이경우에는 그냥 클러스터 자체를 분리시키는 것이 바람직

## Controller

- 컨트롤러: 기본 오브젝트를 관리하며 추가적인 기능을 가진 오브젝트
- Pod의 실행을 제어하는 오브젝트
	- ReplicaSet, Deployment, StatefulSet, DaemonSet 등의 컨트롤러를 통해 Pod의 실행을 제어

### ReplicationController

- **ReplicationController(RC)는 Pod를 관리해주는 역할**
- 이제 사용 잘 안 함 -> ReplicaSet 사용
- ReplicationController에 **지정된 Replica 수보다 Pod이 많으면 Pod을 삭제하거나 Pod이 적으면 Pod을 추가**
- 사용자가 수동으로 생성한 Pod과 달리 **ReplicationController가 관리하는 Pod의 경우 Pod가 삭제되거나 예기치 않게 종료되는 경우 Pod를 자동으로 교체**
- **ReplicationController은 크게 3가지로 구성됨**
	- **Selector**
		- **Selector는 라벨을 기반**으로하며, **라벨을 지정하면 해당 라벨을 가진 모든 Pod를 관리**
		- **ReplicationController는 등호(Eqaulity) 기반의 Selector를 사용**
		- **따라서 라벨이 같은지(\==) 다른지(!=)만 비교**
	- **Replica**
		- **ReplicationController에 의해 관리되는 Pod의 수**
		- **Replica가 3이면 3개의 Pod만 띄우고, 만약 지정한 Replica 보다 Pod의 수가 많다면 Pod를 삭제하고 적다면 Pod를 추가**
	- **Template**
		- **새로운 Pod를 추가할 때 해당 Pod에 대한 정보를 정의해야 하는데 이를 Template에다가 정의**
		- 기존의 Pod가 Template에 정의된 스펙과 다를지라도 Replica 수에 맞게 Pod가 생성되어있다면 해당 Pod를 삭제하지 않음

### ReplicaSet

- **ReplicaSet은 ReplicationController의 상위 버전**으로, 공식 문서에서도 **ReplicatonController보다 ReplicaSet을 사용할 것을 권장**
- ReplicaSet은 ReplicationController와 동일한 기능을 하지만 다음과 같은 차이점이 있음
	- **Selector**
		- **ReplicaSet은 집합(Set) 기반의 Selector를 사용**
		- 따라서 **in, notin, exists 같은 연산자를 지원**
	- **rolling-update**
		- **ReplicationController는 rolling-update 옵션을 사용할 수 있지만 ReplicaSet은 사용할 수 없음**
		- ReplicaSet은 **Deployments를 통해 rolling-update를 사용**할 수 있음

### Deployment

- **Deployment는 ReplicaSet의 상위 개념**으로, **Pod와 ReplicaSet에 대한 배포를 관리**
- 운영 중에 어플리케이션의 새 버전을 배포해야하거나 부하가 증가하면서 ReplicaSet을 추가하는 등 여러 가지 동작을 Deployment로 관리할 수 있음
- Deployment는 배포에 대한 이력을 관리하는데 만약 배포한 새 버전의 문제가 생긴 경우 Deployment를 통해 **쉽게 이전 버전으로 롤백**할 수 있음
- 쿠버네티스로 서비스를 운영하는 상황이라면 ReplicaSet만으로 운영하기 보다는  대부분 **Deployment단위로 Pod와 ReplicaSet을 관리하여 운영**
- Deployments에서 자주 사용되는 배포 전략으로는 **Rolling Update, Blue/Green, Canary** 등이 있음

### StatefulSet

- **StatefulSet은 Pod의 상태를 유지하고 관리할 때 사용**
- Deployment를 통해 Pod를 삭제하고 생성하면 완전히 새로운 가상환경이 시작되는데, 이 경우 StatefulSet을 사용하여 Pod를 생성하면 **Pod가 삭제되었다 새로 생성되어도 기존의 상태를 그대로 유지**할 수 있음
- StatefulSet은 Deployment와 비슷하게 동작하지만 Deployment는 Pod의 이름 뒤에 랜덤한 **영문 + 숫자**가 붙고 StatefulSet은 각 Pod에 **고유한 일련번호(0~N)**가 붙는다는 점에서 차이가 있음
	- 정확하게는 영문 + 숫자가 붙는건 ReplicaSet의 특징
- StatefulSet은 **Pod가 추가되면 마지막 번호의 다음 번호를 선택**하고, **Pod가 삭제될 때는 가장 마지막 번호부터 삭제**하기 때문에 **사용자 입장에서는 추가 / 삭제되는 Pod를 쉽게 예측할 수 있다는 장점**이 있음
- 다만 Pod가 삭제(Scale Down)되는 경우 Deployment에 비해 빠르게 작업을 수행할 수 없다는 단점이 있음
	- 그 이유는 StatefulSet은 Deployment처럼 Pod가 삭제되면 Pod안에 있는 정보를 전부 삭제하는 것이 아니라 **상태를 유지하기 위해 Pod가 삭제되었을 때 해당 Pod의 정보를 다른 Pod에 분산하여 저장**하게 되기 때문
- 이러한 특징 때문에 **StatefulSet**은 보통 **DB같이 디스크에 데이터가 유지 되어야 하는 Stateful Application에 많이 사용**되고, **Deployment**는 **웹이나 웹앱(WAS)같은 Stateless Application을 관리할 때 사용**됨
- 또한 **Deployment는 Rolling Update, Blue/Green, Canary 배포 등 다양한 배포를 지원**하지만 **SatetfulSet은 On Delete와, Rolling Update 배포만 지원**한다는 점에서 차이점이 있음

### DaemonSet

- **DaemonSet역시 Deployment와 유사하게 Pod를 생성하고 관리**하지만, **전체 노드(서버)에서 항상 실행되어야 하는 특정 Pod를 실행할 때 사용**
	- 예를 들어 각 노드의 모니터링 환경을 구성하기 위해 노드 마다 로그 수집용 Pod를 실행해야 하는 경우 DaemonSet을 사용하여 Pod를 관리하면 노드가 추가되어도 로그 수집용 Pod가 자동으로 노드에 추가되어 좀 더 쉽게 모니터링 환경을 구축할 수 있음
- **DaemonSet이 관리하는 Pod는 기본적으로 모든 노드에 균등하게 생성**되지만, **특정 노드에만 Pod가 하나씩 생성되도록 구성**할 수도 있음

### Job

- **Job은 하나 이상의 Pod를 지정하고 지정된수의 Pod를 실행**
- **Pod가 정상적으로 실행되지 않았을 경우 Job은 새로운 Pod를 새로 생성하고 실행**
- Job은 **주로 백업이나 배치 작업같은 한번만 실행되고 종료되는 성격의 작업에 사용**됨
- Job은 **재실행 정책(restartPolicy)에 따라 Pod 생성 실패시 수행하는 작업이 달라짐**

### CronJob

- **CronJob은 이름그대로 알 수 있듯이 Job을 주기적으로 실행**
	- **Linux의 crontab과 유사**
- **CronJob은 주로 특정 시간에 실행되거나 일정 주기마다 실행되야하는 작업에 사용**

## 오브젝트 생성 방법

### 생성형 (Generative)

```
kubectl run
또는
kubectl expose
```

- 컨테이너가 원하는 대로 작동하는지 빠르게 확인할 경우 사용
- 주로 개발 환경에서 많이 사용

### 명령형 (Imperative)

```bash
kubectl create
```

- 오브젝트의 버전 관리가 중요할 때 사용 

### 선언형 (Declarative)

```bash
kubectl apply
```

- 오브젝트의 버전 관리가 중요할 때 사용
