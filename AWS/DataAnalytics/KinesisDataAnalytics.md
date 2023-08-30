# Kinesis Data Analytics for SQL Applications

![knda](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/knda1.png)

- SQL 애플리케이션용 Kinesis Data Analytics부
- 중앙에 위치하여 Kinesis Data Streams와 Kinesis Data Firehose 데이터 소스에서 데이터를 읽음
- 둘 중 한군데서 데이터를 읽어 온 다음 SQL 문에 적용하여 실시간 분석을 처리할 수 있음
- Amazon S3 버킷의 데이터를 참조해 참조 데이터를 조인할 수도 있음
	- 실시간 데이터가 풍성해짐
- 그리고 여러 대상에 데이터를 전송
- 두 가지 방법
	- Kinesis Data Streams는 Kinesis Data Analytics의 실시간 쿼리로 스트림을 생성
		- EC2에서 실행하는 애플리케이션이나 AWS Lambda로 스트리밍하는 데이터를 실시간으로 처리할 수 있음
	- Kinesis Data Firehose로 바로 전송할 수도 있음
		- Amazon S3 Amazon Redshift이나 Amazon OpenSearch 또는 기타 Firehose 대상에 전송됨

## Kinesis Data Analytics (SQL Application)

- 데이터 소스로는 Kinesis Data Streams와 Firehose가 있음
- Amazon S3로 데이터를 강화할 수 있고 완전 관리형 서비스이므로 서버를 프로비저닝하지 않음
- 오토 스케일링이 가능하며 Kinesis Data Analytics에 전송된 데이터만큼 비용을 지불
- Kinesis Data Streams나 Kinesis Data Firehose에 데이터를 출력할 수 있음
- 사용 사례로는 시계열 분석과 실시간 대시보드와 실시간 지표가 있음

# Kinesis Data Analytics for Apache Flink

![knda](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/knda2.png)

- Apache Flink용 Kinesis Data Analytics
- 이름에서 알 수 있듯이 Kinesis Data Analytics에서 Apache Flink를 사용할 수 있음
- Apache Flink를 사용하면 Java, Scala, SQL로 애플리케이션을 작성하고 스트리밍 데이터를 처리, 분석할 수 있음
	- 첫 번째 유형과 유사해 보이지만 다름
	- Flink는 코드로 작성해야 하는 특별한 애플리케이션
	- Flink 애플리케이션을 Kinesis Data Analytics의 Flink 전용 클러스터에서 실행할 수 있음
		- 백그라운드에서
- Apache Flink을 사용해 두 개의 메인 데이터 소스인 Kinesis Data Streams나 Amazon MSK의 데이터를 읽을 수 있음
- AWS의 관리형 클러스터에서 Apache Flink 애플리케이션을 실행할 수 있음
- Apaches Flink는 표준 SQL보다 훨씬 강력하기 때문에 고급 쿼리 능력이나 필요하거나 Kinesis Data Streams나 AWS의 관리형 Kafka인 Amazon MSK 같은 서비스로부터 스트리 밍 데이터를 읽는 능력이 필요할 때 Apache Flink용 Kinesis Data Analytics를 사용
- 이 서비스를 사용하면 컴퓨팅 리소스를 자동 프로비저닝할 수 있고 병렬 연산과 오토 스케일링을 할 수 있음
	- 체크포인트와 스냅샷으로 구현되는 애플리케이션 백업이 가능하고 Apache Flink 프로그래밍 기능을 사용할 수도 있음
- 참고로 Apache Flink는 Kinesis Data Streams나 Amazon MSK의 데이터는 읽지만 Kinesis Data Firehose의 데이터는 읽지 못함
	- Kinesis Data Firehose에서 데이터를 읽고 실시간 분석하려면 SQL 애플리케이션용 Kinesis Data Analytics를 사용해야 함

