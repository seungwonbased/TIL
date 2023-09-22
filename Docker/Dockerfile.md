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
|VOLUMNE|외부 마운트 포인트 생성|
|USER|RUN, CMD, ENTRYPOINT를 실행하는 사용자|
|**WORKDIR**|작업 디렉토리 설정|
|ARGS|빌드 타임 환경변수 생성|
|LABEL|key-value 데이터|
|ONBUILD|다른 빌드의 베이스로 사용될 떄 사용하는 명령어|

- **FROM**
	- Base Image를 가져옴
	- Full 이미지 이름은 레지스트리, 베이스 이미지 이름, 태그, 네임스페이스 등으로 이뤄짐
	- e.g., FROM    golang:1.9
		- 레지스트리가 없으면 디폴트로 Dockerhub
		- 태그가 없으면 디폴트로 Latest
- **RUN**
	- 컨테이너에서 실행할 명령어를 지정
	- e.g., RUN    mkdir /echo
- **COPY**
	- 호스트의 파일을 컨테이너에 복사
	- e.g., COPY    main.go /echo
- **CMD**
	- 컨테이너 실행 시 실행할 명령어
	- e.g., CMD    ["go", "run", "/echo/main.go"]

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

#### WORKDIR
- 작업 디렉터리를 지정 

#### EXPOSE
- 컨테이너에서 노출할 포트
- -p 호스트 포트:컨테이너 포트
	- 호스트 포트를 생략하면 자동으로 할당됨
- -P는 -p 80과 같음

#### CMD vs ENTRYPOINT
- entrypoint가 설정되지 않았다면 cmd에 설정된 명령어를 그대로 실행
- entrypoint가 설정되었다면 cmd는 단지 entrypoint에 대한 인자의 기능을 수행

## Dockerfile 작성 시 유의사항

1. 빌드하는 과정에서 사용자 입력이 발생하지 않도록 해야 함
	- ENV DEBIAN_FRONTEND=noninteractive
		- 예를 들어 우분투 설치할 때 타임존 같은 걸 입력해야 하는 걸 방지
		- 참고: https://www.cyberciti.biz/faq/explain-debian_frontend-apt-get-variable-for-ubuntu-debian/
	- 혹은 apt-get install -y 옵션같은 걸 줘야함
2. 컨테이너 실행 시 포그라운드 프로세스로 실행되도록 해야 함
	- CMD apachectl -DFOREGROUND와 같은 옵션을 줘서 프로세스가 포그라운드로 실행되어야 컨테이너가 중지되지 않음
	- 예시
		- 아파치 서버가 컨테이너의 우분투 위에서 백그라운드 프로세스로 동작하면 우분투의 bash에 다시 제어권이 넘어옴
		- bash가 처리할 일이 끝났으므로 bash가 종료되어 제어권이 호스트로 반환됨
		- 컨테이너가 종료됨

