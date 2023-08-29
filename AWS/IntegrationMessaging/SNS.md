# Amazon SNS

![sns1](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/sns1.png)

- 메시지 하나를 여러 수신자에게 보낸다고 가정
	- 이런 경우 직접 통합(Direct integration)을 쓸 수 있음
	- 구매 서비스 애플리케이션을 예로 들자면 이메일 알림을 보내고 사기 탐지 서비스와 배송 서비스,그리고 SQS 대기열에도 메시지를 보낼 수 있음
- 하지만 수신 서비스를 새로 추가할 때마다 통합을 생성하고 작성해야 하므로 번거로울 수 있음
- 대신 Pub/Sub 즉, 게시/구독이라는 것을 사용할 수 있음
	- 이를 통해 구매 서비스가 메시지를 SNS 주제로 전송할 수 있음
	- 메시지를 주제로 게시하는 것
	- 해당 주제에는 많은 구독자가 있으며 각 구독자는 SNS 주제에서 해당 메시지를 수신하고 이를 보관할 수 있음

## Amazon SNS

![sns2](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/sns3.png)

- Amazon SNS에서 "이벤트 생산자"는 한 SNS 주제에만 메시지를 보냄
- "이벤트 수신자" 또는 구독자는 해당 주제와 관련한 SNS 알림을 받으려는 사람
- 따라서 SNS 주제 구독자는 해당 주제로 전송된 메시지를 모두 받게 됨
- 메시지를 필터링하는 기능을 사용하는 경우에도 메시지를 받을 수 있음
- 주제별 최대 구독자 수 최대 1,200만 이상
	- 이 숫자는 추후 변경될 수 있음
- 계정당 가질 수 있는 주제 수는 최대 10만 개
	- 더 늘릴 수도 있고, 변경될 수 있음
- SNS에서 구독자에게 게시할 수 있는 것
	- SNS에서 직접 이메일을 보낼 수 있음
	- SMS 및 모바일 알림을 보낼 수 있음
	- 지정된 HTTP 또는 HTTPS 엔드 포인트로 직접 데이터를 보낼 수 있음
	- SNS는 SQS와 같은 특정 AWS 서비스와 통합하여 메시지를 대기열로 직접 보낼 수 있음
	- 메시지를 수신한 후 함수가 코드를 수행하도록 Lambda에 보낼 수 있음
	- Firehose를 통해 데이터를 Amazon S3나 Redshift로 보낼 수 있음

## SNS integrates with a lot of AWS services

![sns3](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/sns2.png)

- SNS는 다양한 AWS 서비스에서 데이터를 수신하기도 함
	- SNS로 직접 데이터를 보냄
- SNS로 데이터를 보낼 수 있는 AWS 서비스
	- CloudWatch 경보
	- Auto Scaling 그룹 알림
	- CloudFormation State Changes
	- Budgets
	- S3 버킷
	- DMS
	- Lambda
	- DynamoDB
	- RDS 이벤트
	- 등등
	- 서비스명은 기억하지 않아도 됨
	- AWS에서 알림이 발생하면 이러한 서비스가 지정된 SNS 주제로 알림을 보낸다는 것만 기억하면 됨

## SNS – How to publish

- SNS 게시 방법
	- SDK 주제 게시를 사용해 SNS에 메시지를 게시할 수 있음
		- 먼저 주제를 만든 다음 하나 또는 여러 개의 구독을 만들고 SNS 주제에 게시
		- 그럼 모든 구독자가 자동으로 해당 메시지를 받게됨
	- 혹은 모바일 앱 SDK 전용 직접 게시 방법
		- 플랫폼 애플리케이션을 만든 다음 플랫폼 엔드 포인트를 만들고 플랫폼 엔드 포인트에 게시
		- 수신 가능 대상은 Google, GCM, Apple APNS 또는 Amazon ADM 구독자
		- 모두 모바일 애플리케이션으로 알림을 수신

## SNS - Security

