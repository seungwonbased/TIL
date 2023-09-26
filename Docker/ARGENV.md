# Arguments

> ARG

- **Build-time** Arguments
- 특정 Dockerfile 명령어로 다른 값을 추출하는 데 사용할 수 있는 변수를 설정할 수 있음
- Dockerfile에 다양한 값을 플러그인 하거나, 이러한 값을 Dockerfile에 하드 코딩 하지 않아도 이미지를 빌드할 때 다른 값을 플러그인 할 수 있게 함
- 예를 들면, Dockerfile ENV 환경 변수 설정을 통해 디폴트 포트로 80 포트를 지정하는 것은 하드코딩
	- 이런 값을 유연하게 만들고 싶다면 이미지를 만들 때 ARG를 받아 빌드타임에 사용할 수 있음
- Dockerfile에서 빌드타임에 사용되는 값은 ARG에서 받아온 값을 쓸 수 있음
	- $DEFAULT_PORT 처럼 쓰면 됨
	- CMD에서는 못 씀, 런타임에 실행되는 명령이기 때문

# Environment Variables

> ENV

- **Runtime** Environment Variables
- 보통은 Dockerfile에서 ENV 명령어로 환경변수가 존재함을 Docker에 알린 다음, docker run에서 -e, --env 옵션을 사용해 구체적인 값을 제공
	- 이 값을 애플리케이션 코드에서 사용할 수 있음
	- 또한 같은 Dockerfile 내에서도 $[환경변수명]으로 사용할 수 있음
- .env와 같은 외부 파일로도 작성 가능
	- --env-file 옵션을 사용해 파일을 지정

```dockerfile
PORT=80
SOURCE="0.0.0.0"
...
```

## ENV 보안

- 보안 데이터를 Dockerfile에 포함시키지 않고, 별도의 환경 변수 파일에 저장해야 함
- 그렇지 않으면 값이 이미지에 포함되고 docker history를 통해 해당 값을 읽을 수 있음
- 별도의 파일을 사용하면 이미지의 일부분이 아니기 때문에 보안적으로 안전
- 자격 증명, 개인 키 등의 경우에는 조심

> 💡 참고
> ARG, ENV에 값이 주어지면, 리빌드할 때 해당 라인 이후 레이어는 전부 다시 리빌드
> 그러므로 종속성을 설치하거나 하는 작업은 Dockerfile에서 ARG, ENV 위에 적어주는 게 좋음

