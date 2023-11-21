# CodeDeploy

- EC2 인스턴스, 온프레미스, 서버리스 Lambda 함수 또는 ECS 서비스로 애플리케이션 배포를 자동화하는 배포 서비스

# GitHub Action
## Workflows

- 자동화해 놓은 작업 과정
- 코드 저장소 내에 .github/workflows 디렉터리에 위치한 yaml 파일로 작업 과정을 설정
- 하나의 코드 저장소에 여러 개의 워크플로우 설정 가능
- on 속성을 이용해 해당 워크플로우가 언제 실행되는지와 job 속성을 이용해 해당 워크플로우가 구체적으로 어떤 일을 하는 지 명시

## Events

- 워크플로우를 실행하는 특정 활동 또는 규칙

```yaml
on:
  push:
    branches:
      - main ⇐ 코드 저장소의 main 브랜치에 push 이벤트가 발생할 때 마다 워크플로우를 실행
jobs:
  ...
on:
  schedules:
    - cron: "0 0 * * * " ⇐ 매일 자정에 워크플로우를 실행
jobs:
  ...
```

### Jobs

- 독립된 가상머신 또는 컨테이너에서 돌아가는 하나의 처리 단위
- 하나의 워크플로우는 한 개 이상의 잡으로 구성
- 모든 잡은 기본적으로 동시에 실행되며, 의존 관계 설정을 통해 실행 순서를 제어할 수 있음
- jobs 속성을 사용해서 정의하고, job ID와 세부 내용을 매핑하는 형태로 기술 
- job 세부 내용에는 runs-on 속성으로 실행 환경을 지정해야 함

```
jobs:
  job1: ⇐ job ID
    runs-on: ubuntu-latest ⇐ 우분투 최신 버전의 실행 환경에서 해당 잡을 실행
    steps:
      ...      
```

### Steps

- 하나의 잡은 한 개 이상의 스탭으로 구성
- 하나의 스탭은 하나 이상의 태스크(task)로 구성
- 단순한 명령어(command) 또는 스크립트(script) 또는 복잡한 액션(action)을 실행  
- 명령어나 스크립트를 실행할 때는 run 속성을 사용
- 액션을 실행할 때는 uses 속성을 사용

```yaml
jobs:
  test:
    runs-on: ubuntu-lastes
    steps:
      - uses: actions/checkout@v3 ⇐ 코드 저장소에 코드를 작업 실행 환경에 가져오는 동작
      - run: npm install
      - run: npm start
```