- 보안 측면에서 Amazon SNS는 SQS와 동일
- Encryption
	- 전송 중 암호화
	- KMS 키를 사용한 저장 데이터 암호화
	- 클라이언트가 SNS에 암호화된 메시지를 보내려는 경우를 위한 클라이언트 측 암호화
		- 암호화 및 암호 해독은 클라이언트 몫
- Access Control
	- 액세스 제어는 IAM 정책 중심
		- 모든 SNS API가 IAM 정책으로 규제되기 때문
- SNS Access Policies
	- S3 버킷 정책과 매우 유사
	- SNS 주제에 교차 계정 액세스 권한을 갖거나 S3 이벤트와 같은 서비스가 SNS 주제에 작성할 수 있도록 허용하려는 경우 매우 유용

# SNS + SQS
## Fan Out Pattern

![sns4](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/sns4.png)

- SNS+SQS 팬아웃 패턴
- 메시지를 여러 SQS 대기열로 보내고 싶은데 모든 SQS 대기열에 개별적으로 메시지를 보내면 문제가 발생할 수 있음
	- 예를 들어 애플리케이션이 중간에 비정상적으로 종료될 수도 있고 전달에 실패한다거나 SQS 대기열이 더 추가될 수도 있음
	- 이런 경우에 이 팬아웃 패턴을 사용
- 일단 SNS 주제에 메시지를 전송한 후 원하는 수의 SQS 대기열이 이 SNS 주제를 구독하게 하는 것
	- 이 대기열들은 구독자로서 SNS로 들어오는 모든 메시지를 받게됨
- 예시
	- 구매 서비스가 있고 두 개의 SQS 대기열로 메시지를 보낸다고 가정
	- 이때 직접 보내는 대신 메시지를 하나의 SNS 주제로 보내고 대기열들이 이 SNS 주제를 구독하여 사기 탐지 서비스나 배송 서비스가 각자의 SQS 대기열에서 모든 메시지를 읽어 들이게 함
	- 이는 완전히 분리된 모델이며 데이터도 손실되지 않음
	- SQS로 작업을 다시 시도할 수 있을 뿐 아니라 데이터 지속성, 지연 처리도 수행할 수 있음
	- 게다가 이런 방식으로 SNS 주제를 구독하도록 더 많은 SQS 대기열을 추가할 수도 있음
	- 이를 위해서는 앞서 살펴봤듯이 SQS 액세스 정책에서 SNS 주제가 SQS 대기열에 쓰기 작업을 할 수 있도록 허용해야 함
	- 리전 간 전달도 가능
		- 즉 보안상 가능하다면 한 리전의 SNS 주제에서 다른 리전의 SQS 대기열로 메시지를 보낼 수 있음

### Application: S3 Events to Multiple Queues

![sns5](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/sns5.png)

- 팬아웃 패턴을 다른 목적으로 사용하는 사례
- 이벤트 세 개를 여러 대기열에 넣는 경우를 가정
- 먼저 S3 이벤트 규칙에 제한 조건이 있음
	- 객체 생성과 같은 이벤트 유형과 /images와 같은 접두사 조합이 동일하다면 S3 이벤트 규칙은 한 가지여야 함
- 만약 여러 대기열에 동일한 S3 이벤트 알림을 보내고 싶다면 팬아웃 패턴을 사용해야 함
- 예를 들어 S3 객체를 생성하여 S3 버킷에 이벤트를 형성하고 이 이벤트를 SNS 주제로 전송한 후 팬아웃 패턴으로 많은 SQS 대기열이 SNS 주제를 구독하게 함
	- 물론 다른 유형의 애플리케이션, 이메일, Lambda 함수 등도 구독할 수 있음
- 이런 식으로 팬아웃 패턴 덕분에 Amazon S3에서 발생하는 이벤트의 메시지가 여러 다른 목적지에 도달할 수 있게됨

### Application: SNS to Amazon S3 through Kinesis Data Firehose

