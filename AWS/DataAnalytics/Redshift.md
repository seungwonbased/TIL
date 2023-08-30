# Redshift

- 데이터베이스이자 분석 엔진인 Amazon Redshift
- Redshift는 PostgreSQL 기술 기반이지만 온라인 트랜잭션 처리(OLTP)에 사용되지는 않음
- 온라인 분석 처리를 의미하는 OLAP 유형의 데이터베이스이며 분석과 데이터 웨어하우징에 사용
- 다른 어떤 데이터 웨어하우징보다 성능이 10배 이상 좋고 데이터가 PB 규모로 확장되므로 모든 데이터를 Redshift에 로드하면 Redshift에서 빠르게 분석할 수 있음
- Redshift는 성능 향상이 가능한데 이는 열 기반 데이터 스토리지를 사용하기 때문
	- 행 기반이 아니라 병렬 쿼리 엔진이 있는 것
- Redshift 클러스터에서 프로비저닝한 인스턴스에 대한 비용만 지불
- 쿼리를 수행할 때 SQL 문을 사용할 수 있음
- Amazon Quicksight 같은 BI 도구나 Tableau 같은 도구도 Redshift와 통합할 수 있음
- vs Athena
	- Redshift는 예를 들면 먼저 Amazon S3에서 모든 데이터를 Redshift에 로드해야 함
	- Redshift에 데이터를 로드하고 나면 Redshift의 쿼리가 더 빠르고 조인과 통합을 훨씬 더 빠르게 할 수 있는데, Athena에는 없는 인덱스가 있기 때문
	- Redshift는 데이터 웨어하우스가 높은 성능을 발휘하도록 인덱스를 빌드
	- Amazon S3의 임시 쿼리라면 Athena가 좋은 사용 사례가 되지만 쿼리가 많고 복잡하며 조인하거나 집계하는 등 집중적인 데이터 웨어하우스라면 Redshift가 더 좋음

## Redshift Cluster

![reds](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/reds1.png)

- Redshift 클러스터에는 두 노드가 있음
	- 리더 노드는 쿼리를 계획하고 결과를 집계
	- 컴퓨팅 노드는 실제로 쿼리를 실행하고 결과를 리더 노드에 보냄
- Redshift 클러스터는 노드 크기를 미리 프로비저닝해야 하고 비용을 절감하려면 예약 인스턴스를 사용하면 됨
- 리더 노드에 쿼리를 SQL 형식으로 제출하면 백엔드에서 쿼리가 발생

## Redshift - Snapshots & DR

![reds](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/reds2.png)

- Redshift의 스냅샷과 재해 복구
- Redshift는 대부분의 클러스터는 단일 AZ이나, 이제 일부 클러스터 타입에 대해서는 다중 AZ 모드가 가능
- 다중 AZ 모드가 된다면 재난 복구 전략이 적용되나, 단일 AZ의 경우, Redshift에 재해 복구 전략을 적용하려면 스냅샷을 사용해야 함
- 스냅샷은 클러스터의 지정 시간 백업으로 Amazon S3 내부에 저장되며 증분함
	- 변경된 사항만 저장되므로 많은 공간을 절약할 수 있음
- 새로운 Redshift 클러스터에 스냅샷을 복원할 수 있으며 스냅샷에는 두 가지 모드가 있음
	- 수동으로 사용하거나 자동화
	- 스냅샷이 8시간마다 또는 5GB마다 자동으로 생성되도록 일정을 예약할 수 있고 자동화된 스냅샷의 보존 기간을 설정할 수 있음
	- 수동 스냅샷을 사용할 경우 직접 스냅샷을 삭제하기 전까지 스냅샷이 유지됨
- Redshift의 정말 훌륭한 기능은 자동이든 수동이든 클러스터의 스냅샷을 다른 AWS 리전에 자동으로 복사하도록 Redshift를 구성하여 재해 복구 전략을 적용할 수 있다는 점
- 원본 Redshift 클러스터와 다른 리전이 있을 때 스냅샷을 생성하면 새로운 리전에 자동으로 복사되고 복사된 스냅샷에서 새 Redshift 클러스터를 복원할 수 있음

## Loading Data into Redshift: Large inserts are much better

![reds](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/reds3.png)

- Redshift에 데이터를 주입하는 방법을 살펴보죠
- Amazon Kinesis Data Firehose
	- Firehose가 다양한 소스로부터 데이터를 받아서 Redshift에 보내는 것
	- 먼저 Amazon S3 버킷에 데이터를 써야함
	- 그러면 자동으로 Kinesis Data Firehose가 S3 복사 명령을 실행해 Redshift에 데이터가 로드됨
- 복사 명령
	- 수동으로도 가능함
	- S3에 데이터를 로드하고 Redshift에서 복사 명령을 실행하면 IAM 역할을 사용해 S3 버킷에서 Amazon Redshift 클러스터로 데이터를 복사
		- 아시다시피 두 가지 방법이 있음
			- 인터넷을 사용할 수 있음, S3 버킷은 퍼블릭이기 때문
				- 데이터가 인터넷을 통해 다시 Redshift 클러스터로 이동하게 되는데,  향상된 VPC 라우팅 없이도 가능
			- 모든 네트워크를 가상 프라이빗 클라우드에 비공개 상태로 유지하고 싶다면 모든 데이터가 VPC로 완전히 이동되도록 향상된 VPC 라우팅을 사용하면 됨
- EC2 인스턴스 JDBC 드라이버
	- JDBC 드라이버를 사용해 Redshift 클러스터에 데이터를 삽입하는 방법이 있음
	- 예를 들어 애플리케이션에 Redshift 클러스터에 써야 하는 EC2 인스턴스가 있을 때 이 방법을 사용하면 됨
- 이때 Amazon Redshift에 큰 배치로 데이터를 쓰는 것이 좋음
	- 이 유형의 데이터베이스에 한 번에 한 행씩 쓰는 건 비효율적

## Redshift Spectrum

![reds](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/reds4.png)

- Redshift Spectrum은 Amazon S3에 있는 데이터를 Redshift를 사용해 분석은 하지만 Redshift에 로드는 하지 않음
	- 더 많은 처리 능력을 사용하려는 것
- Redshift Spectrum을 사용하려면 쿼리를 시작할 수 있는 Redshift 클러스터가 있어야 함
- 일단 쿼리를 시작하면 S3에 있는 데이터에 쿼리를 실행할 수천 개의 Redshift Spectrum 노드에 쿼리가 제출됨
- 예시
	- Redshift 클러스터에 리더 노드와 여러 개의 컴퓨팅 노드가 있음
	- 분석할 데이터는 Amazon S3에 있으며 Redshift 클러스터에서 쿼리를 실행할 것임
	- 보다시피 쿼리하려는 테이블이 S3에 있으므로 쿼리는 From S3.으로 시작해야 함
	- 그러면 Spectrum이 자동으로 시작되고 쿼리는 Amazon S3에서 데이터를 읽고 집계하는 수천 개의 Redshift Spectrum 노드에 제출됨
	- 제출이 완료되면 결과가 Amazon Redshift 클러스터를 통해 쿼리를 시작한 곳으로 전송됨
- 이 기능을 사용하면 Amazon S3에서 Redshift로 데이터를 로드하지 않아도 되므로 클러스터에서 프로비저닝한 것보다 더 많은 처리 능력을 활용할 수 있음
