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

#### 2. Config Map 값을 Pod 내부의 파일로 마운트해서 사용
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

### YAML 파일로 Config Map 정의

- kubectl create 명령에서 --dry-run 옵션과 --o yaml 옵션을 사용해서 Config Map을 생성하지 않고 Config Map 설정을 YAML 파일로 추출할 수 있으며, 해당 파일을 이용해서 Config Map을 생성할 수 있음

## Secret

- 인증 정보와 같이 보안이 필요한 정보, 민감한 정보를 저장하기 위해 사용
- 네임스페이스에 종속되는 쿠버네티스 오브젝트
	- 볼륨은 전역적인 컴포넌트
	- 접근 통제를 위해서 볼륨보다는 네임스페이스에 종속되는 시크릿을 사용하는 것이 권장됨

### Secret 생성 방법

- --from-literal 옵션을 이용
- --from-file 옵션을 이용

### Pod에서 Secret을 사용하는 방법
#### 1. Secret의 값을 Pod의 환경 변수로 사용
- 1.1. Secret에 저장된 모든 Key=Value 쌍을 Pod의 환경 변수로 설정
- 예시
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-to-env-all
spec:
  containers:
  - name: my-container
    image: docker.io/busybox
    args: ['tail', '-f', '/dev/null']
    envFrom:
    - secretRef:
        name: my-password**
```
- 결과
```shell
vagrant@master-node:~$ kubectl exec secret-to-env-all -- env
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
TERM=xterm
HOSTNAME=secret-to-env-all
password=p@ssw0rd
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

- 1.2. Secret에 저장된 Key=Value 쌍 중 원하는 데이터만 Pod의 환경 변수로 설정
- 예시
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-to-env-selective
spec:
  containers:
  - name: my-container
    image: docker.io/busybox
    args: ['tail', '-f', '/dev/null']
    env:
    - name: YOUR_PASSWORD
      valueFrom:
        secretKeyRef:
          name: our-password
          key: pw2
```
- 결과
```shell
vagrant@master-node:~$ kubectl exec ssecret-to-env-selective -- env
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
TERM=xterm
HOSTNAME=secret-to-env-selective
YOUR_PASSWORD=yourpassword
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
#### 2. Secret의 값을 Pod의 내부의 파일로 마운트해서 사용
- 1.1. Secret에 저장된 모든 Key=Value 쌍을 Pod에 마운트
- 예시
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-to-volume-all
spec:
  containers:
  - name: my-container
    image: docker.io/busybox
    args: ['tail', '-f', '/dev/null']
    volumeMounts:
    - name: secret-volume 
      mountPath: /etc/secret
  volumes:
  - name: secret-volume 
    secret:
      secretName: our-password
```
- 결과
```shell
vagrant@master-node:~$ kubectl exec secret-to-volume-all -- ls /etc/secret
pw1
pw2
vagrant@master-node:~$ kubectl exec secret-to-volume-all -- cat /etc/secret/pw1
mypassword
vagrant@master-node:~$ kubectl exec secret-to-volume-all -- cat /etc/secret/pw2
yourpassword
```

- 1.2. Secret에 저장된 Key=Value 쌍 중 원하는 데이터만 Pod에 마운트
- 예시
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-to-volume-selective
spec:
  containers:
  - name: my-container
    image: docker.io/busybox
    args: ['tail', '-f', '/dev/null']
    volumeMounts:              
    - name: secret-volume      
      mountPath: /etc/secret  
  volumes:
  - name: secret-volume        
    secret:
      secretName: our-password
      items:
      - key: pw1
        path: password1
```
- 결과
```shell
vagrant@master-node:~$ kubectl exec secret-to-volume-selective -- ls /etc/secret
password1
vagrant@master-node:~$ kubectl exec secret-to-volume-selective -- cat /etc/secret/password1
mypassword
```

### Secret Types

> https://kubernetes.io/ko/docs/concepts/configuration/secret/#secret-types

- 시크릿 타입은 여러 종류의 기밀 데이터를 프로그래밍 방식으로 용이하게 처리하기 위해 사용

|빌트인 타입|사용처|
|---|---|
|`Opaque`|임의의 사용자 정의 데이터|
|`kubernetes.io/service-account-token`|서비스 어카운트 토큰|
|`kubernetes.io/dockercfg`|직렬화 된 (Serialized) `~/.dockercfg` 파일|
|`kubernetes.io/dockerconfigjson`|직렬화 된 `~/.docker/config.json` 파일|
|`kubernetes.io/basic-auth`|기본 인증을 위한 자격 증명 (Credential)|
|`kubernetes.io/ssh-auth`|SSH를 위한 자격 증명|
|`kubernetes.io/tls`|TLS 클라이언트나 서버를 위한 데이터|
|`bootstrap.kubernetes.io/token`|부트스트랩 토큰 데이터|

