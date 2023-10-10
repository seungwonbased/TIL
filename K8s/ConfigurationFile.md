# K8s YAML 설정 파일

### 설정 파일의 세 부분

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

### metadata

- 컴포넌트를 서로 구분 지어줄 수 있는 이름과 설명을 입력할 수 있음

### specification

- kind에 적힌 컴포넌트에 대한 구체적인 내용을 정의하는 **spec** 부분
- spec에 대한 포맷은 쿠버네티스 컴포넌트 종류마다 다름

### status

- **status**는 자동적으로 생성되고, 쿠버네티스에 자동 등록됨
- **Desired State(목표 상태)** 와 **Actual State(실제 상태)** 로 구분될 수 있음

