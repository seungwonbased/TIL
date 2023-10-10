# Deployment

- Deployment는 Pod와 ReplicaSet에 대한 선언적 업데이트를 제공
- Deployment에서 의도하는 상태를 설명하고, 디플로이먼트 컨트롤러(Controller)는 현재 상태에서 의도하는 상태로 비율을 조정하며 변경

> **참고:** 디플로이먼트가 소유하는 레플리카셋은 관리하지 말아야 함

# The "Deployment" Object

- Deployment 객체는 쿠버네티스로 작업할 때 작업하는 주요 객체 중 하나
	- 일반적으로 수동으로 Pod 객체를 직접 생성하여 특정 워커 노드로 이동하지는 않음
	- 대신 deployment 객체를 생성하고, 생성하고 관리해야 하는 pod의 수와 컨테이너 수에 대한 지침을 제공
- Deployment 객체는 하나 이상의 Pod를 제어할 수 있음
	- 따라서 이를 사용하여, 한 번에 여러 Pod를 생성하여 Deployment 객체의 핵심 철학인 내부적으로 컨트롤러 객체를 생성할 수 있음

## 특징
### 원하는 목표를 설정하면 쿠버네티스가 이를 수행

- 예시
	- 구동중인 컨테이너와 두 개의 포드를 가지고 있다면 쿠버네티스가 이 목표 상태에 도달하기 위해 필요한 모든 작업을 수행
	- 그래서 원하는 것을 정의할 수 있고, 쿠버네티스가 수행
- 수동으로 리모트 머신을 선택하여 거기에 Pod를 배치할 필요가 없음
	- 쿠버네티스가 대신 이 작업을 수행

### Deployment를 일시 중지하거나, 삭제하고, 롤백할 수 있음

- 무언가 엉망이 되어 애플리케이션이 실패하더라도, 코드를 변경하고, 오류를 수정하고, 실패한 Pod를 새 것으로 교체하기 위해 서두르지 않아도 됨
- 대신 실패한 Deployment를 롤백하고 이전에 작동했던 Deployment로 돌아갈 수 있음
- 서두르지 않고 버그를 수정한 뒤 업데이트된 컨테이너 이미지의 업데이트된 코드로 새 Deployment를 시작하기만 하면 됨
- 스케일은 변경되어도 버전이 바뀌지는 않음

### Deployment도 Pod를 스케일링할 수 있음

- 더 많은 Pod 또는 더 적은 Pod를 갖고 싶다고 쿠버네티스에 알릴 수 있으며, 특정 메트릭을 설정할 수 있는 오토 스케일링 기능을 사용할 수도 있음
	- 예를 들어 메트릭엔 수신 트래픽과 CPU 사용률이 있음
- 메트릭을 초과하면, 쿠버네티스가 자동으로 더 많은 Pod를 생성하므로 들어오는 요청을 처리하기 위해 더 많은 실행 중인 컨테이너 인스턴스가 생성됨
	- 트래픽이 저하되어 줄어들면, 불필요한 Pod는 다시 제거됨

### 자동 복구

- Pod 내의 컨테이너가 종료되는 경우
	- Pod는 컨테이너 수준의 장애에 대해 자동 복구를 시도
	- Deployment는 파드 단위로 복구를 시도
- 예시
	- 노드에 장애가 발생하여 종료됨
	- 5~6분 정도 경과하면 배포한 Deployment가 활성화 노드에 대체 Pod를 생성
		- Deployment 말고 Pod를 단독으로 기동하면 원래 노드에서 계속 Terminating 되어 있음
	- 중지되었던 노드가 복원되면, Terminating 상태였던 Pod가 모드 제거됨
	- 노드와의 통신이 회복되어 상태가 불분명했던 Pod의 상태가 확인되어 삭제되는 것으로 단독으로 기동한 파드는 완전히 소멸됨
- 일시적인 장애로 자동 복구가 발동되었을 때 더 불안한 상태로 빠지는 것을 막기 위해서, 디플로이먼트는 천천히 복구를 수행하도록 만들어져 있음

## 정리

- Deployment를 생성할 때, Deployment로 하나의 Pod를 관리하지만 쿠버네티스가 관리하는 여러 포드를 갖기 위해 여러 개의 deployment를 생성할 수도 있음
	- 따라서 일반적으로 pod를 직접 생성하지 않으며, 직접 관리하지도 않음
- 대신, Deployment를 사용하여 Deployment 객체를 생성하고 이를 쿠버네티스 클러스터에 전송하여 쿠버네티스가 이를 수행하도록 함

## Deployment 배포 전략
### Recreate

- 업데이트 시 모든 레플리카를 종료하고 다시 새로운 레플리카를 띄움
- 예시
```yaml
# deployment-recreate.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sample-deployment-recreate
spec:
  strategy:
    type: Recreate
  replicas: 3
  selector:
    matchLabels:
      app: sample-app
  template:
    metadata:
      labels:
        app: sample-app
    spec:
      containers:
      - name: nginx-container
        image: docker.io/nginx:1.16
      imagePullSecrets:
      - name: regcred
```
- 업데이트 시 기존 RS의 레플리카 수를 0으로 하고 리소스를 반환
- 신규 RS를 생성하고 레플리카의 수를 늘림

### Rolling Update

- 여러 개의 가동 중인 Pod를 점진적으로 업데이트하는 방법
- 예시
```yaml
# deployment-rollingupdate.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sample-deployment-rollingupdate
spec:
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 0
      maxSurge: 1
  replicas: 3
  selector:
    matchLabels:
      app: sample-app
  template:
    metadata:
      labels:
        app: sample-app
    spec:
      containers:
      - name: nginx-container
        image: docker.io/nginx:1.16
      imagePullSecrets:
      - name: regcred
```
- maxUnavailable: 업데이트 중 동시에 정지 가능한 최대 Pod
- maxSurge: 업데이트 중 동시에 생성할 수 있는 최대 Pod
- maxUnavailable과 maxSurge를 모두 0으로 설정할 수는 없음