![sns6](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/sns6.png)

- 다른 아키텍처로, Kinesis Data Firehose (KDF)를 통해 SNS에서 Amazon S3로 직접 데이터를 전송할 수 있음
- SNS를 KDF에 직접 연결하여 구매 서비스에서 데이터를 SNS 주제로 전송할 수 있음
- 그런 다음 Kinesis Data Firehose, 즉 KDF에서 해당 정보를 수신하고 해당 KDF에서 Amazon S3 버킷으로 전달하거나 특정한 KDF 목적지로 어디든 전달할 수 있음
- 이런 방식으로 SNS 주제의 메시지를 계속 이어가도록 확장할 수 있음

### SNS – FIFO Topic

![sns7](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/sns7.png)

- 이 패턴은 FIFO 주제에도 적용할 수 있음
- Amazon SNS에는 주제의 메시지 순서를 지정하는 FIFO, 즉 선입 선출 기능이 있음
- 생산자가 메시지 1, 2, 3, 4를 보내고 구독자는 1, 2, 3, 4의 순서로 메시지를 수신하는 SQS FIFO 대기열이 됨
- SNS FIFO는 SQS FIFO와 유사
	- 메시지 그룹 ID에 따라 순서를 매기고 중복 제거 ID를 활용하거나 내용을 비교하여 중복 데이터를 제거하며 SQS FIFO 대기열을 FIFO SNS 주제의 구독자로 설정
	- 처리량은 제한적이며 SQS FIFO 대기열과 동일
	- SQS FIFO 대기열만 SNS FIFO 주제를 읽어 들일 수 있기 때문

### SNS FIFO + SQS FIFO - Fan Out

- 필요한 이유
	- SQS FIFO를 활용하여 팬아웃을 수행하려면 팬아웃, 순서, 중복 제거가 필요
	- 이렇게 구매 서비스가 데이터를 SNS FIFO 주제로 전달하고 두 개의 SQS FIFO 대기열로 팬아웃한 후 사기 탐지 서비스와 배송 서비스가 이 FIFO 대기열에서 데이터를 읽어 들임

### SNS - Message Filtering

![sns8](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/sns8.png)

- SNS에서 메시지 필터링을 할 수 있음
- 메시지 필터링이란
	- SNS 주제를 구독할 때 전송되는 메시지를 필터링하는 데 사용되는 JSON 정책
	- 구독에 어떤 필터링 정책도 없다면 모든 메시지를 받아들이며 이것이 기본 동작
- 메시지 필터링 정책이 있을 때 어떤 일이 발생하는지 예시
	- 구매 서비스가 있고 이 서비스에서 SNS 주제로 트랜잭션을 보낸다고 가정
	- 이 트랜잭션에는 순서가 있고 제품은 연필이며 개수는 네 개로 발주 완료 상태
	- 여기에서 모든 주문이 아니라 발주된 주문만 골라서 SQS 대기열을 만들고자 한다면 SQS 대기열이 SNS 주제를 구독하게 하고 JSON으로 필터링 정책을 적용해야 함
	- 이 정책에서 상태를 ‘발주 완료’로 지정하면 여기에 해당하는 메시지만 SQS 대기열에 들어가게 됨
	- 반면 취소된 주문을 고르는 필터링 정책을 만들면 전과 동일한 SNS 주제에서 취소된 주문에 해당하는 SQS 대기열을 만들 수 있음
	- 이렇게 발주된 주문과 취소된 주문에 해당하는 SQS 대기열에는 같은 메시지가 도달하지 않음
	- 또한 이 취소된 주문을 골라내는 필터링 정책을 똑같이 사용해서 취소된 주문에 해당하는 이메일 구독을 생성할 수 있음
	- 또한 거절된 주문에 해당하는 필터링 정책과 SQS 대기열도 만들 수 있고 아무 필터링 정책 없이 SNS 주제에 있는 모든 메시지를 받는 SQS 대기열도 만들 수 있음
