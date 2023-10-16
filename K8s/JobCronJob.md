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

# CronJob

> Cron: UNIX 계열 운영체제에 구현된 시간 기반의 스케줄러

- Job을 시간 기준으로 관리하도록 생성
- 지정한 시간에 한 번만 Job을 실행하거나 지정한 시간동안 주기적으로 Job을 반복 실행
	- [Cron 형식](https://github.com/seungwonbased/TIL/blob/main/K8s/cron.md)의 시간을 지정
- **주로 애플리케이션 프로그램 데이터, 데이터베이스와 같은 중요 데이터를 백업하는 데 사용**
	- 프로젝트할 때 사용 가능
- 생성된 Pod의 개수가 정해진 수를 넘어서면 가비지 컬렉터 컨트롤러가 종료된 Pod를 삭제

## CronJob과 Job의 관계

![job](https://github.com/seungwonbased/TIL/blob/main/K8s/assets/job6.png)

## CronJob 설정
### spec.schedule

- **Cron 형식으로 스케줄을 기술**
- 분 (0-59), 시 (0-23), 일 (1-31), 월 (1-12), 요일 (0-7, 0: 일요일, 7: 일요일)

### spec.startingDeadlineSeconds

- 지정된 시간에 CronJob이 실행되지 못했을 때 필드 값으로 설정한 시간까지 지나면 CronJob이 실행되지 않게 함

### spec.concurrentPolicy

- CronJob이 실행하는 Job의 동시성을 관리
- 병렬성과는 개념이 조금 다름
	- 앞에 실행한 Job이 끝나지 않더라도 시간이 되면 새로운 Job을 띄울 것인지를 관리
- Allow
	- 기본값, 여러 개의 Job을 동시에 실행할 수 있게 함
- Fobid
	- 동시 실행을 금지
- Replace
	- 앞에 실행했던 Job이 실행 중인 상태에서 새로운 Job을 실행할 시간이 되면 이전에 실행 중이던 Job을 새로운 Job으로 대체

### spec.successfulJobHistoryLimit

- 정상 종료된 Job 내역의 보관 개수
	- 기본값: 3

### spec.failedJobHistoryLimit

- 비정상 종료된 Job 내역의 보관 개수
