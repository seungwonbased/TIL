# 한끼얼마 K8s Cluster 구축 Report

## 1. 클러스터 구축 전 도커 이미지 빌드
### 1.1. Redis Database
#### 1.1.1. Dockerfile

```dockerfile
FROM redis:alpine
COPY ./redis.conf /usr/local/etc/redis/redis.conf
CMD [ "redis-server", "/usr/local/etc/redis/redis.conf" ]
```

- Redis 초기 설정을 위한 .conf 파일을 기본 이미지에 복사 후 적용

#### 1.1.2. redis.conf

```
bind 0.0.0.0
```

- 기본 설정은 호스트 자신에게만 바인딩되어 있기 때문에 0.0.0.0에 바인딩

### 1.2. Flask BE Application
#### 1.2.1. Dockerfile

```dockerfile
FROM ubuntu:20.04  
  
WORKDIR /rest-recipe-book  
  
COPY . .  
  
RUN apt-get update -y  
  
RUN apt-get install -y python3  
RUN apt-get install -y python3-pip  
  
RUN pip install wheel  
RUN pip install -r requirements.txt  
  
ENV FLASK_APP=app  
ENV FLASK_DEBUG=false
ENV APP_CONFIG_FILE=/rest-recipe-book/config/production.py  

RUN chmod +x app.sh  

CMD ./app.sh
```

- Production 환경에 맞는 환경변수 설정
- 컨테이너 실행 시 애플리케이션 실행을 위한 초기 스크립트 실행

#### 1.2.2. app.sh

```shell
#!/bin/bash
flask db init  
flask db migrate  
flask db upgrade  
  
gunicorn --bind 0.0.0.0:5000 --timeout 90 "app:create_app()"
```

- Flask ORM을 초기화하고 DB와 동기화하는 스크립트
- Gunicorn WSGI로 WAS를 실행하는 스크립트

### 1.3. React FE Application
#### 1.3.1. Dockerfile

```dockerfile
FROM node AS builder
RUN mkdir /recipe
WORKDIR /recipe
COPY . .
RUN npm install
RUN npm run build

FROM nginx AS runtime
COPY --from=builder /recipe/build /usr/share/nginx/html/
CMD ["nginx", "-g", "daemon off;"]
```

- 다단계 도커 빌드
- 리액트를 빌드하고 빌드한 결과를 Nginx에 넣어 서빙 가능하게 함

## 2. 한끼얼마 3-Tier K8s Cluster

