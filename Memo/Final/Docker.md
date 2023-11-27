
- docker container 명령어 옵션 (예: 컨테이너 종료 시 자동 종료)


# Dockerfile

 - 도커 이미지를 빌드하는 데 필요한 모든 명령어를 순서대로 기술한 텍스트 파일
- 컨테이너에 설치해야 하는 패키지 (FROM), 추가해야 하는 소스코드 (ADD, COPY), 실행해야 하는 명령어와 쉘 스크립트 (RUN, CMD) 등을 기록한 파일
- Dockerfile을 사용하면 직접 컨테이너를 생성하고 이미지로 커밋해야 하는 번거로움을 덜 수 있을 뿐 아니라 깃과 같은 개발 도구를 통해 애플리케이션의 빌드 및 배포를 자동화할 수 있음
- 설치되는 것들에 대한 명세를 볼 수 있어 감사할 수 있기 때문에 보안적으로 안전

## Dockerfile 기본 명령어

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
|VOLUME|외부 마운트 포인트 생성|
|USER|RUN, CMD, ENTRYPOINT를 실행하는 사용자|
|**WORKDIR**|작업 디렉토리 설정|
|ARGS|빌드 타임 환경변수 생성|
|LABEL|key-value 데이터|
|ONBUILD|다른 빌드의 베이스로 사용될 떄 사용하는 명령어|

### 주요 명령어
#### **FROM**
- Base Image를 가져옴
- Full 이미지 이름은 레지스트리, 베이스 이미지 이름, 태그, 네임스페이스 등으로 이뤄짐
- e.g., FROM    golang:1.9
	- 레지스트리가 없으면 디폴트로 Dockerhub
	- 태그가 없으면 디폴트로 Latest

#### **RUN**
- 컨테이너에서 실행할 명령어를 지정
- e.g., RUN    mkdir /echo

#### **COPY**
- 호스트의 파일을 컨테이너에 복사
- e.g., COPY    main.go /echo

#### **CMD**
- 컨테이너 실행 시 실행할 명령어
- e.g., CMD    ["go", "run", "/echo/main.go"]

#### **ENTRYPOINT**
- CMD와 비슷하게 컨테이너가 실행되고 최초로 실행할 명령어를 정의
- e.g., ENTRYPOINT ["echo", "hello"]

#### **WORKDIR**
- 작업 디렉터리를 지정 

#### **EXPOSE**
- 컨테이너에서 노출할 포트

#### **ENV**
- Dockerfile에서 사용할 환경변수를 지정
	- Build 또는 애플리케이션에서 사용되는 환경변수를 지정
- e.g., ENV 환경변수_이름 환경변수_값
	- 일반적으로 환경변수 이름은 대문자로 많이 씀
- 설정한 환경변수는 ${환경변수_이름} 또는 $환경변수_이름 형태로 Dockerfile, 이미지, 이미지로 생성한 컨테이너에서 사용이 가능
- docker container run 명령어의 -e 옵션을 이용해 같은 이름의 환경변수 값을 덮어쓸 수 있음
	- 이 옵션은 애플리케이션에서 사용되는 환경변수를 지정

### Volumes

- docker docs 정리 내용
- Managed by Docker
- Two Types of Volume
	- 두 타입 모두 도커가 일부 폴더와 경로를 호스트 머신에 설정
		- 이러한 볼륨에 액세스할 수 있는 유일한 방법은 docker volume 명령어
		- 어떻게 파일 시스템에서 찾을 수는 있지만 그렇게 작업하면 안됨
			- Docker가 아닌 프로세스가 파일을 수정하면 안됨
		- 목적 자체가 호스트와 컨테이너 간의 데이터 공유가 아니고, 컨테이너 간의 공유가 목적
			- 호스트와 컨테이너 간의 데이터 공유는 bind mount로
	- Anonymous Volumes
		- 컨테이너가 존재하는 동안에만 존재
		- Dockerfile에서 VOLUME 명령어로 지정
	- Named Volumes
		- 컨테이너가 종료된 후에도 볼륨이 유지됨
		- 영구적이어야 하는 데이터나, 편집하거나 직접 볼 필요 없는 중요한 데이터에 적합
		- docker run 할 때 -v, --volume 옵션으로 지정
			- docker run -v [볼륨 이름 지정:공유하고자 하는 container의 path]
		- 컨테이너를 종료해도 docker volume 하면 그대로 데이터가 남아있는 것을 확인할 수 있음
		- 다시 새 컨테이너를 시작하고, -v 옵션으로 볼륨을 연결하면 그대로 이전 데이터 사용 가능

