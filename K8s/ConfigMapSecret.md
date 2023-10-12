# Config Map & Secret

![cm](https://github.com/seungwonbased/TIL/blob/main/K8s/assets/cm1.png)

- 설정값을 Pod에 전달
- 애플리케이션의 설정 정보나 패스워드와 같은 인증 정보는 컨테이너에 담지 말고 분리해서 네임스페이스에 저장하고 컨테이너가 읽도록 하면 테스트 환경, 프로덕션 환경별로 빌드할 필요가 없음
- 컨테이너에스는 Config Map 또는 Secret을 파일 시스템으로 마운트해서 읽거나 환경 변수로 참조할 수 있음

## Config Map

- 일반적인 설정 정보를 담아 저장할 수 있는 쿠버네티스 오브젝트
- **네임스페이스에 속하기 때문에 네임스페이스 별로 Config Map이 존재**
- 네임스페이스에 저장한 설정 정보
- 노출되어도 큰 문제가 없는 정보

### Config Map 생성

```bash
kubectl create configmap [Config Map 이름] [저장될 값을 Key=Value 형태로] ...
```

```bash
kubectl create configmap log-level-configmap --from-literal LOG_LEVEL=DEBUG --from-literal ...
```

### Config Map 조회

```bash
# 원시 형태 조회
kubectl describe configmap log-level-configmap
```

```bash
# YAML 형태 조회
kubectl get configmap log-level-configmap -o yaml
```

### Pod에서 Config Map의 값을 사용하는 방법
#### 1. Config Map의 값을 Pod의 환경 변수로 사용
- 1.1. Config Map에 정의된 모든 Key=Value 쌍을 Pod의 환경 변수로 설정
- 예시
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: cm-to-env-all
spec:
  containers:
  - name: my-container
    image: docker.io/busybox
    args: ['tail', '-f', '/dev/null']  # 컨테이너가 아무 작업도 하지 않고 대기
    envFrom:
    - configMapRef:
        name: log-level-configmap
    - configMapRef:
        name: config-k8s
```
- 결과
```bash
vagrant@master-node:~$ kubectl exec cm-to-env-all -- env
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
TERM=xterm
HOSTNAME=cm-to-env-all
LOG_LEVEL=DEBUG
container=docker
k8s=kubernetes
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_ADDR=172.17.0.1
KUBERNETES_SERVICE_HOST=172.17.0.1
KUBERNETES_SERVICE_PORT=443
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_PORT=tcp://172.17.0.1:443
KUBERNETES_PORT_443_TCP=tcp://172.17.0.1:443
KUBERNETES_PORT_443_TCP_PROTO=tcp
HOME=/root
```
- Config Map의 모든 Key=Value가 컨테이너의 환경 변수로 설정됨

- 1.2. Config Map에 정의된 Key 이름 대신 별칭으로 Pod의 환경 변수로 설정
- 예시
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: cm-to-env-selective
spec:
  containers:
  - name: my-container
    image: docker.io/busybox
    args: ['tail', '-f', '/dev/null']
    env:
    - name: NEW_LOG_LEVEL
      valueFrom:
        configMapKeyRef:
          name: log-level-configmap
          key: LOG_LEVEL
    - name: NEW_CONTAINER            
      valueFrom:
        configMapKeyRef:
          name: config-k8s
          key: container**
```
- 결과
```bash
vagrant@master-node:~$ kubectl exec cm-to-env-selective -- env
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
TERM=xterm
HOSTNAME=cm-to-env-selective
NEW_CONTAINER=docker
NEW_LOG_LEVEL=DEBUG
KUBERNETES_PORT_443_TCP_PROTO=tcp
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_ADDR=172.17.0.1
KUBERNETES_SERVICE_HOST=172.17.0.1
KUBERNETES_SERVICE_PORT=443
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_PORT=tcp://172.17.0.1:443
KUBERNETES_PORT_443_TCP=tcp://172.17.0.1:443
HOME=/root
```
- 선택한 Config Map의 Key=Value가 별칭으로 컨테이너의 환경 변수로 설정됨

#### 2. 컨피그맵의 값을 파드 내부의 파일로 마운트해서 사용
- 2.1. Config Map의 모든 Key=Value 쌍을 Pod에 파일로 마운트하는 방법
- 예시
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: cm-to-volume-all
spec:
  containers:
  - name: my-container
    image: docker.io/busybox
    args: ['tail', '-f', '/dev/null']
    volumeMounts:
    - name: configmap-volume
      mountPath: /etc/config
  volumes:
  - name: configmap-volume
    configMapRef:
      name: config-k8s
```
- 결과
```bash
vagrant@master-node:~$ kubectl exec cm-to-volume-all -- ls /etc/config
container
k8s
vagrant@master-node:~$ kubectl exec cm-to-volume-all -- cat /etc/config/container
docker
vagrant@master-node:~$ kubectl exec cm-to-volume-all -- cat /etc/config/k8s
kubernetes
```
- Config Map의 Key는 파일의 이름으로, Value는 파일의 내용이 됨

- 2.2. Config Map에 정의된 Key 이름 대신 별칭으로 Pod에 파일을 마운트하는 방법
- 예시
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: cm-to-volume-selective
spec:
  containers:
  - name: my-container
    image: docker.io/busybox
    args: ['tail', '-f', '/dev/null']
    volumeMounts:
    - name: configmap-volume
      mountPath: /etc/config
  volumes:
  - name: configmap-volume
    configMap:
      name: config-k8s
      items:
      - key: k8s
        path: k8s_fullname
```
- 결과
```bash
vagrant@master-node:~$ kubectl exec cm-to-volume-selective -- ls /etc/config
k8s_fullname
vagrant@master-node:~$ kubectl exec cm-to-volume-selective -- cat /etc/config/k8s_fullname
kubernetes
```
- 선택된 Config Map의 Key가 별칭으로 파일의 이름이 되고, Value는 파일의 내용이 됨
## Secret

- 인증 정보와 같이 보안이 필요한 정보