![k8s](https://github.com/seungwonbased/TIL/blob/main/assets/k1.png)

### 2.1. 기본 구성

#### 2.1.1. Nodes
- 한 개의 Master Node
	- 클러스터의 Control Plane을 구성
	- API Server, Scheduler, Control Manager, etcd 포함
	- 본 프로젝트에서는 Master Node에서 Kubectl 명령을 통해 쿠버네티스 클러스터 작업 수행
- 두 개의 Worker Node
	- 컨테이너가 실행되고 관리되는 머신
	- Container Runtime, Kubelet, Kube Proxy 포함

#### 2.1.2. Namespace
- Default 네임스페이스

#### 2.1.3. Load Balancer
- Metal LB Load Balancer
- 쿠버네티스 클러스터 내에서 로드 밸런싱을 제공하는 오픈 소스
- 서비스의 외부 노출을 관리하고, 서비스가 클러스터 외부에서 접근될 때 트래픽을 로드 밸런싱하여 여러 백엔드 Pod으로 분배
- 클라우드 프로바이더에 종속되지 않고 온프레미스나 다른 환경에서도 사용 가능
- 클러스터에 Metal LB 설치 후 routing-config.yaml 파일을 적용해 설정
- routing-config.yaml

```yaml
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: address-pool
  namespace: metallb-system
spec:
  addresses:
  - 10.0.0.3-10.0.0.254
---
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: recipe-lb
  namespace: metallb-system
spec:
  ipAddressPools:
  - address-pool
```

#### 2.1.4. Bare Metal Database Server
- 클러스터 외부 물리 서버에 PostgreSQL 데이터베이스 서버를 실행시켜 놓음
- 데이터의 안정성을 위해 외부에 DB 서버를 구성했으며, 메인 데이터를 영구적으로 저장

### 2.2. Database Tier: Redis

- 데이터 관리와 데이터 저장을 처리
- 빠른 데이터 처리를 위해 인메모리 데이터베이스인 Redis 선택

#### 2.2.1. redis.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-depolyment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: redis-container
        image: seungwonbae/recipe-redis:1.0
        ports:
        - containerPort: 6379
---
apiVersion: v1
kind: Service
metadata:
  name: redis
  labels:
    app: redis
spec:
  selector:
    app: redis
  ports:
  - name: redis
    protocol: TCP
    port: 6379
    targetPort: 6379
  type: ClusterIP
```

- Redis 데이터베이스는 외부에서 접근할 필요가 없기 때문에 내부에서 접근할 Cluster IP 타입의 서비스를 생성

#### 2.2.2. Config Map에 Redis Cluster IP 저장
- Redis 서비스의 Cluster IP를 Config Map에 저장
- Cluster IP만 출력하는 스크립트 작성
```shell
kubectl create configmap backend --from-literal=REDIS_HOST=$(kubectl get svc redis-service -o custom-columns="CLUSTER-IP:.spec.clusterIP" --no-headers | tail -n 1)
```

### 2.3. Application Tier (Backend Layer): Flask, Gunicorn

- 비즈니스 로직을 처리하고 사용자의 요청에 대한 응답을 생성
- 사용자 요청을 처리하고 데이터 처리, 검증, 계산 등을 실행
- Flask에 Gnicorn을 붙인 Web Application Server가 있음

#### 2.3.1. flask-backend-deployment-service.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: rest-flask-backend-deployment
spec:
  selector:
    matchLabels:
      app: rest-flask-backend-app
  replicas: 5
  template:
    metadata:
      labels:
        app: rest-flask-backend-app
    spec:
      containers:
      - name: flask
        image: seungwonbae/rest-flask-backend:2.2
        env:
          - name: REDIS_HOST
            valueFrom:
              configMapKeyRef:
                name: backend
                key: REDIS_HOST
        ports:
        - containerPort: 5000
          protocol: TCP
---
apiVersion: v1
kind: Service
metadata:
  name: backend-lb
spec:
  selector:
    app: rest-flask-backend-app
  ports:
  - protocol: TCP
    port: 5000
    targetPort: 5000
  type: LoadBalancer
  loadBalancerIP: 10.0.0.3
```

- Deployment
	- WAS Replica 5개를 Deployment로 배포
	- Config Map에 저장해놓은 REDIS_HOST의 IP를 환경변수로 넣어줌
		- WAS가 Redis에 종속적임
	- Gunicorn WSGI를 5000번 포트에 바인딩 시켜놓았으므로 해당 포트를 열어줌
- Service
	- External IP를 얻기 위해 Load Balancer 서비스를 붙임
		- 외부에서 사용자와 Axios & Fetch 통신 목적
	- Load Balancer의 External IP를 10.0.0.3으로 고정
		- Frontend Layer와의 종속성을 없애기 위한 목적

### 2.4. Presentation Tier (Frontend Layer): React, Nginx

- 사용자 인터페이스와 관련된 정적 파일 및 클라이언트 측 코드를 처리하며, 사용자의 브라우저로 전달

#### 2.4.1. react-frontend-deployment-service.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: react-frontend-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: react-frontend-app
  template:
    metadata:
      labels:
        app: react-frontend-app
    spec:
      containers:
      - name: react
        image: seungwonbae/react-frontend:2.0
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: frontend-lb
spec:
  selector:
    app: react-frontend-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: LoadBalancer
  loadBalancerIP: 10.0.0.4
```

- External IP를 얻기 위해 Load Balancer 서비스를 붙임
	- 외부 사용자가 Nginx에 빌드된 리액트 파일 서빙을 요청하기 위한 목적
- Load Balancer의 External IP를 10.0.0.4로 고정
	- API 서버의 CORS 정책의 Origin 허용을 10.0.0.4에 설정해놓았기 때문

## 3. 결과

### 3.1. Pod

![k8s](https://github.com/seungwonbased/TIL/blob/main/assets/k2.png)

- Deployment로 생성한 Pod의 레플리카가 Worker Node에 배포됨

### 3.2. Replica Set, Deployment

![k8s](https://github.com/seungwonbased/TIL/blob/main/assets/k3.png)

![k8s](https://github.com/seungwonbased/TIL/blob/main/assets/k4.png)

### 3.3. Service

![k8s](https://github.com/seungwonbased/TIL/blob/main/assets/k5.png)

- Backend Layer, Frontend Layer에 설정한 Externel IP로 설정됨

### 3.4. Config Map, Namespace

![k8s](https://github.com/seungwonbased/TIL/blob/main/assets/k6.png)

![k8s](https://github.com/seungwonbased/TIL/blob/main/assets/k7.png)

- REDIS_HOST가 설정된 backend Config Map이 생성됨
- Deployment가 배포되는 default 네임스페이스, Metal LB의 구성요소를 포함하는 metallb-system 네임스페이스가 생성됨

### 3.4. Frontend External IP로 접속

![k8s](https://github.com/seungwonbased/TIL/blob/main/assets/k8.png)

- 설정한 10.0.0.4 IP로 접속 성공

### 3.5. 로그인 요청 - CORS Preflight 요청

![k8s](https://github.com/seungwonbased/TIL/blob/main/assets/k9.png)

- CORS Simple Request 조건(Content-Type Header)이 맞지 않아 브라우저가 자동으로 Preflight 요청 전송
- 서버 애플리케이션에 허용 Origin을 10.0.0.4로 설정해두었기 때문에 Access-Control-Allow-Origin 헤더가 응답으로 도착하여 브라우저가 허용 여부를 결정

### 3.6. 로그인 요청 - 실제 요청

![k8s](https://github.com/seungwonbased/TIL/blob/main/assets/k10.png)

- Preflight 요청 이후 실제 요청을 보냄
- 응답으로 토큰값이 각각 쿠키, Body로 넘어옴

### 3.7. 로그인 후 Recoil에 상태 (토큰, 로그인 상태) 저장 확인

![k8s](https://github.com/seungwonbased/TIL/blob/main/assets/k11.png)

- Local Storage의 recoil-persistent에 토큰값과 상태가 저장되었음
