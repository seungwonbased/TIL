# API Gateway
## Example: Building a Serverless API

![apg](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/apg1.png)

- Lambda 함수에서 API의 데이터베이스로 DynamoDB를 사용할 수 있으며 테이블을 생성, 읽기, 업데이트 삭제할 수 있음
- 클라이언트도 이 Lambda 함수를 지연 호출(Invoke)하게 하고 싶음
	- 클라이언트가 직접 Lambda 함수를 지연 호출할 수 있게 하려면 클라이언트에게 IAM 권한이 있어야 함
	- 클라이언트와 Lambda 함수 사이에 애플리케이션 로드 밸런서를 배치하면 Lambda 함수가 HTTP 엔드포인트에 노출됨
- API Gateway를 사용하는 방법도 있음
	- AWS의 서버리스 서비스로 REST API를 생성할 수 있으므로 클라이언트가 퍼블릭 액세스할 수 있음
	- API Gateway에 클라이언트가 직접 소통하며 다양한 작업을 할 수 있고 Lambda 함수에 요청을 프록시
- API Gateway를 사용하는 이유는 HTTP 엔드포인트뿐만 아니라 다양한 기능 예를 들어 인증부터 사용량 계획, 개발 단계 등의 기능을 제공하기 때문

## AWS API Gateway

- API Gateway는 Lambda와 통합하면 완전 서버리스 애플리케이션이 구축되므로 인프라 관리가 필요 없음
- WebSocket 프로토콜을 지원하므로 API Gateway로 두 가지 방법의 실시간 스트리밍이 가능
- API 버저닝을 핸들링하므로 버전 1, 2, 3이 생겨도 클라이언트 연결이 끊기지 않음
- dev, test, prod 등 여러 환경을 핸들링
- 보안에도 활용할 수 있음
- 인증, 권한 부여 등 수많은 보안 기능을 API Gateway에 활성화할 수 있음
- API 키를 생성할 수 있고 API Gateway에 클라이언트 요청이 과도할 때 요청을 스로틀링할 수 있음
- Swagger나 Open API 3.0과 같은 공통 표준을 사용하여 신속히 API를 정의하여 가져올 수 있음
	- Swagger나 Open API로 내보낼 수도 있음
- API Gateway 수준에서 요청과 응답을 변형하거나 유효성을 검사해 올바른 호출이 실행되게 할 수 있음
- SDK나 API 스펙을 생성하거나 API 응답을 캐시할 수도 있음

## API Gateway - Integrations High Level

- API Gateway가 지원하는 통합 유형
- Lambda 함수
	- Lambda 함수를 지연 호출
	- Lambda 함수를 사용하는 REST API를 완전 서버리스 애플리케이션에 노출시키는 가장 일반적이고 간단한 방법
- HTTP
	- 백엔드의 HTTP의 엔드포인트를 노출시킬 수 있음
	- 온프레미스에 HTTP API가 있거나 클라우드 환경에 애플리케이션 로드 밸런서가 있을 때 API Gateway를 활용하면 속도 제한 기능, 캐싱, 사용자 인증, API 키 등의 기능을 추가할 수 있음
	- HTTP 엔드포인트에서 API Gateway 계층을 활용
- AWS 서비스
	- 어떤 AWS API라도 노출시킬 수 있음
	- 단계 함수 워크플로우를 시작할 수 있고 API Gateway에서 직접 SQS에 메시지를 게시할 수도 있음
	- 인증을 추가하거나 API를 퍼블릭으로 배포하거나 특정 AWS 서비스에 속도 제한을 추가하기 위해 통합

### API Gateway - AWS Service Integration Kinesis Data Streams Example

![apg](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/apg2.png)

- AWS 서비스에 API Gateway를 사용하는 예시
- Kinesis Data Streams
- Kinesis Data Streams에 사용자가 데이터를 전송할 수는 있지만 AWS 자격 증명은 가질 수 없도록 보안 설정을 할 수 있음
	- Kinesis Data Streams과 클라이언트 사이에 API Gateway를 두는 것
- 클라이언트가 API Gateway로 HTTP 요청을 보내면 Kinesis Data Streams에 전송하는 메시지로 구성해 전송
- 따로 서버를 관리할 필요가 없음
- Kinesis Data Streams에서 Kinesis Data Firehose로 레코드를 전송하고 최종적으로 JSON 형식으로 Amazon S3 버킷에 저장

## API Gateway - Endpoint Types

- API Gateway의 장점은 API Gateway를 통해 AWS 서비스를 외부에 노출시키는 것
- API Gateway 배포 방법은 세 가지이며, 이를 엔드포인트 유형이라고 함
- 엣지 최적화 (Default)
	- 글로벌 클라이언트용
	- 전 세계 누구나 API Gateway에 액세스할 수 있음
	- 모든 요청이 CloudFront 엣지 로케이션을 통해 라우팅되므로 지연 시간이 개선됨
	- API Gateway는 여전히 생성된 리전에 위치하지만 모든 CloudFront 엣지 로케이션에서 액세스될 수 있음
- Regional
	- CloudFront 엣지 로케이션을 원하지 않을 때는 리전 배포를 사용
	- 모든 사용자는 API Gateway를 생성한 리전과 같은 리전에 있어야 함
	- 자체 CloudFront 배포를 생성할 수도 있음
		- 엣지 최적화 배포와 동일한 결과를 내며 캐싱 전략과 CloudFront 설정에 더 많은 권한을 가질 수 있음
- Private
- 프라이빗 API Gateway는 VPC 내에서만 액세스할 수 있음
	- ENI 같은 인터페이스 VPC 엔드포인트를 사용해 액세스
- API Gateway에 액세스를 정의할 때는 리소스 정책을 사용

## API Gateway - Security

- API Gateway의 보안
- 사용자를 식별하는 방법에는 여러 가지가 있음
	- IAM 역할을 사용
		- IAM 역할
			- 내부 애플리케이션에 유용
		- Amazon Cognito
			- 모바일 애플리케이션이나 웹 애플리케이션의 외부 사용자에 대한 보안 조치
		- Custom Authorizer
			- 자체 로직을 실행
			- Lambda 함수
	- HTTPS 보안
		- 사용자 지정 도메인 이름을 AWS Certificate Manager, 즉 ACM과 통합할 수 있음
		- 엣지 최적화 엔드포인트를 사용할 경우 인증서는 us-east-1에 있어야 하고 리전 엔드포인트를 사용한다면 인증서는 API Gateway 단계와 동일한 리전에 있어야 함
		- 끝으로 Route 53에 CNAME이나 A-별칭 레코드를 설정해 도메인 및 API Gateway를 가리키도록 해야 함

