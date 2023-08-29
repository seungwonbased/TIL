# Amazon DynamoDB

- Amazon DynamoDB는 완전 관리형 데이터베이스로 데이터가 다중 AZ 간에 복제되므로 가용성이 높음
- DynamoDB는 클라우드 네이티브이며 AWS의 독점 서비스
- NoSQL 데이터베이스
	- RDS나 Aurora 같은 관계형 데이터베이스는 아니지만 트랜잭션 지원 기능이 있음
- DynamoDB를 이용하면 방대한 워크로드로 확장이 가능
	- 데이터베이스가 내부에서 분산되기 때문
- 초당 수백만 개의 요청을 처리하고 수조 개의 행, 수백 TB의 스토리지를 갖게됨
	- 이 점은 기억해야 함
- 성능은 한 자릿수 밀리초를 자랑하고 일관성 또한 높음
- 보안과 관련된 모든 기능은 IAM과 통합되어 있음
	- 보안, 권한 부여, 관리 기능이 포함
- 비용이 적게 들고 오토 스케일링 기능이 탑재돼 있음
- 유지 관리나 패치 없이도 항상 사용할 수 있음
- 데이터베이스를 프로비저닝할 필요가 없음
	- 항상 사용할 수 있으므로 테이블을 생성해 해당 테이블의 용량만 설정하면 됨
- 테이블 클래스 두 종류
	- Standard: 액세스가 빈번한 데이터
	- IA Table: 액세스가 빈번하지 않는 데이터

## DynamoDB - Basics

- DynamoDB는 테이블로 구성되며 데이터베이스를 생성할 필요가 없음
	- Aurora나 RDS와 달리 DynamoDB는 데이터베이스가 이미 존재하는 서비스
- DynamoDB에 테이블을 생성하면 각 테이블에 기본 키가 부여되는데 기본 키는 생성 시 결정됨
	- 각 테이블에 데이터를 추가
- 항목, 즉 행을 무한히 추가할 수 있음
	- 각 항목은 속성을 가지며 속성은 열에 표시됨
- 속성은 나중에 추가할 수도 있고 null이 될 수도 있음
	- 상당한 장점, RDS나 Aurora 데이터베이스에서는 열을 나중에 추가할 수 있으나 과정이 복잡하고 스키마 전개가 어려울 수 있지만 DynamoDB에서는 사전 요구 사항 없이 나중에 쉽게 속성을 추가할 수 있음
- DynamoDB 항목의 최대 크기는 400KB이므로 큰 객체를 저장할 때는 적합하지 않음
- DynamoDB는 다양한 데이터 유형을 지원
	- 문자열, 숫자, 바이너리, 불리언 null과 같은 스칼라 유형
	- 목록, 지도와 같은 문서 유형과 세트 유형을 지원
- 시험에서 스키마를 빠르게 전개해야 할 때 DynamoDB를 선택하면 됨
	- 데이터의 유형과 구성 면에서 스키마를 빠르게 전개해야 할 때 Aurora나 RDS보다는 DynamoDB가 더 나은 선택

## DynamoDB - Table Example

![dnd](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/dnd1.png)

- DynamoDB 테이블의 예시
- 기본 키는 파티션 키와 선택 사항인 정렬 키로 구성됨
- 속성 테이블이 있음
	- 데이터베이스 형태
- 속성은 null로 설정하거나 나중에 추가할 수 있음
	- DynamoDB의 장점

## DynamoDB - Read/Write Capacity Modes

- DynamoDB를 사용하려면 읽기/쓰기 용량 모드도 설정해야 함
- 테이블 용량 관리 방식을 제어하는 데는 두 가지 모드가 있음
	- 프로비저닝된 모드 (기본 설정)
		- 미리 용량을 프로비저닝
		- 초당 읽기/쓰기 요청 수를 예측해서 미리 지정하면 그것이 테이블의 용량이 됨
		- 미리 용량을 계획하고 프로비저닝된 RCU와 WCU만큼의 비용을 지불하는 방식
			- RCU는 읽기 용량 단위 WCU는 쓰기 용량 단위를 뜻함
		- 미리 용량을 계획한 경우에도 오토 스케일링 기능이 있으므로 테이블의 로드에 따라 자동으로 RCU와 WCU를 늘리거나 줄일 수 있음
		- 프로비저닝된 모드는 로드를 예측할 수 있고 서서히 전개되며 비용 절감을 원할 때 적합
	- 온디맨드 모드
		- 온디맨드 모드에서는 읽기/쓰기 용량이 워크로드에 따라 자동으로 확장됨
		- 미리 용량 계획을 하지 않으므로 RCU나 WCU 개념 자체가 없음
		- 온디맨드 모드에서는 정확히 사용한 만큼의 비용을 지불
		- 모든 읽기와 쓰기에 비용을 지불
		- 비싼 솔루션으로 볼 수도 있지만 워크로드를 예측할 수 없거나 급격히 증가하는 경우에 유용
			- 시험에 나올 수 있음
			- 수천 개의 트랜잭션을 수백만 개의 트랜잭션으로 1분 내로 확장해야 하는 애플리케이션에서는 빠르게 확장되지 않는 프로비저닝된 모드는 적합하지 않음
				- 온디맨드 모드를 선택해야함
			- 트랜잭션이 없거나 하루에 많아야 4~5회밖에 되지 않는 워크로드도 트랜잭션 횟수만큼만 비용을 지불하는 온디맨드 모드가 적합