### RUN, CMD, ENTRYPOINT에서 명령어를 기술하는 방법

#### CMD
- JSON 배열 형식
	- ["aaa", "bbb",  ...]
	- 쉘을 실행하거나 어떤 종류의 확장도 수행하지 않고 직접 해석
- CMD mkdir /echo ⇒ /bin/sh -c mkdir /echo 형식으로 실행
- CMD [ "mkdir", "/echo" ] ⇒ mkdir /echo 형식으로 실행
- 둘이 결과는 거의 똑같음

#### COPY, ADD 
- COPY는 호스트 로컬의 파일만 이미지에 추가할 수 있지만, ADD는 외부 URL 및 tar 파일에서도 파일을 추가할 수 있음 (tar 파일의 경우 자동으로 해제해서 추가됨)
- ADD 보다는 COPY 사용을 권장 (의도하지 않은 파일 추가를 방지)

#### CMD vs ENTRYPOINT
- ENTRYPOINT는 docker run 할 때 항상 실행이 됨
- CMD는 docker run 할 때 변경이 가능
	- CMD 인자 값을 주는 방식으로 변경 가능
- ENTRYPOINT가 설정되었다면 cmd는 단지 ENTRYPOINT에 대한 인자의 기능을 수행
	- 기본 설정된 ENTRYPOINT 명령 뒤에 붙게 됨
- ENTRYPOINT run할 때 CMD로 덮어쓸 수 없기 때문에 명령을 보호하는 목적으로 사용함

## Dockerfile 작성 시 유의사항

### 빌드하는 과정에서 사용자 입력이 발생하지 않도록 해야 함

- ENV DEBIAN_FRONTEND=noninteractive
	- 예를 들어 우분투 설치할 때 타임존 같은 걸 입력해야 하는 걸 방지
	- 참고: https://www.cyberciti.biz/faq/explain-debian_frontend-apt-get-variable-for-ubuntu-debian/
- 혹은 apt-get install -y 옵션같은 걸 줘야함

### 컨테이너 실행 시 포그라운드 프로세스로 실행되도록 해야 함

- CMD apachectl -DFOREGROUND와 같은 옵션을 줘서 프로세스가 포그라운드로 실행되어야 컨테이너가 중지되지 않음
- 예시
	- 아파치 서버가 컨테이너의 우분투 위에서 백그라운드 프로세스로 동작하면 우분투의 bash에 다시 제어권이 넘어옴
	- bash가 처리할 일이 끝났으므로 bash가 종료되어 제어권이 호스트로 반환됨
	- 컨테이너가 종료됨

### Docker 이미지 크기가 커지지 않도록 유의

- 도커 컨테이너 이미지를 빌드하고 배포하는 시간을 단축하기 위해서는 도커 이미지 크기를 최대한 작게 유지하는 것이 중요
- Dockerfile의 모든 명령어는 레이어를 생성하고, 생성된 레이어는 저장됨
	- 실제 컨테이너에서 사용하지 않는 레이어도 저장 공간을 차지
	- 불필요한 레이어가 생성되지 않도록 하는 것이 중요
- **Dockerfile을 작성할 때, 명령어를 묶어서 실행하는 것이 좋음**
- **빌더 패턴, 다단계 도커 빌드 등의 기법을 이용해서 이미지를 작게 만드는 것이 필요**

#### 1. 레이어 최소화
- RUN 지시문을 결합해 최적화
- Dockerfile의 각 줄은 도커 이미지에서 공간을 차지할 새 레이어를 생성함
- 가능한 지시문을 결합해 적은 레이어를 생성

#### 2. 불필요한 도구 설치 금지
- 불필요한 개발 및 디버깅 도구(vi, curl, 컴파일러, ...)와 종속성을 제거하면 크기가 작은 효율적인 도커 이미지를 생성할 수 있음
- \--no-install-recommends 플래그 사용
	- 추천 및 제안 패키지가 자동으로 설치되는 것을 방지
	- e.g., apt-get install --no-install-recommends
#### 3. apt 패키지 매니저의 캐시 삭제
- apt 패키지 매니저의 캐시를 삭제해 이미지 크기 최적화
- RUN rm -rf /var/lib/apt/lists/\*

#### 4. 빌더 패턴

