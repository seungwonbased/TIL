# Amazon EventBridge

- Amazon EventBridge의 예전 이름은 CloudWatch Events
- Amazon EventBridge의 기능은 다양함
	- Schedule
		- 클라우드에서 CRON 작업을 예약할 수 있음
		- 스크립트를 예약할 수 있음
		- 가령 한 시간마다 Lambda 함수를 트리거해서 스크립트를 실행할 수 있음
		- 한 시간마다 이벤트가 생성되므로 Amazon EventBridge라고 부름
	- Event Pattern
		- 한 시간마다 작업을 예약하는 것뿐만 아니라 이벤트 패턴에 반응할 수도 있음
		- 특정 작업을 수행하는 서비스에 반응하는 이벤트 규칙이 있음
		- 예를 들어 콘솔의 IAM 루트 사용자 로그인 이벤트에 반응할 수 있음
		- 이벤트가 발생하면 SNS 주제로 메시지를 보내고 이메일 알림을 받을 수 있음
		- 누군가 루트 계정을 사용하면 이메일을 받을 수 있으니 계정 보안 기능으로 훌륭
		- 또한 대상(destination)이 다양하다면 Lambda 함수를 트리거해서 SQS, SNS 메시지 등을 보낼 수 있음

## Amazon EventBridge Rules

![aeb](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/aeb1.png)

- Amazon EventBridge가 중앙에 위치하고 Amazon EventBridge로 보낼 다양한 소스가 있음
	- 시작, 중단, 종료할 때는 EC2 Instance에서 실패한 빌드는 CodeBuild에서, 객체 업로드 이벤트가 발생할 때는 S3 Event에서, 계정의 새 보안 문제는 Trusted Advisor에서 가져옴
	- EventBridge와 CloudTrail을 결합하는 건 좋은 조합
		- AWS 계정에서 생성된 API 호출을 모두 가로채므로 아주 유용
		- 일정 및 CRON 작업도 예약할 수 있음
			- 네 시간마다 혹은 매월 첫 번째 월요일 오전 8시에 실행되게 할 수 있음
			- 이것도 소스가 될 수 있음
- Amazon EventBridge로 전송되는 이벤트에 필터를 설정할 수 있음
	- 예를 들어 Amazon S3에 있는 특정 버킷의 이벤트만 필터링하도록 하면 Amazon EventBridge는 이벤트의 세부 사항을 나타내는 JSON 문서를 생성할 것임
	- 어떤 인스턴스가 이 ID로 실행되는지 등을 나타내고 시간, IP 등의 정보가 담김
	- JSON 문서 생성이 끝나면 이벤트들을 다양한 대상으로 전송할 수 있고 유용한 통합 기능을 사용할 수 있음
	- Lambda 함수를 트리거하는 일정을 예약하거나, AWS Batch에서 배치 작업 예약을 하거나 Amazon ECS에서 ECS 작업을 실행할 수 있음
	- SQS, SNS나 Kinesis Data Streams로 메시지를 보낼 수도 있음
	- 단계 함수를 실행할 수도 있고, CodePipeline으로 CI/CD 파이프라인을 시작하거나 CodeBuild에서 빌드를 실행할 수 있음
	- 이렇게 다양한 서비스를 모두 알 필요는 없고, 가능한 작업을 개괄적으로 보여주는 것
	- SSM 자동화를 실행하거나 EC2 가동, 중지, 재개와 같은 특정 EC2 작업을 실행할 수도 있음
	- 가능한 작업이 끝도 없고 사용 사례에 따라 달라짐

## Amazon EventBridge

![aeb](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/aeb2.png)

- 지금 살펴본 Amazon EventBridge는 기본 이벤트 버스
	- 이벤트를 기본 이벤트 버스로 전송하는 AWS의 서비스를 말함
- Amazon EventBridge의 능력은 더 뛰어남
- 파트너 이벤트 버스라는 서비스가 있음
	- 파트너와 통합된 AWS 서비스를 말하는데, 대부분의 파트너는 소프트웨어 서비스 제공자
	- 이 파트너들은 파트너 이벤트 버스로 이벤트를 전송
		- Zendesk 혹은 Datadog, Auth0 등을 사용할 수 있고 파트너 목록을 확인하면 알 수 있음
	- 이런 특정 파트너 이벤트 버스로 이벤트를 전송할 수 있고 계정을 통해 AWS 외부에서 일어나는 변화에 반응할 수 있게 되는 것
- 마지막은 사용자 지정 이벤트 버스
	- 자체 버스를 만들 수 있음
	- 애플리케이션의 자체 이벤트를 사용자 지정 이벤트 버스로 전송
- Amazon EventBridge 규칙 덕분에 다른 이벤트 버스처럼 여러 대상으로 이벤트를 보낼 수 있음
	- 리소스 기반 정책을 사용해 다른 계정의 이벤트 버스에 액세스할 수도 있음
- 이벤트 아카이빙도 가능
	- 전부 또는 필터링된 서브셋을 아카이빙할 수 있고 이벤트를 아카이빙할 때는 보존 기간을 무기한이나 일정 기간으로 설정할 수 있음
	- 이를 통해 아카이브된 이벤트를 재생할 수 있음
	- 예를 들어 Lambda 함수의 버그를 수정할 때 수정 후에 이벤트를 다시 테스트하고 재생해야함
		- 이때 아카이브된 이벤트를 재생하면 됨
		- 디버깅에 아주 유용
		- 트러블 슈팅이나 프로덕션을 수정할 때도 유용

## Amazon EventBridge - Schema Registry

![aeb](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/aeb3.png)

- EventBridge는 여러 곳에서 이벤트를 받음
	- 그러므로 이벤트가 어떻게 생겼는지 파악해야 함
- 이벤트는 JSON 형식
- '스키마 레지스트리'라는 것이 있는데, Amazon EventBridge는 버스의 이벤트를 분석하고 스키마를 추론하는 능력이 있음
	- 스키마 레지스트리의 스키마를 사용하면 애플리케이션의 코드를 생성할 수 있고 이벤트 버스의 데이터가 어떻게 정형화되는지 미리 알 수 있음
- 특정 코드 파이프라인 작업의 스키마 예시
	- 주황색 버튼을 눌러 코드를 다운로드하면 EventBridge가 이벤트 버스로부터 스키마 추론 방법과 데이터의 정형화 방식을 파악
- 스키마를 버저닝할 수도 있어 애플리케이션의 스키마를 반복할 수 있음

## Amazon EventBridge - Resource-based Policy

![aeb](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/aeb4.png)

- EventBridge의 리소스 기반 정책
- 특정 이벤트 버스의 권한을 관리할 수 있음
	- 예를 들면 특정 이벤트 버스가 다른 리전이나 다른 계정의 이벤트를 허용하거나 거부하도록 설정
- 리소스 기반 정책의 사용 사례
	- 여러 계정의 모음인 AWS Organizations의 중앙에 이벤트 버스를 두고 모든 이벤트를 모음
	- central-event-bus 계정에 다른 계정이 이벤트를 보낼 수 있도록 리소스 기반 정책을 추가하면 오른쪽에 있는 다른 계정에서 PutEvents 작업을 통해 이벤트를 central-event-bus로 전송할 수 있음
