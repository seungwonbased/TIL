# Docker Desktop

- 간단한 GUI를 제공
- 도커 데스크탑에 포함되어 있는 것
	- Docker Engine
	- Docker CLI Client
	- Docker Buildx
	- Docker Extensions
	- Docker Compose
	- Docker Content Trust
	- Kubernetes
	- Credential Helperw

# 도커의 구성 요소

![arc](https://github.com/seungwonbased/TIL/blob/main/Docker/assets/arc1.png)

## Docker Daemon

- dockerd
- 도커 API 요청을 수신
- 도커 이미지, 컨테이너, 네트워크, 볼륨과 같은 도커 객체를 관리
- 도커 서비스를 관리하기 위해 다른 데몬과 통신

## Client

- docker
- 도커 사용자가 도커와 상호작용하는 기본 방법
- 도커 사용자의 명령을 도커 API를 사용해 도커 데몬으로 전달
- 하나 이상의 도커 데몬과 통신이 가능

## Registry

- 도커 이미지 저장소
- 도커 이지미를 저장하고 배포하는 표준 방법
- Public Registry
	- Docker Hub: 별도로 지정하지 않으면 기본 값으로 사용
- Private Registry

## Docker Object
### Image

- 컨테이너를 생성할 때 필요한 요소
- 가상 머신을 생성할 때 사용하는 가상 머신 이미지 또는 ISO 파일과 비슷한 개념
- 여러 개의 계층으로 된 바이너리 파일로 존재
- 컨테이너를 생성하고 실행할 때 **읽기 전용**으로 사용됨
- Registry / Repository: Tag

### Container

- 도커 이미지로 생성한 해당 이미지의 목적에 맞는 파일이 들어있는 파일 시스템과 격리된 시스템 자원 및 네트워크를 사용할 수 있는 독립된 공간
- 컨테이너는 이미지를 읽기 전용으로 사용하되, 이미지에서 변경된 사항만 컨테이너 계층에 저장하므로 컨테이너에서 무엇을 하든지 원래 이미지는 영향 받지 않음
- 생성된 컨테이너는 각기 독립된 파일 시스템을 제공받으며 호스트와 분리되어 있으므로 특정 컨테이너가 어떤 애플리케이션을 설치하거나 삭제해도 다른 컨테이너와 호스트는 변화가 없음

#### Container Lifecycle

![arc](https://github.com/seungwonbased/TIL/blob/main/Docker/assets/arc2.png)

## Dockerfile 구조와 명령어

![arc](https://github.com/seungwonbased/TIL/blob/main/Docker/assets/arc3.png)

## Dockerfile

- 도커 이미지를 빌드하는 데 필요한 모든 명령어를 순서대로 기술한 텍스트 파일
- 컨테이너에 설치해야 하는 패키지 (FROM), 추가해야 하는 소스코드 (ADD, COPY), 실행해야 하는 명령어와 쉘 스크립트 (RUN, CMD) 등을 기록한 파일
- Dockerfile을 사용하면 직접 컨테이너를 생성하고 이미지로 커밋해야 하는 번거로움을 덜 수 있을 뿐 아니라 깃과 같은 개발 도구를 통해 애플리케이션의 빌드 및 배포를 자동화할 수 있음
- 설치되는 것들에 대한 명세를 볼 수 있어 감사할 수 있기 때문에 보안적으로 안전

### Dockerfile 기본 명령어

|명령어|동작|
|:---:|:---:|
|**FROM**|기본 이미지|
|**RUN**|쉘 명령어 실행|
|**CMD**|컨테이너 기본 실행 명령어 (Entrypoint의 인자로 사용)|
|**EXPOSE**|오픈되는 포트 정보|
|ENV|환경변수 설정|
|ADD|파일 또는 디렉토리 추가, URL/ZIP 사용 가능|
|**COPY**|파일 또는 디렉토리 추가|
|ENTRYPOINT|컨테이너 기본 실행 명령어|
|VOLUMNE|외부 마운트 포인트 생성|
|USER|RUN, CMD, ENTRYPOINT를 실행하는 사용자|
|**WORKDIR**|작업 디렉토리 설정|
|ARGS|빌드 타임 환경변수 생성|
|LABEL|key-value 데이터|
|ONBUILD|다른 빌드의 베이스로 사용될 떄 사용하는 명령어|

- FROM
	- Base Image를 가져옴
	- Full 이미지 이름은 레지스트리, 베이스 이미지 이름, 태그, 네임스페이스 등으로 이뤄짐
	- e.g., FROM    golang:1.9
		- 레지스트리가 없으면 디폴트로 Dockerhub
		- 태그가 없으면 디폴트로 Latest
- RUN
	- 컨테이너에서 실행할 명령어를 지정
	- e.g., RUN    mkdir /echo
- COPY
	- 호스트의 파일을 컨테이너에 복사
	- e.g., COPY    main.go /echo
- CMD
	- 컨테이너 실행 시 실행할 명령어
	- e.g., CMD    ["go", "run", "/echo/main.go"]
- ...

## Context

- 지정된 경로 (PATH) 또는 URL에 있는 파일 세트
- Docker CLI를 사용해 다른 도커 호스트 또는 레지스트리와 상호 작용할 때 사용되는 설정 및 연결 정보의 집합
- build 프로세스는 컨텍스트에 있는 모든 파일을 참조할 수 있음

## Dangling Image

- 다른 이미지에 연결되지 않은 쓸모 없는 이미지
- Dangling 이미지는 더 이상 어떤 컨테이너나 이미지와도 연관이 없는 이미지로, 주로 불필요한 디스크 공간을 차지하고 시스템 자원을 낭비할 수 있음
- 새 이미지가 원래 있던 이미지의 이름을 가져가고, 원래 있던 이미지가 Dangling 되는 거라고 이해하면 됨
1. 이미지 버전 업데이트
	- 이미지를 새로 빌드하거나 업데이트할 때, 이전 버전의 이미지는 Dangling 이미지로 남을 수 있음
2. 컨테이너 삭제
	- 컨테이너를 삭제할 때, 해당 컨테이너에 연결된 이미지가 Dangling 이미지로 남을 수 있음
