# Job

![job](https://github.com/seungwonbased/TIL/blob/main/K8s/assets/job1.png)

- 일회성 또는 배치 작업을 실행하기 위한 객체
- 주로 한 번 실행하고 완료해야 하는 작업을 정의하는 데 사용됨
- Pod의 모든 컨테이너가 정상적으로 종료될 때까지 재실행
- 특정 개수만큼 Pod를 정상적으로 실행 종료되는 것을 보장
	- Pod 내의 모든 컨테이너가 exit 0를 반환되는 것이 정상 종료

## Job Controller의 특징

- 지정한 실행 횟수와 병행 개수에 따라 한 개 이상의 Pod를 실행
- Job은 Pod 내에 있는 모든 컨테이너가 정상 종료한 경우에 Pod를 정상 종료한 것으로 취급
- Job에 기술한 Pod의 실행 횟수를 전부 정상 종료하면 Job은 종료됨
- 노드 장해 등에 의해 Job의 Pod가 제거된 경우 다른 노드에서 Pod를 재실행
- Job에 의해 실행된 Pod는 Job이 삭제될 때까지 유지되고, Job을 삭제하면 모든 Pod가 삭제됨

## Use Cases

### 동시 실행과 순차 실행

![job](https://github.com/seungwonbased/TIL/blob/main/K8s/assets/job2.png)

### Pod를 실행할 노드를 선택

![job](https://github.com/seungwonbased/TIL/blob/main/K8s/assets/job3.png)

### 온라인 Batch 처리 요청

![job](https://github.com/seungwonbased/TIL/blob/main/K8s/assets/job4.png)

### 정기 실행 Batch 처리

- Cron Job은 설정한 시간에 정기적으로 Job을 실행 
	- 데이터의 백업이나 매시간 마다 실행되는 배치 처리 등을 실행

## Job의 병렬성 관리

- Job 하나가 동시에 실행할 Pod를 지정
- 메니페스트 파일의 spec.parallelism 필드에 설정
- 기본값은 1이고 0으로 설정하면 Job을 정지

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: job-abnormal-end
spec:
  template:
    spec:
      containers:
      - name: busybox
        image: docker.io/busybox
        command: ["sh", "-c", "sleep 5; exit 1"]  
      restartPolicy: Never
  # completions: 1      # 총 실행 회수 (기본값 1)
  # parallelism: 1      # 동시 실행할 파드의 개수 (기본값 1)
  backoffLimit: 3       # 실패했을 때 재실행하는 최대 횟수
```

## 메시지 브로커와 잡 컨트롤러의 조합

![job](https://github.com/seungwonbased/TIL/blob/main/K8s/assets/job5.png)