## DynamoDB Accelerator (DAX)

![dnd](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/dnd2.png)

- DynamoDB Accelerator 즉 DAX는 DynamoDB를 위한 고가용성의 완전 관리형 무결절 인메모리 캐시
- DynamoDB 테이블에 읽기 작업이 많을 때 DAX 클러스터를 생성하고 데이터를 캐싱하여 읽기 혼잡을 해결
- DAX는 캐시 데이터에 마이크로초 수준의 지연 시간을 제공
	- 시험에 나올 만한 키워드
- DAX 클러스터는 기존 DynamoDB API와 호환되므로 애플리케이션 로직을 변경할 필요가 없음
- DynamoDB 테이블과 애플리케이션이 있을 때 몇몇 캐시 노드가 연결된 DAX 클러스터를 생성하면 백그라운드에서 DAX 클러스터가 Amazon DynamoDB 테이블에 연결됨
- 캐시의 기본 TTL은 5분으로 설정되어 있으나 변경할 수 있음

## DynamoDB Accelerator (DAX) vs ElastiCache

![dnd](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/dnd3.png)

- ElastiCache가 아니라 DAX를 사용하는 이유
	- DAX는 DynamoDB 앞에 있고 개별 객체 캐시와 쿼리와 스캔 캐시를 처리하는 데 유용
	- 예를 들어 집계 결과를 저장할 때는 Amazon ElastiCache가 좋고 Amazon DynamoDB는 대용량의 연산을 저장할 때 좋음
		- 두 서비스는 상호 보완적인 성격
- Amazon DynamoDB에 캐싱 솔루션을 추가할 때는 보통 DynamoDB Accelerator 즉 DAX를 사용

## DynamoDB - Stream Processing

- DynamoDB에서는 스트림 처리도 가능
	- 테이블의 모든 수정 사항, 즉 생성, 업데이트, 삭제를 포함한 스트림을 생성할 수 있음
	- 사용자 테이블에 새로운 사용자가 등록됐을 때 환영 이메일을 보내는 등 DynamoDB 테이블의 변경 사항에 실시간으로 반응하는 데 활용할 수 있음
	- 실시간으로 사용 분석을 하거나 파생 테이블을 삽입할 수도 있음
- 리전 간 복제를 실행하거나 DynamoDB 테이블 변경 사항에 대해 Lambda 함수를 실행할 수도 있음
- DynamoDB의 두 가지 스트림 처리
	- DynamoDB 스트림
		- 보존 기간이 24시간이고 소비자 수가 제한됨
		- Lambda 트리거와 함께 사용하면 좋음
		- 자체적으로 읽기를 실행하려면 DynamoDB Stream Kinesis 어댑터를 사용
	- Kinesis Data Streams
		- Kinesis Data Streams에 변경 사항을 바로 보내는 방법
		- 이 스트림은 보존 기간이 1년
		- 더 많은 수의 소비자 수를 가짐
		- 데이터를 처리하는 방법이 훨씬 많음
		- AWS Lambda 함수, Kinesis Data Analytics, Kinesis Data Firehose, Glue 스트리밍 ETL 등

### DynamoDB Streams

![dnd](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/dnd4.png)

- 애플리케이션이 DynamoDB 테이블에서 작업을 생성, 업데이트, 삭제하면 이는 DynamoDB 스트림이나 Kinesis Data Streams로 전송됨
- Kinesis Data Streams을 선택하면 Kinesis Data Firehose를 사용할 수 음
	- 그런 다음 분석 목적으로 데이터를 Amazon Redshift로 전송
		- 데이터를 아카이빙하려면 Amazon S3로 전송
		- Amazon OpenSearch로 보내면 인덱싱이나 검색을 할 수 있음
- DynamoDB 스트림을 사용하면 처리 계층을 둘 수 있음
	- EC2 인스턴스에서 애플리케이션을 실행하려면 KCL Adapter나 Lambda 함수를 사용
	- 처리 계층에서 SNS로 알림을 보내거나 DynamoDB 테이블을 필터링하거나 변환할 수 있음
	- Amazon OpenSearch로 처리 계층의 데이터를 전송할 수도 있음
