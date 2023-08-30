# Amazon Athena

- Athena는 Amazon S3 버킷에 저장된 데이터 분석에 사용하는 서버리스 쿼리 서비스로, 데이터를 분석하려면 표준 SQL 언어로 파일을 쿼리해야 함
- Athena는 SQL 언어를 사용하는 Presto 엔진에 빌드됨
- 사용자가 S3 버킷에 데이터를 로드하거나 여러분이 자신의 S3 버킷에 데이터를 로드하면 Athena 서비스를 사용해 이동하지 않고 Amazon S3에서 데이터를 쿼리하고 분석할 수 있음
	- 즉 Athena는 서버리스로 S3 버킷의 데이터를 바로 분석
- CSV, JSON, ORC, Avro Parquet 등 다양한 형식을 지원
- 가격 책정
	- 스캔된 데이터의 TB당 고정 가격을 지불
- 전체 서비스가 서버리스여서 데이터베이스를 프로비저닝 할 필요가 없음
- Athena는 Amazon QuickSight라는 도구와 함께 사용하는 일이 많음
	- 이를 통해 보고서와 대시보드를 생성
	- QuickSight는 S3 버킷에 연결된 Athena 다음에 배치됨
- 사용 사례
	- 임시 쿼리 수행
	- 비즈니스 인텔리전스 분석 및 보고
	- AWS 서비스에서 발생하는 모든 로그를 쿼리하고 분석할 수 있음
		- VPC 흐름 로그, 로드 밸런서 로그 CloudTrail 추적 등이 해당됨
- 시험에 관한 팁
	- 서버리스 SQL 엔진을 사용한 Amazon S3 데이터 분석이 나오면 Athena를 떠올리면 됨

## Amazon Athena - Performance Improvement

- Athena 성능도 향상이 가능하며 시험에 이 부분이 출제될 수 있음
- Use Columnar Data
	- 비용을 지불할 때 스캔된 데이터의 TB당 가격을 지불하므로 데이터를 적게 스캔할 유형의 데이터를 사용하는 것
	- 열 기반 데이터 유형을 사용하면 필요한 열만 스캔하므로 비용을 절감할 수 있음
	- 따라서 Amazon Athena에 권장하는 형식은 Apache Parquet과 ORC
	- 이 형식을 사용하면 성능이 크게 향상됨
	- 파일을 Apache Parquet나 ORC 형식으로 가져오려면 Glue 서비스를 사용해야 함
		- Glue는 적재(ETL) 작업으로 CSV와 Parquet 간의 데이터를 변환하는 데 매우 유용
- Compress Data
	- 더 적은 데이터를 스캔해야 하므로 데이터를 압축해 더 적게 검색해야 함
- Partion
	- 다음으로 특정 열을 항상 쿼리한다면 데이터 세트를 분할하면 됨
	- S3 버킷에 있는 전체 경로를 슬래시로 분할한다는 뜻
		- Amazon S3에 있는 데이터를 정리하고 분할하는 것
		- 각 슬래시에 다른 열 이름을 붙여 열별로 특정 값을 저장
	- 데이터를 쿼리할 때 Amazon S3의 어떤 폴더 즉, 어떤 경로로 데이터를 스캔할지 정확히 알 수 있음
	- 데이터 분할의 예
		- Parquet 형식의 전송 데이터에 /year=1991을 입력
			- 이렇게 연도별로 분할하면 연도별로 한 개의 폴더가 생성됨
		- 연도별 다음으로 월별 분할을 위해 /month=1을 입력했고, 일별 분할을 위해 /day=1을 입력
		- 이제 Athena에서 쿼리를 실행할 때 특정 연도, 월, 일로 필터링하면 Amazon S3의 정확히 어떤 폴더에서 데이터를 가져와야 하는지 알 수 있어 데이터의 서브셋만 복구하면 됨
- Use Larger Files
	- 큰 파일을 사용해서 오버헤드를 최소화하면 성능을 향상할 수 있음
	- Amazon S3에 작은 파일이 너무 많으면 Athena는 큰 파일이 있을 때보다 성능이 떨어짐
	- 파일이 클수록 스캔과 검색이 쉬우므로 128MB 이상의 파일을 사용해야 함

## Amazon Athena - Federated Query

![athe](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/athe2.png)

- Amazon Athena의 다른 기능으로 연합 쿼리가 있음
- Athena는 S3뿐만 아니라 어떤 곳의 데이터도 쿼리할 수 있음
	- 관계형 데이터베이스나 비관계형 데이터베이스 객체, 사용자 지정 데이터 원본을 쿼리할 수 있음
- AWS나 온프레미스에서 데이터 원본 커넥터를 사용하면 쿼리 가능
	- 데이터 원본 커넥터는 Lambda 함수로 다른 서비스에서 연합 쿼리를 실행
	- CloutWacth Logs, DyanamoDB RDS 등에서 실행하며 매우 강력
- 예시
	- Athena와 Lambda 함수가 있음
	- 데이터 원본 커넥터당 하나의 Lambda 함수를 가지며 Amazon Athena을 통해 ElastiCache, DocumentDB DynamoDB, Redshift와 Aurora, SQL 서버, MySQL EMR 서비스의 HBase에서 쿼리를 실행할 수 있고, Athena에서 바로 Amazon S3뿐만 아니라 모든 온프레미스 데이터베이스를 쿼리할 수 있으며 쿼리를 조인하거나 경쟁할 수도 있음
	- 그래서 연합 쿼리라고 부름
- 쿼리 결과는 사후 분석을 위해 Amazon S3 버킷에 저장할 수 있음
