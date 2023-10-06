# Pods

- 컨테이너 애플리케이션의 기본 단위 
- 단일 목적을 수행하기 위한 한 개 이상의 컨테이너로 구성된 컨테이너의 집합
- **여러 리눅스 네임스페이스를 공유하는 여러 컨테이너들을 추상화된 집합으로 사용하기 위해 사용**
	- Pod 내부의 컨테이너는 리소스를 공유
	- Pod 내부에서는 컨테이너끼리 localhost로 통신 가능

## 특징

- Pod은 임시적이고 지속되지 않음
	- 쿠버네티스에 의해 교체되거나 제거되면 모든 리소스, 데이터를 잃음
- Pod이 쿠버네티스에 의해 관리되려면 Controller가 필요

## 생성 방법

- CLI
	- kubectl run \[POD NAME] \[IMAGE] \[OPTIONS]
- YAML
	- .yaml 파일 작성 후 kubectl apply

## Pods Lifecycle

|Value|Description|
|---|---|
|ContainerCreating|이미지를 다운로드 중이거나 컨테이너를 생성하는 중<br>Config Map, 시크릿이 마운트되지 않아 컨테이너 생성이 보류 중인 상태를 의미|
|CrashLoopBackOff|파드 내의 컨테이너가 종료되어 다음 기동 시까지 대기 상태에 있음|
|Pending|The Pod has been accepted by the Kubernetes cluster, but one or more of the containers has not been set up and made ready to run. This includes time a Pod spends waiting to be scheduled as well as the time spent downloading container images over the network.|
|Running|The Pod has been bound to a node, and all of the containers have been created. At least one container is still running, or is in the process of starting or restarting.|
|Terminating|컨테이너에 종료 요청 시그널을 보낸 후 컨테이너가 종료할 때까지 대기 중|
|Succeeded|All containers in the Pod have terminated in success, and will not be restarted.|
|Completed|파드 내 컨테이너가 정상적으로 종료되었음<br>파드 내에 복수의 컨테이너가 있는 경우, 첫 번째 컨테이너가 정상 종료하면 Completed가 표시됨|
|Error|컨테이너가 이상 종료된 경우<br>파드 내에 복수의 컨테이너가 있는 경우, 첫 번째 컨테이너가 이상 종료하면 Error가 표시됨|
|Failed|All containers in the Pod have terminated, and at least one container has terminated in failure. That is, the container either exited with non-zero status or was terminated by the system.|
|Unknown|For some reason the state of the Pod could not be obtained. This phase typically occurs due to an error in communicating with the node where the Pod should be running.|

## Init Containers

- Pod의 앱 컨테이너들이 실행되기 전에 실행되는 특수한 컨테이너    
- 앱 이미지에는 없는 셋업을 위한 유틸리티 또는 설정 스크립트 등을 포함할 수 있음
- 일반적인 컨테이너와 차이점
	- 앱 컨테이너의 리소스 상한, 볼륨, 보안 셋팅을 포함한 모든 필드와 기능을 지원
	- 초기화 컨테이너는 파드가 준비 상태가 되기 전에 완료를 목표로 실행되어야 하므로, lifecycle, livenessProbe, readinessProbe, startupProbe을 지원하지 않음

## Health Checks

- **파드의 컨테이너에는 애플리케이션이 정상적으로 기동하고 있는지 확인하는 기능을 설정할 수 있음**
- 이상이 감지되면 컨테이너를 강제 종료하고 재시작할 수 있음 
- kubelet가 컨테이너의 헬스 체크를 담당 
- kubelet의 헬스 체크는 활성 프로브(Liveness Probe)와 준비 상태 프로브(Readiness Probe)를 사용하여 실행 중인 파드의 컨테이너를 검사
	- 활성 프로브(Liveness Probe)
		- 컨테이너의 애플리케이션이 정상적으로 실행 중인 것을 검사
		- 검사에 실패하면 파드 상의 컨테이너를 강제로 종료하고 재시작
		- 이 기능을 사용하기 위해서는 매니페스트(YAML 파일)에 명시적으로 설정해야 함
	- 준비 상태 프로브(Readiness Probe)
		- 컨테이너의 애플리케이션이 요청을 받을 준비가 되었는지를 검사
		- 검사에 실패하면 서비스에 의한 요청 트래픽 전송을 중지
		- 파드가 기동하고 나서 준비가 될 때까지 요청이 전송되지 않기 위해 사용
		- 이 기능을 사용하기 위해서는 매니페스트에 명시적으로 설정해야 함
	- 프로브 대응 핸들러
		- exec
			- 컨테이너 내 커맨드를 실행
			- exit 코드 0으로 종료하면 진단 결과는 성공으로 간주하고, 그 외는 실패로 간주 
		- tcpSocket
			- 지정한 TCP 포트 번호로 연결할 수 있으면 성공으로 간주
		- httpGet
			- 지정한 포트와 경로로 HTTP GET 요청을 정기적으로 실행
			- HTTP 상태 코드가 200 이상 400 미만이면 성공으로 간주하고, 그 외는 실패로 간주 
			- 지정 포트가 열려 있지 않은 경우도 실패로 간주
