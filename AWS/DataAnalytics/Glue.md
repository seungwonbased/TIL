# AWS Glue

![glue](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/glue1.png)

- Glue는 추출과 변환 로드 서비스를 관리하며 ETL 서비스라고도 함
- 분석을 위해 데이터를 준비하고 변환하는 데 매우 유용
- Glue는 완전 서버리스 서비스이며 원하는 모든 작업을 수행할 것
- 예시
	- S3 버킷이나 Amazon RDS 데이터베이스에 있는 데이터를 데이터 웨어하우스인 Redshift에 로드한다고 가정
	- Glue를 사용해 추출한 다음 일부 데이터를 필터링하거나 열을 추가하는 등 원하는 대로 데이터를 변형할 수 있음
	- 그런 다음 최종 출력 데이터를 Redshift 데이터 웨어하우스에 로드
- Glue ETL 서비스 내에서 모든 작업이 이뤄짐
	- 간단한 코드를 작성하고 ETL 작업을 시작하기만 하면 됨

## Glue - Convert Data into Parquet Format

![glue](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/glue2.png)

- 데이터를 Parquet 형식으로 변환하는 것
- 이런 작업을 하는 이유는 Parquet은 열 기반의 데이터 형식이므로 Athena 같은 서비스와 함께 사용하면 효과적이기 때문
- 예시
	- S3 버킷에 CSV 형식으로 된 데이터를 삽입한다고 가정
	- Glue ETL 서비스를 사용해 CSV 파일을 가져와 Glue 서비스 내에서 Parquet 형식으로 변환한 다음 출력 S3 버킷으로 데이터를 보냄
	- Parquet 형식으로 변환하면 Amazon Athena가 파일을 훨씬 더 잘 분석
	- 전체 과정을 자동화할 수도 있는데, 파일이 S3 버킷에 삽입될 때마다 Lambda 함수로 이벤트 알림을 보내 Glue ETL 작업을 트리거하면 됨
		- Lambda 함수 대신 EventBridge를 사용해도 됨

## Glue Data Catalog: Catalog of Datasets

![glue](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/glue3.png)

- Glue Data Catalog는 데이터 세트의 카탈로그
- Glue Data Catalog는 Glue 데이터 크롤러를 실행해 Amazon S3, Amazon RDS, Amazon DynamoDB 또는 호환 가능한 온프레미스 JDBC 데이터베이스에 연결
- Glue 데이터 크롤러는 데이터베이스를 크롤링하고 데이터베이스의 테이블 열, 데이터 형식 등의 모든 메타 데이터를 Glue 데이터 카탈로그에 기록
	- 따라서 ETL을 수행하기 위한 Glue 작업에 활용될 모든 데이터베이스, 테이블 메타 데이터를 갖게 됨
- Amazon Athena는 데이터와 스키마를 검색할 때 백그라운드에서 AWS Glue Data Catalog를 활용
	- Amazon Redshift Spectrum과 Amazon EMR도 마찬가지
- Glue Data Catalog 서비스는 다른 여러 AWS 서비스의 중심

## Glue - Things to Know at a High-level

- Glue의 기능 중에 시험에 나올 수 있고 전반적으로 알아야 할 첫 번째 기능은 Glue 작업 북마크
	- 새 ETL 작업을 실행할 때 이전 데이터의 재처리를 방지
	- 매우 중요하고 시험에 나올 수 있음
- Glue Elastic Views는 SQL을 사용해 여러 데이터 스토어의 데이터를 결합하고 복제
	- 가령 RDS 데이터베이스와 Aurora 데이터베이스, Amazon S3에 걸친 뷰를 생성
	- 커스텀 코드를 지원하지 않으며 Glue가 원본 데이터의 변경 사항을 모니터링
	- 서버리스 서비스
	- 또한 여러 데이터 스토어에 분산된 구체화된 뷰인 가상 테이블을 생성할 수 있음
- Glue DataBrew는 사전 빌드된 변환을 사용해 데이터를 정리하고 정규화
- Glue Studio는 Glue에서 ETL 작업을 생성, 실행 및 모니터링하는 GUI
- Glue 스트리밍 ETL은 Apache Spark Structured Streaming 위에 빌드되며 ETL 작업을 배치 작업이 아니라 스트리밍 작업으로 실행할 수 있음
	- Kinesis Data Streaming Kafka 또는 AWS의 관리형 Kafka인 MSK에서 Glue 스트리밍 ETL을 사용해 데이터를 읽을 수 있음
