# Micro Service Architecture

- 많은 서비스가 REST API를 통해 상호작용
- 마이크로서비스의 아키텍처는 모습과 형태가 달라 뭐든 원하는 대로 할 수 있음
- 마이크로서비스 아키텍처를 쓰려는 이유
	- 서비스 개발 수명 주기를 줄이고자 함
	- 각 서비스가 독립적으로 확장하며 코드 리포지토리를 갖추길 원한다면 마이크로서비스 아키텍처를 사용

## Micro Services Environment

![micro](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/micro1.png)

- 사용자는 첫 마이크로서비스와 HTTPS를 통해 통신한다고 가정
- 일래스틱 로드 밸런서가 ECS와 통신하고 난 뒤에 DynamoDB와 통신하도록 함
- 마이크로서비스는 보통 DNS 이름이나 URL이 있음
- 첫 번째 서비스
	- service1.example.com
	- 정보를 얻으려면 DNS 쿼리를 Route 53에 보내 별칭 레코드를 받고 나서 서비스와 상호작용이 가능
- 두 번째 서비스
	- service2.example.com
	- 전형적인 서버리스용 아키텍처를 사용
	- 하지만 DynamoDB 대신 일래스티 캐시가 있음
	- 그냥 재미 삼아 이것저것 섞어서 사용하려면 Lambda의 백엔드로 일래스티 캐시를 써도 괜찮음
	- 두 번째 서비스는 첫 번째 서비스와 상호작용 할 수도 있음
		- 일래스틱 로드 밸런서에 람다 함수가 호출을 보내 첫 마이크로서비스에서 정보를 얻고 응답을 생성
- 세 번째 서비스
	- ELB를 사용
	- 이건 서버리스가 아니라 Amazon EC2 오토 스케일링과 Amazon RDS 데이터베이스를 사용하는 거라 전에 봤던 전형적인 아키텍처에 해당
	- EC2 인스턴스가 결정을 내리기 전에 두 번째 마이크로서비스를 호출해야 함
	- URL은 service3.example.com입니다

## Discussions on Micro Services

- 마이크로서비스에는 두 가지 패턴이 있음
- 동기식 패턴 (Synchronous Patterns)
	- 다른 마이크로서비스를 명시적으로 호출
	- API Gateway와 로드 밸런서는 다른 마이크로서비스에 HTTPS 호출을 보내기에 적합
- 비동기식 패턴 (Asynchronous Patterns)
	- S3처럼 SQS, Kinesis, SNS Lambda에 트리거로 작용하는 경우
	- 예시: SQS에 메시지를 남기지만 응답을 언제 받을지 내용이 뭔지 또 무슨 일이 일어날지 개의치 않음
- 마이크로서비스의 문제
	- 새로운 마이크로서비스를 생성할 때마다 오버헤드가 발생한다는 점
	- 서버 밀도나 사용률을 최적화하는 데 어려움을 겪기도 함
	- 여러 버전의 마이크로서비스를 동시에 가동하려면 복잡
	- 여러 서비스와 통합하려다 클라이언트 코드 요구사항이 급증하기도 함
	- 하지만 이런 문제는 서버리스 패턴으로 어느 정도 해결됨
		- API Gateway나 Lambda는 자동으로 확장
			- 사용량만큼 돈만 내면 되니까 서버 사용률을 걱정할 필요가 없어짐
		- API Gateway에서 API를 쉽게 복제하고 재생산하며 API Gateway를 위한 Swagger 통합으로 클라이언트 SDK 생성이 가능
- 정리해 보자면 마이크로서비스는 AWS를 사용해 원하는 대로 설계할 수 있는데 문제를 해결해 주기도 하지만 말썽을 일으킬 때도 있음