![file](https://github.com/seungwonbased/TIL/blob/main/Docker/assets/file1.png)

- 최적 크기의 도커 이미지를 생성하기 위해 사용하는 방법
- 두 개의 도커 이미지를 사용
	- 이미지 1: Builder
		- 소스 코드를 실행 파일로 만들기 위한 빌드 환경
		- 빌드에 필요한 컴파일러, 빌드 도구, 개발 종속성 등을 포함
	- 이미지 2: Runtime
		- 첫 번째 도커 컨테이너가 생성한 실행 파일을 실행하기 위한 런타임 환경
		- 실행 파일, 종속성 및 런타임 도구만 포함
	- **첫 번째 도커 컨테이너가 생성한 실행 파일을 두 번째 도커 컨테이너로 전달하는 스크립트가 필요**

#### 5. 다단계 도커 빌드
- Docker 17.05 버전에 새롭게 추가된 기능
- 하나의 Dockerfile에 여러 개의 FROM 문을 사용해 빌드 단계를 정의
	- --from 플래그를 사용해 각 단계에서 생성된 아티팩트 참조가 가능하도록 하는 것
- 각 단계는 0부터 순서대로 부여된 번호 또는 AS 절을 사용해 부여한 별칭을 이용할 수 있음
- 빌더 패턴에 비해 불필요한 파일(스크립트 등)이 없기 때문에 이걸 쓰는 게 좋음

## Dockerfile Best Practices

- 도커 이미지 빌드 시간 단축
- 이미지 크기 감소
- 보안 강화 및 유지 관리 가능성 보장

### 1. 적절한 베이스 이미지 사용
#### 1.1 도커 허브 공식 이미지를 사용
- 모범 사례에 따라 만들어지고 문서화되어 있으며 보안 패치가 적용돼 있음

#### 1.2. 특정 버전의 태그 사용
- 프로덕션 환경을 위한 도커 이미지를 빌드할 때, 베이스 이미지에 latest 태그를 사용하면 하위 호환성을 제공하지 않을 경우 문제 발생 가능
- 컨테이너 뜰 때마다 새 버전이 됨
- 그냥 latest 사용하면 안됨

#### 1.3. 최소 크기의 이미지 사용
- 최소 크기 버전의 부모 (베이스) 이미지를 사용
	- 최소 크기의 도커 이미지를 생성
- alpine linux 이미지를 중심으로 빌드된 최소 크기 이미지 또는 빌드 도구가 포함된 JDK 대신 JRE를 사용해 애플리케이션을 실행해야 함

### 2. 루트가 아닌 사용자로 컨테이너를 실행

- 도커 컨테이너는 기본적으로 루트 (id=0) 사용자로 실행
	- 해커가 도커 컨테이너 내부에서 실행되는 애플리케이션을 해킹한 후 도커 호스트에 대한 루트 액세스 권한을 획득할 수 있음
- 프로덕션 환경에서는 루트 사용자로 도커 컨테이너를 실행하는 것은 나쁜 보안 관행으로 간주
- 애플리케이션 실행에 필요한 최소한의 권한만 갖도록 최소 권한 원칙을 준수

#### 2.1. -u, --user 옵션 사용

```bash
docker container run --user=1000 ubuntu
```

#### 2.2. USER 지시문 사용

```dockerfile
FROM ubuntu

RUN  apt-get update

RUN  useradd demo-user

USER demo-user    # 이렇게 USER 지시문을 사용

CMD  whoami
```

- 2.1. 보다는 이 방법이 더 확실함

# docker image 명령어 및 옵션

- Docker 이미지를 관리하기 위한 주요 명령어는 다양하며, 여러 옵션을 사용할 수 있음
- 아래는 몇 가지 주요 Docker 이미지 관련 명령어와 옵션에 대한 간략한 설명

### 1. **이미지 빌드 (Build):**

```bash
docker build [OPTIONS] PATH | URL | -
```

- **옵션:**
  - `-t, --tag`: 이미지에 태그를 지정
  - `--build-arg`: 빌드 중에 전달되는 빌드-인 타임 변수를 설정
  - `-f, --file`: Dockerfile의 경로를 지정

### 2. **이미지 목록 보기:**

```bash
docker images [OPTIONS] [REPOSITORY[:TAG]]
```

- **옵션:**
  - `-a, --all`: 모든 이미지 표시 (기본적으로 사용 중인 이미지만 표시)
  - `--filter`: 필터를 적용하여 이미지 목록을 제한

### 3. **이미지 삭제:**

```bash
docker rmi [OPTIONS] IMAGE [IMAGE...]
```

- **옵션:**
  - `-f, --force`: 사용 중인 이미지를 강제로 삭제
  - `--no-prune`: 중간 이미지 삭제를 수행하지 않음

### 4. **이미지 가져오기 (Pull):**

```bash
docker pull [OPTIONS] NAME[:TAG|@DIGEST]
```

- **옵션:**
  - `--all-tags`: 모든 태그를 가져오기
  - `--disable-content-trust`: 서명된 이미지 가져오기 비활성화

### 5. **이미지에서 컨테이너 생성:**

```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

- **옵션:**
  - `-d, --detach`: 컨테이너를 백그라운드에서 실행
  - `-p, --publish`: 호스트와 컨테이너 간의 포트 매핑

### 6. **컨테이너로부터 이미지 생성:**

```bash
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
```

- **옵션:**
  - `-a, --author`: 이미지의 작성자를 설정
  - `-m, --message`: 이미지에 대한 설명 추가

### 7. **이미지 업로드:**

```bash
docker push [OPTIONS] NAME[:TAG]
```

- **옵션:**
  - `--disable-content-trust`: 이미지 업로드에 대한 서명 비활성화

### 8. **이미지 검색:**

```bash
docker search [OPTIONS] TERM
```

- **옵션:**
  - `--no-trunc`: 결과를 줄임 없이 표시

### 9. **이미지 저장 및 로드:**

```bash
docker save [OPTIONS] IMAGE [IMAGE...]
docker load [OPTIONS]
```

### 10. 모든 이미지 강제 삭제

```bash
docker image rm -f $(docker image ls -q)
```

- **저장 옵션:**
  - `-o, --output`: 저장할 파일의 경로를 지정

- 이 외에도 다양한 Docker 이미지 관련 명령어와 옵션이 있음
- `docker --help` 명령어를 사용하면 도움말을 확인할 수 있음

# docker container 명령어 및 옵션

- Docker에서 컨테이너를 관리하기 위한 주요 명령어와 옵션은 다양
- 아래는 몇 가지 주요한 Docker 컨테이너 관련 명령어와 옵션에 대한 간략한 설명

### 1. **컨테이너 목록 보기:**

```bash
docker ps [OPTIONS]
```

- **옵션:**
  - `-a, --all`: 모든 컨테이너 표시 (기본적으로 실행 중인 컨테이너만 표시)
  - `-q, --quiet`: 컨테이너 ID만 표시
  - `-s, --size`: 컨테이너의 디스크 사용량 표시

### 2. **컨테이너 시작 및 실행:**

```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

- **옵션:**
  - `-d, --detach`: 컨테이너를 백그라운드에서 실행
  - `-p, --publish`: 호스트와 컨테이너 간의 포트 매핑
  - `--name`: 컨테이너에 이름 지정

### 3. **컨테이너 중지:**

```bash
docker stop [OPTIONS] CONTAINER [CONTAINER...]
```

- **옵션:**
  - `-t, --time`: 컨테이너가 강제 종료되기 전에 기다리는 시간

### 4. **컨테이너 재시작:**

```bash
docker restart [OPTIONS] CONTAINER [CONTAINER...]
```

### 5. **컨테이너 삭제:**

```bash
docker rm [OPTIONS] CONTAINER [CONTAINER...]
```

- **옵션:**
  - `-f, --force`: 실행 중인 컨테이너를 강제로 삭제

### 6. **컨테이너 로그 확인:**

```bash
docker logs [OPTIONS] CONTAINER
```

- **옵션:**
  - `--follow`: 실시간으로 로그 출력
  - `--tail`: 마지막 N라인의 로그만 표시

### 7. **컨테이너 내부로 쉘 접속:**

```bash
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
```

- **옵션:**
  - `-it, --interactive --tty`: 대화형으로 실행

### 8. **컨테이너 정보 확인:**

```bash
docker inspect [OPTIONS] CONTAINER [CONTAINER...]
```

- **옵션:**
  - `--format`: 출력 형식 지정

### 9. **컨테이너에서 파일 복사:**

```bash
docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH
docker cp [OPTIONS] SRC_PATH CONTAINER:DEST_PATH
```

### 10. **컨테이너 일시 중지 및 재개:**

```bash
docker pause CONTAINER
docker unpause CONTAINER
```

### 11. **컨테이너 실행 명령 갱신:**

```bash
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
```

### 12. 모든 컨테이너 강제 삭제

```bash
docker container rm -f $(docker container ls -aq)
```

- 이 외에도 다양한 Docker 컨테이너 관련 명령어와 옵션이 있음
- `docker --help` 명령어를 사용하면 도움말을 확인할 수 있음