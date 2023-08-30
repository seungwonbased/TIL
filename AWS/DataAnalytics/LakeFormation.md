# AWS Lake Formation

- AWS Lake Formation은 데이터 레이크 생성을 도움
- 데이터 레이크: 데이터 분석을 위해 모든 데이터를 한곳으로 모아 주는 중앙 집중식 저장소
- Lake Formation은 데이터 레이크 생성을 수월하게 해 주는 완전 관리형 서비스
- Lake Formation을 사용하면 보통 수개월씩 걸리는 작업을 며칠 만에 완료할 수 있음
- Lake Formation은 데이터 레이크에서의 데이터 검색, 정제, 변환 주입을 도움
	- 데이터 수집, 정제나 카탈로깅, 복제 같은 복잡한 수작업을 자동화하고 기계 학습(ML) 변환 기능으로 중복제거를 수행
- 데이터 레이크에서는 정형 데이터와 비정형 데이터 소스를 결합할 수 있으며 블루프린트를 제공
	- 내장된 블루프린트는 데이터를 데이터 레이크로 이전(Migrate)하는 것을 도와주며 Amazon S3, Amason RDS, 온프레미스에서 실행되는 관계형 데이터베이스, NoSQL 데이터베이스 등에서 지원됨
- Lake Formation을 설정하는 이유는 모든 데이터를 한곳에서 처리하는 것 외에도 애플리케이션에서 행, 열 수준의 세분화된 액세스 제어를 할 수 있기 때문
- AWS Lake Formation에 연결된 애플리케이션에서는 세분화된 액세스 제어가 가능
- AWS Glue 위에 빌드되는 계층이긴 하지만 Glue와 직접 상호 작용하지 않음

![lake](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/lake1.png)

- Amazon S3에 저장되는 데이터 레이크의 생성을 도움
- 데이터 소스로는 Amazon S3 RDS, Aurora, SQL, NoSQL 같은 온프레미스 데이터베이스가 있음
- Lake Formation의 블루프린트를 통해 데이터를 주입
- Lake Formation에는 소스 크롤러와 ETL 및 데이터 준비 도구 데이터 카탈로깅 도구가 포함됨
	- Glue의 기본 서비스에 해당
- 데이터 레이크의 데이터를 보호하는 보안 설정과 액세스 제어도 포함됨
- Lake Formation을 활용하는 서비스로는 Athena, Redshift, EMR, Apache Spark 프레임워크 같은 분석 도구가 있음
	- 사용자는 이와 같은 서비스를 통해 Lake Formation 및 데이터 레이크에 연결

## AWS Lake Formation Centralized Permissions Example

![lake](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/lake2.png)

- Lake Formation을 사용하는 이유
	- 시험에서 반드시 언급되는 가장 핵심적인 이유는 바로 중앙화된 권한
- 회사가 데이터 분석에 Athena와 QuickSight를 사용할 때, 사용자는 허용된 데이터만 볼 수 있어야 하고 읽기 권한이 있어야 함
- 데이터 소스는 Amazon S3 RDS, Aurora 등
- Athena에 보안을 설정하거나 QuickSight에서 사용자 수준의 보안을 설정할 수 있음
	- S3 버킷 정책이나 사용자 정책에 보안 설정을 할 수도 있음
	- RDS, Aurora도 마찬가지
	- 이렇게 보안을 관리할 곳이 많아지면 엉망이 될 것임
- Lake Formation이 해결 방법임
	- 액세스 제어 기능과 열 및 행 수준 보안이 있기 때문
	- Lake Formation에 주입된 모든 데이터는 중앙 S3 버킷에 저장되지만 모든 액세스 제어와 행, 열 수준 보안은 Lake Formation 내에서 관리됨
	- 따라서 Lake Formation에 연결하는 서비스는 읽기 권한이 있는 데이터만 볼 수 있게 됨
- Athena, QuickSight 등 어떤 도구를 사용하든 Lake Formation에 연결하면 한곳에서 보안을 관리할 수 있음
	- Lake Formation의 큰 장점으로 시험에 나올 수 있음