- 아키텍처의 모든 옵션을 살펴본 건 아니고, 여러 변형이 가능

## DynamoDB Global Tables

- DynamoDB의 글로벌 테이블은 여러 리전 간에 복제가 가능한 테이블
	- 예를 들면 테이블을 US-EAST-1과 AP-SOUTHEAST-2에 둘 수 있음
- 두 테이블 간에는 양방향 복제가 가능
	- 예를 들면 US-EAST-1이나 AP-SOUTHEAST-2 테이블 둘 중 하나에 쓰기를 하면 된다는 뜻
- DynamoDB 글로벌 테이블은 복수의 리전에서 짧은 지연 시간으로 액세스할 수 있게 해 줌
- 다중 활성 복제가 가능하므로 애플리케이션이 모든 리전에서 테이블을 읽고 쓸 수 있다는 뜻
- 글로벌 테이블을 활성화하려면 DynamoDB 스트림을 활성화해야 리전 간 테이블을 복제할 수 있는 기본 인프라가 구축됨

## Dynamo DB - TTL

![dnd](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/dnd5.png)

- DynamoDB의 기능 중에 타임 투 리브(TTL)라는 기능은 만료 타임스탬프가 지나면 자동으로 항목을 삭제하는 기능
- SessionData라는 테이블에서 ExpTime(TTL)이라는 만료 기간 속성이 있음
	- 이 안에 타임스탬프가 들어감
- TTL을 정의한 다음 에포크 타임스탬프에서의 현재 시간이 ExpTime 열을 넘어설 경우 해당 항목을 만료 처리하고 삭제 처리를 진행하는 개념
	- 데이터 테이블의 항목이 일정 시간 후에 삭제되도록 하는 것
- 사용 사례
	- 최근 항목만 저장하도록 하거나 2년 후 데이터를 삭제해야 한다는 규정을 따라야 할 때 사용
	- 시험에서 자주 등장하는 사용 사례는 웹 세션 핸들링
		- 사용자가 웹사이트에 로그인했을 때 해당 세션을 중앙 저장소인 DynamoDB에 두 시간 동안 저장하는 것
		- 여기에 세션 데이터를 저장하면 모든 애플리케이션이 액세스할 수 있고 두 시간 후에 세션이 갱신되지 않으면 만료되어 해당 테이블에서 삭제됨

## DynamoDB - Backups for Disaster Recovery

- 재해 복구에도 DynamoDB를 활용
- 지정 시간 복구(PITR)
	- 지속적인 백업을 할 수 있음
	- 활성화를 선택할 수 있고 35일 동안 지속됨
	- 활성화하면 백업 기간 내에는 언제든 지정 시간 복구를 실행할 수 있음
	- 복구를 진행할 경우 새로운 테이블을 생성
- 온디맨드 백업
	- 이 백업은 직접 삭제할 때까지 보존됨
	- 온디맨드 백업은 DynamoDB의 성능이나 지연 시간에 영향을 주지 않음
- 백업을 좀 더 제대로 관리할 수 있는 방법 중 하나로 AWS Backup 서비스가 있음
	- 백업에 수명 주기 정책을 활성화할 수 있고 재해 복구 목적으로 리전 간 백업을 복사할 수 있음
	- 이 옵션 또한 백업으로 복구를 진행하면 새로운 테이블이 생성됨

## DynamoDB - Integration with Amazon S3

![dnd](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/dnd6.png)

- DynamoDB와 Amazon S3 간 통합
- S3에 테이블을 내보낼 수 있는데, 이를 위해서는 지정 시간 복구 기능을 활성화해야 함
- DynamoDB 테이블을 S3로 내보내고 쿼리를 수행하려면 Amazon Athena 엔진을 사용
- 지속적인 백업을 활성화한 상태이므로 최근 35일 이내 어떤 시점으로든 테이블을 내보낼 수 있음
	- 테이블을 내보내도 테이블의 읽기 용량이나 성능에 영향을 주지 않음
- 따라서 DynamoDB를 Amazon S3로 먼저 내보내기 하여 데이터 분석을 수행할 수 있음
	- 감사 목적으로 스냅샷을 확보할 수도 있고, 데이터를 DynamoDB로 다시 가져오기 전에 데이터 ETL 등 대규모 변경을 실행할 수도 있음
	- 내보낼 때는 DynamoDB JSON이나 ION 형식을 이용
- Amazon S3에서 테이블을 가져올 수도 있음
	- S3에서 CSV, JSON 그리고 ION 형식으로 내보낸 다음 새로운 DynamoDB 테이블을 생성하는 식
	- 쓰기 용량을 소비하지 않고 새로운 테이블이 생성됨
	- 가져올 때 발생한 오류는 모두 CloudWatch Logs에 기록됨
