# Amazon QuickSight

![qus](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/qus1.png)

- Amazon QuickSight는 서버리스 머신 러닝 기반 비즈니스 인텔리전스 서비스
- 비즈니스 인텔리전스니까 대화형 대시보드를 생성함
- QuickSight는 대시보드를 생성하고 소유한 데이터 소스와 연결할 수 있음
	- 시각화할 수 있고 빠르며 오토 스케일링이 가능
- 웹사이트에 임베드할 수 있으며 세션당 비용을 지불
- 사용 사례
	- 비즈니스 분석
	- 시각화 구현
	- 시각화된 정보를 통한 임시 분석 수행
	- 데이터를 활용한 비즈니스 인사이트 획득
- RDS, Aurora, Athena, Redshift, S3 등 다양한 데이터 소스에 연결할 수 있음
- SPICE 엔진이라는 인 메모리 연산 엔진은 Amazon QuickSight로 데이터를 직접 가져올 때 사용되며 Amazon QuickSight가 다른 DB에 연결돼 있을 때는 작동하지 않음
- QuickSight는 훌륭한 사용자 수준의 기능을 제공
- Amazon QuickSight의 엔터프라이즈 에디션에서는 액세스 권한이 없는 사용자에게 일부 열이 표시되지 않도록 열 수준 보안(CLS)을 설정할 수 있음

## QuickSight Integrations

![qus](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/qus2.png)

- AWS의 다양한 데이터 소스와 통합할 수 있음
	- RDS 데이터베이스
	- Aurora
	- 데이터 웨어하우징 서비스인 Redshift
	- S3에서 임시 쿼리를 수행하는 Athena
	- 데이터를 가져오기 위한 Amazon S3
	- OpenSearch
	- 시계열 데이터를 최적할 수 있는 Timestream
- 타사 데이터 소스와 통합할 수도 있음
	- QuickSight가 지원하는 서비스형 소프트웨어여야 함
	- 전체 목록은 QuickSight 웹사이트에 있는데, Salesforce와 Jira가 대표적
	- Teradata 같은 타사 데이터베이스와 통합할 수도 있음
- 내부적으로 JDBC 프로토콜을 사용하는 온프레미스 데이터베이스와 통합할 수도 있음
	- QuickSight로 직접 Excel 파일, CSV 파일 JSON 문서, TSV 파일, 로그 형식의 ELF 및 CLF 등의 데이터 소스를 가져올 수 있음
	- Amazon QuickSight로 데이터 소스를 가져온 다음 SPICE 엔진을 활용해 매우 빠르게 인 메모리 연산을 수행할 수 있음
- 시험에는 QuickSight를 Athena나 Redshift와 함께 사용하는 문제가 자주 나오지만 다른 통합도 출제될 수 있음

## QuickSight - Dashboard & Analysis

- QuickSight에는 세 가지 개념이 있음
	- 대시보드 및 분석 그리고 사용자
	- 스탠다드 버전에서는 사용자를 정의할 수 있고, 그룹은 엔터프라이즈 버전에서만 사용할 수 있음
- QuickSight의 사용자와 그룹은 QuickSight 서비스 전용
	- IAM 사용자와는 다름
		- IAM 사용자는 관리용으로만 사용됨
- QuickSight 내에서 사용자와 그룹을 정의하고 대시보드를 생성하면 됨
- 대시보드는 읽기 전용 스냅샷이며 분석 결과를 공유할 수 있음
	- 또한 분석의 구성을 저장
	- 분석을 위해 설정한 필터 또는 매개변수 제어, 정렬 옵션 등이 저장되어 대시보드에 표시됨
	- 따라서 분석이 좀 더 충실하며 특정 사용자 또는 그룹과 분석 결과나 대시보드를 공유할 수 있음
	- 일단 대시보드부터 게시해야 하고, 액세스 권한이 있는 사용자는 기본 데이터를 볼 수도 있음
- QuickSight에서는 분석 및 대시보드를 생성해야 하고, 특정 사용자나 그룹과 공유할 수 있음
