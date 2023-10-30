kubectl create configmap backend --from-literal=REDIS_HOST=

- 이건 Cluster IP만 뽑아오는 명령
kubectl get svc <서비스 이름> -o custom-columns="CLUSTER-IP:.spec.clusterIP" --no-headers | tail -n 1

1. 레디스 디플로이 : 볼륨은 emptyDir
2. 레디스 클러스터IP 서비스 
3. 레디스 클러스터IP 컨피그맵1에 저장
4. 플라스크 디플로이 < 레디스 IP 삽입
5. 플라스크 로드밸런서 서비스
6. 플라스크 외부IP 컨피그맵2에 저장
7. 리액트 엔진액스 디플로이 < 플라스크 IP 삽입

Configmap 1
REDIS_HOST
DB_HOST
DB_PORT
DB_NAME
DB_USER

secret1
DB_PASSWORD
SECRET_KEY
JWT_SECRET_KEY

Configmap 2
BASE_URL
