- Amazon OpenSearch는 Amazon ElasticSearch의 후속 서비스
	- 라이선스 문제로 이름이 변경됨
- 기본 키나 데이터베이스의 인덱스로만 데이터를 쿼리할 수 있는 DynamoDB와 비교해 보면 OpenSearch로는 부분적으로 일치하는 필드를 포함해 모든 필드를 검색할 수 있음
- 애플리케이션에서 검색 기능을 제공할 때 많이 사용되고 일반적으로 다른 데이터베이스를 보완하는 데 사용됨
- 검색에 사용되지만 OpenSearch를 사용하면 쿼리를 분석할 수도 있음
- OpenSearch를 생성하고 사용하려면 인스턴스의 클러스터를 생성해야 함
	- 서비리스 서비스가 아님
- 자체 쿼리 언어가 있어 SQL을 지원하지 않음
- Kinesis Data Firehose AWS IoT, CloudWatch Logs나 사용자 지정 애플리케이션의 데이터를 주입할 수 있음
- Cognito, IAM과 통합해 제공하는 보안을 통해 저장 데이터 암호화와 전송 중 암호화가 가능

## OpenSearch Patterns MongoDB

![oser](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/oser1.png)

- OpenSearch 서비스로 쿼리를 분석할 수 있음
- OpenSearch Dashboards로 OpenSearch 데이터를 시각화할 수 있음
- OpenSearch의 흔한 사용 패턴 예시
	- 데이터가 포함된 DynamoDB가 있고 사용자가 이 데이터를 삽입, 삭제, 업데이트
	- 모든 스트림을 DynamoDB Stream으로 보내면 Lambda 함수가 받아 Amazon OpenSearch에 실시간으로 데이터를 삽입
	- 이러한 과정을 통해 애플리케이션은 특정 항목을 찾을 수 있게 됨
		- 예를 들면 항목의 이름을 부분 검색해 항목 ID를 알아낼 수 있음
	- 항목 ID를 얻은 후에는 DynamoDB를 호출해 DynamoDB 테이블에서 전체 항목을 검색
- OpenSearch로 검색하지만 메인 소스 데이터는 DynamoDB에 남아 있음

## OpenSearch Patterns CloudWatch Logs

![oser](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/oser2.png)

- CloudWatch Logs로 OpenSearch에 데이터를 주입할 수도 있음
- 예시
	- 먼저 AWS Lambda 함수에 실시간으로 데이터를 전송하는 CloudWatch Logs 구독 필터를 사용
	- Lambda 함수는 Amazon OpenSearch에 실시간으로 데이터를 보냄
	- 아니면 CloudWatch Logs에서 구독 필터로 보낸 다음 Kinesis Data Firehose로 구독 필터의 데이터를 읽고 Amazon OpenSearch에 거의 실시간으로 데이터를 주입할 수도 있음

## OpenSearch Patterns Kinesis Data Streams & Kinesis Data Firehose

![oser](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/oser3.png)

- Kinesis를 사용한 패턴
- Kinesis Data Streams의 스트림을 Amazon OpenSearch로 보낼 때 두 가지 전략이 있음
- Kinesis Data Firehose
	- 근 실시간 유형의 서비스
	- Lambda 함수를 사용해 일부 데이터를 변환한 다음에 Amazon OpenSearch로 전송하는 옵션도 있음
- Kinesis Data Streams
	- 데이터 스트림을 실시간으로 읽는 Lambda 함수를 생성하고 커스텀 코드를 작성해서 Lambda 함수가 Amazon OpenSearch에 실시간으로 데이터를 쓰게 할 수도 있음
- 모두 유효한 패턴
