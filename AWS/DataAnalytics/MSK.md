# Amazon Managed Streaming for Apache Kafka (MSK)

- Apache Kafka용 Amazon 관리형 스트리밍 서비스로 Amazon MSK로 줄여 부름
- Kafka는 Amazon Kinesis의 대안
	- 두 서비스 모두 데이터를 스트리밍
- MSK는 AWS의 완전 관리형 Kafka 클러스터 서비스로 그때그때 클러스터를 생성, 업데이트, 삭제
	- MSK는 클러스터 내 브로커 노드와 Zookeeper 브로커 노드를 생성 및 관리하고 고가용성을 위해 VPC의 클러스터를 최대 세 개의 다중 AZ 전역에 배포
- 일반 Kafka 장애를 자동 복구하는 기능이 있으며 EBS 볼륨에 데이터를 저장할 수도 있음
- Amazon MSK 서버리스를 사용할 수도 있음
	- MSK에서 Apache Kafka를 실행하지만 서버 프로비저닝이나 용량 관리가 필요 없음
	- MSK가 리소스를 자동으로 프로비저닝하고 컴퓨팅과 스토리지를 스케일링

## Apache Kafka at a High-level

![msk](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/msk1.png)

- Apache Kafka는 데이터를 스트리밍하는 방식
- Kafka 클러스터는 여러 브로커로 구성되고 데이터를 생산하는 생산자는 Kinesis, IoT, RDS 등의 데이터를 클러스터에 주입
- Kafka 주제로 데이터를 전송하면 해당 데이터는 다른 브로커로 완전 복제됨
	- Kafka 주제는 실시간으로 데이터를 스트리밍하고 소비자는 데이터를 소비하기 위해 주제를 폴링
- 소비자는 데이터로 원하는 대로 처리하거나 EMR, S3, SageMaker, Kinesis RDS 등의 대상으로 보냄
- Kafka는 Kinesis와 유사하지만 몇 가지 차이점이 있음

## Kinesis Data Streams vs Amazon MSK

- Kinesis Data Streams
	- 1MB의 메시지 크기 제한
	- 샤드로 데이터를 스트리밍
	- 용량을 확장하려면 샤드 분할, 축소하려면 샤드 병합
	- TLS 전송 중 암호화 기능이 있음
	- 저장 데이터 암호화가 가능

- Amazon MSK
	- 1MB이 기본값이고, 더 큰 메시지 보존을 위해 10MB로 설정할 수도 있음
	- 파티션을 이용한 Kafka 주제를 사용
	- 파티션 추가로 주제 확장만 할 수 있음 (삭제는 불가능)
	- 평문과 TLS 전송 중 암호화 기능이 있음
	- 저장 데이터 암호화가 가능
- 참고로 몇 가지 차이점 외에도 Amazon MSK에서는 원한다면 1년 이상 데이터를 보관할 수도 있음
	- EBS 스토리지 비용을 지불하면 됨

## Amazon MSK Consumers

![msk](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/msk2.png)

- MSK에 데이터를 생산하려면 Kafka 생산자를 생성해야 함
- MSK의 데이터를 소비하는 방법은 여러 가지
	- Apache Flink용 Kinesis Data Analytics를 사용해 Apache Flink 앱을 실행하고 MSK 클러스터의 데이터를 읽어 오면 됨
	- AWS Glue로 ETL 작업을 스트리밍해도 됨
		- Glue는 Apache Spark Streaming으로 구동됨
	- Amazon MSK를 이벤트 소스로 이용하려면 Lambda 함수를 사용할 수 있음
	- 자체 Kafka 소비자를 생성해 원하는 플랫폼에서 실행할 수도 있음
		- 예를 들어 Amazon EC2 인스턴스나 EC2 클러스터, EKS 클러스터에서 실행
