# K8s YAML 설정 파일
## apiVersion, kind
### `apiVersion`

- 스크립트를 실행하기 위한 쿠버네티스 API 버전
- 존재하지 않는 값을 설정하면 오류 발생

#### API Object의 종류 및 버전

- Deployment : apps/v1
- Pod : v1
- ReplicaSet : apps/v1
- ReplicationController : v1
- Service : v1
- PersistentVolume : v1
- kubectl explane [오브젝트명]으로 오브젝트의 apiVersion을 확인 가능
	- i.g., kubectl explane pod

### `kind`

- 쿠버네티스 리소스의 종류 정의
- Pod, ReplicaSet, Service, Deployment 등

## 설정 파일의 세 부분

```yaml
# nginx-deployment.yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  labels:
spec:
  replicas: 2
  selector:
  ports:
  templates:
```

### `metadata`

- 레플리카셋의 이름이나 Label과 같은 메타데이터
- 컴포넌트를 서로 구분 지어줄 수 있는 **이름과 설명**을 입력할 수 있음

### `spec`

- kind에 적힌 컴포넌트에 대한 구체적인 내용을 정의하는 **spec** 부분
- spec에 대한 포맷은 쿠버네티스 컴포넌트 종류마다 다름

### `status`

- **status**는 자동적으로 생성되고, 쿠버네티스에 자동 등록됨
- **Desired State(목표 상태)** 와 **Actual State(실제 상태)** 로 구분될 수 있음
- etcd를 확인해보면 자동으로 생성된 것을 볼 수 있음

## Pod의 청사진

- Deployment는 Pod를 생성하고 관리
- Pod에 관한 부분은 spec의 template 부분에서 선언
- 또한 template은 자신만의 metadata와 spec을 갖고 있음
	- 어떤 이미지로 생성할 지, 어떤 포트를 오픈할 지

## 컴포넌트 간 연결

- Service와 Deployment, Deployment와 Pod의 연결을 구성하는 부분은 labels와 selector
- labels는 metadata에 속해 있으며 selector는 spec에 속해 있음

### Deployment와 Pod의 연결

```yaml
# 기본 항목
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    environment: production
    app: nginx
# Deployment 스펙
spec:
  replicas: 2
  selector:
    matchLables:
      app: nginx
  # Pod 템플릿
  template:
    metadata:
      labels:
        app: nginx
    # Pod 스펙
    spec:
      containers:
      - name: nginx
        image: nginx:1.16
        # 이하 생략
```

- metadata.labels의 app: nginx
	- nginx-deployment가 app: nginx라는 이름의 Key-Value 쌍으로 관리된다는 것
	- 여기서 app: nginx는 Deployment 자체를 식별하는 데 사용됨
		- Deployment 리소스에 레이블을 할당하는 것
		- 다른 리소스가 해당 Deployment를 식별하거나 참조할 때 이 레이블을 활용할 수 있음
- spec.selector의 app: nginx
	- Deployment가 어떤 Pod를 생성하고 관리해주는 지 명시
	- 여기서는 Deployment가 app: nginx 레이블을 가진 Pod을 관리하게 됨
- spec.template의 app: nginx
	- Deployment가 관리하는 Pod를 생성할 때 사용됨
	- 새로운 Pod가 만들어질 때, 해당 Pod에 app: nginx 레이블이 자동으로 할당됨
	- Pod가 Deployment에 속한 것임을 나타내며 Deployment가 Pod을 관리하는 데 사용됨

### Service와 Deployment의 연결

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: LoadBalancer
  selector:
    app: nginx
  ports:
    - port: 80
    - targetPort: 80
  # 이하 생략
```

- Service의 spec.selector에 있는 **app: nginx**는 **자신이 관리할 Deployment 또는 Pod를 나타내는 의미로 사용됨**
