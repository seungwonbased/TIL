# Command: docker build

```docker
docker build [options] [Dockerfile 경로 또는 URL]
```

- Bulid: Dockerfile 및 Context에서 Docker 이미지를 빌드하는 것
- [options]
	- Docker 빌드 명령어에 사용되는 옵션
	- 다양한 빌드 옵션을 지정할 수 있음
- [Dockerfile 경로 또는 URL]
	- Docker 이미지를 빌드하는 데 사용할 Dockerfile의 경로를 지정
	- Dockerifle은 이미지를 빌드하기 위한 구성 파일로 이미지 내부에서 어떤 파일과 설정을 사용할 것인지 정의
	- . 은 CLI 상 현재 작업 디렉터리

## docker build 주요 옵션

### -t, --tag

```docker
docker build -t my-image:1.0 .
```

- 이미지명과 태그명을 붙이는 것
- 실제 사용에서 거의 필수
### -f, --file

```docker
docker build -t my-image:1.0 -f /path/to/Dockerfile .
```

- Dockerfile의 경로 지정
- 디폴트는 /path/Dockerfile

### --no-cache

```docker
docker build -t my-image:1.0 --no-cache .
```

- 이전 빌드에서 캐싱하지 않고 모든 레이어를 다시 빌드

# Command: docker run

```docker
docker build [options] [image name:tag] [cmd] [arg ...]
```

- 도커 컨테이너를 실행할 때 사용되는 명령어
- 이 명령어를 이용해 이미지를 컨테이너로 만들고 실행할 수 있음
- [options]
	- 도커 컨테이너 실행 시 추가적인 옵션을 설정
	- 포트 매핑, 환경 변수 등 여러가지를 설정할 수 있음
- [image name:tag]
	- 실행할 도커 이미지의 이름과 태그를 지정
	- 이미지 이름은 필수 항목이며 태그는 선택 사항
	- 이미지 이름은 도커 허브 또는 로컬에서 빌드한 이미지의 이름과 일치해야 함
- [cmd]
	- 컨테이너가 시작될 때 실행할 명령어 지정

## docker run 주요 옵션

### -a, --attach

- 컨테이너에 표준 입력(stdin), 표준 출력(stdout), 표준 에러(stderr)를 연결
	- "컨테이너"에 연결하는 것임
	- 컨테이너와 대화형으로 쓰려면 -it 옵션을 추가로 줘서 호스트에도 표준 입출력을 붙여줘야 함

### -d, -dettach

- 일반적으로 데몬 모드라고 부르며, 컨테이너가 백그라운드로 실행됨

### --entrypoint

- 컨테이너가 시작될 때 실행할 명령어 또는 스크립트를 지정할 수 있음
- 이를 통해 컨테이너가 기본 실행 명령어 대신에 사용자 지정 명령어나 스크립트를 실행하도록 할 수 있음

```docker
docker run --entrypoint [명령어 또는 스크립트] [이미지 이름:태그]
```

### -e, -env

- 컨테이너 내에서 사용할 환경 변수를 설정
- 보통 설정 값이나 비밀번호를 전달할 때 사용

```docker
docker run -e GRANT_SUDO=yes [이미지 이름:태그]
```

### --expose

- 컨테이너 내부에서 실행 중인 서비스 또는 포트를 외부에 노출시킬 수 있음
- 이 옵션을 사용해 컨테이너 내부의 포트를 호스트 머신 또는 다른 컨테이너에서 접근 가능하게 만들 수 있음

```docker
docker run --expose [포트 번호] [이미지 이름:태그]
```

### -i, --interactive

- 표준 입력(stdin)을 활성화하며, 컨테이너와 Attach 되어 있지 않더라도 표준 입력을 유지
- 보통 이 옵션을 사용해 Bash에 명령을 입력

### --mount

- 옛날에 쓰던 거임
- --volume에 비해 보안에 취약
- 기본적인 -v 또는 --volume 옵션보다 더 다양한 옵션을 설정할 수 있음
	- 좀 장황함

```docker
docker run --mount [구성 옵션] [이미지 이름:태그]
```

### --name

- 컨테이너에 고유한 이름을 부여할 수 있음
- 컨테이너의 이름을 지정하면 해당 이름으로 컨테이너를 식별할 수 있으며, 컨테이너가 실행 중일 때도 해당 이름을 참조할 수 있음

```docker
docker run --name [컨테이너 이름] [이미지 이름:태그]
```

### -p, --publish

- 호스트와 컨테이너의 포트를 연결 (포트 바인딩, 포트 포워딩, ..)

```docker
docker run -p [호스트 포트:컨테이너 포트] [이미지 이름: 태그]
```

### --previleged

- 컨테이너 안에서 호스트의 리눅스 커널 기능을 모두 사용하도록 설정
- 호스트의 주요 자원에 접근할 수 있음

### --restart

- 프로세스 종료 시 컨테이너 재시작
### --rm

- 프로세스 종료 시 컨테이너 자동 삭제

### -t, --tty

- TTY 모드(Pseudo-TTY)를 사용
- Bash를 사용하려면 이 옵션을 설정해야 함
- 이 옵션을 설정하지 않으면 명령을 입력할 수는 있지만, 쉘이 표시되지 않음

### -u, --user

- 컨테이너가 실행될 리눅스 사용자 계정 이름 또는 UID를 설정
- 컨테이너 내에서 실행 중인 프로세스가 특정 사용자와 그룹 권한을 갖도록 설정할 수 있음

```docker
docker run -u [사용자 또는 UID:그룹 또는 GID] [이미지 이름:태그]
```

### -v, --volume

- 호스트 머신의 파일 시스템 디렉터리를 컨테이너 내부로 마운트할 수 있음
- 이를 통해 호스트 머신과 컨테이너 간에 데이터를 공유하거나 컨테이너에서 데이터를 지속적으로 저장할 수 있음
- Docker Volume으로 만들면 Docker가 관리하는 디렉터리 내에 생성됨

```docker
docker run -v [호스트 경로:컨테이너 경로] [이미지 이름:태그]
```

### -w, --workdir

- 컨테이너 내부에서 프로세스가 실행되는 작업 디렉터리를 설정
- 작업 디렉터리: 컨테이너 내에서 명령어 실행 및 파일 작업을 수행하는 기본 디렉터리

```docker
docker run -w [작업 디렉터리 경로] [이미지 이름:태그]
```
