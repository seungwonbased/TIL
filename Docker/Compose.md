# Docker Compose

- 도커 데몬이 돌고 있는 단일 머신: Host, Node, Server, Peer
- Compose는 다중 컨테이너 Docker 애플리케이션을 정의하고 실행하기 위한 도구
	- 다중 컨테이너 설정을 더 쉽게 관리하고,  설정 프로세스를 자동화하는 데 도움
- YAML 파일을 사용하여 애플리케이션 서비스를 구성
	- 그런 다음 단일 명령을 사용하여 구성에서 모든 서비스를 생성하고 시작
- Compose는 모든 환경에서 작동
	- 프로덕션, 스테이징, 개발, 테스트는 물론 CI 워크플로까지, 또한 애플리케이션의 전체 수명주기를 관리하기 위한 명령도 있음
		- 서비스 시작, 중지 및 재구축
		- 실행 중인 서비스 상태 보기
		- 실행 중인 서비스의 로그 출력 스트리밍
		- 서비스에서 일회성 명령 실행

## YAML

![yaml](https://github.com/seungwonbased/TIL/blob/main/Docker/assets/yaml1.png)

- [yaml](https://github.com/seungwonbased/TIL/blob/main/Docker/YAML.md)
## docker-compose.yaml 파일 구조

### version

- docker-compose 파일의 구문 버전을 정의
- 버전에 따라 지원하는 형식이 다름
- 주로 3 버전을 사용함

### services

```yaml
# 버전에 따라서 지원하는 형식이 다름
version: '3.0'

# Service 정의
# docker-compose로 생성할 컨테이너의 옵션을 정의
# 서비스 안의 컨테이너들은 하나의 프로젝트로서 docker-compose로 관리됨
services:
    # 생성할 컨테이너 이름을 지정
    컨테이너_이름:
        # 컨테이너 생성시 사용할 베이스 이미지 지정
        image: ubuntu:latest
        # build 옵션
        # docker-compose build 옵션에서 사용됨
        # Dockerfile에 명시된 FROM의 이미지를 사용하여 위에 명시된 이미지 이름과 태그로 새로운 이미지를 생성
        build:
            # Dockerfile의 위치를 지정
            context: .
        # 호스트와 컨테이너의 포트 매핑 정보
        ports:
            - "80:80"
        # 도커 컨테이너가 실행되었을 때 요청을 리슨하고 있는 포트를 지정
        # 호스트 포트:컨테이너 포트를 매핑하게 되면 나중에 스케일할 때 충돌 남
        # 여러 개의 포트를 지정할 수 있음
        expose:
            - "80"
        # 환경 변수 리스트를 정의
        environment:
            EXAMPLE_URL: "https://www.example.com"
        # 실행 환경 설정
        stdin_open: true
        tty: true
        entrypoint: /bin/bash
```

- 실행될 도커 컨테이너에 대한 설명
	- 컨테이너 이미지를 빌드하는 방법
	- 컨테이너를 실행하는 방법
	- 레지스트리에서 도커 이미지를 사용하는 방법
- 서비스는 실제 컨테이너를 의미

### networks

- 서비스에서 사용할 네트워크에 대한 설명

### volumes

- 서비스에서 마운트될 데이터 볼륨에 대한 설명

## docker-compose 명령어

- **docker-compose pull [Service]**
	- docker-compose.yaml 내의 이미지들을 모두 다운로드
	- 뒤에 \[Service](컨테이너 이름)를 추가하면, 추가한 이름의 이미지만 다운받음
- **docker-compose build [Service]**
	- docker-compose.yaml 내에 명시한 dockerfile 을 빌드
	- 뒤에 [Service] 를 추가하면, 추가한 이름의 dockerfile 만 빌드
- **docker-compose up [Service]**
	- docker-compose.yaml 을 기반으로 컨테이너를 실행
	- 실행된 후에 서비스가 forground 에서 동작하는데, docker-compose up -d 처럼 -d 옵션을 추가하여 서비스를 background 에서 동작하도록 함
- **docker-compose ps**
	- 현재 실행중인 서비스 목록 출력
- **docker-compose logs [Service]**
	- 현재 실행중인 서비스의 로그 출력
	- docker-compose logs -f 처럼 -f 옵션을 주면 로그를 실시간으로 볼 수 있음
- **docker-compose top**
	- 서비스 내에서 실행중인 프로세스 목록 출력
- **docker-compose stop [Service]**
	- 서비스 중지
- **docker-compose start [Service]**
	- 멈춰있는 서비스의 컨테이너를 실행
- **docker-compose exec \[Container] \[Command]**
	- 컨테이너에서 명령어를 실행
	- 컨테이너 내부로 들어갈 때 이 명령어를 사용
- **docker-compose run \[service] \[command]**
	- 해당 서비스에 컨테이너를 하나 더 실행
	- exec 와 다른 점은, 이건 실행할 때 아예 새로운 컨테이너 하나를 만들고 실행함
- **docker-compose down [Service]**
	- 서비스를 멈추고 (Stop) 컨테이너를 삭제 (rm)
	- docker-compose down -v 처럼 -v 옵션을 주면 볼륨까지 함께 삭제

