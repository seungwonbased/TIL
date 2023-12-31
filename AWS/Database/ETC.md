# DocumentDB

- DocumentDB는 Aurora로 AWS에서 PostgreSQL과 MySQL의 대규모 클라우드 네이티브 버전을 구현하는 것과 비슷
	- DocumentDB는 MongoDB 용 Aurora
	- MongoDB는 NoSQL 데이터베이스
		- 시험을 위해 기억해야 함
- 즉, DocumentDB는 NoSQL 데이터베이스
	- MongoDB 기술 기반
	- 따라서 몽고DB와 호환
- 몽고DB는 JSON 데이터를 저장, 쿼리, 인덱스하는 데 사용됨
- DocumentDB에도 Aurora와 같은 배포 개념이 있음
	- 즉, 완전 관리형 데이터베이스, 가용성이 높음
	- 데이터는 3개의 가용 영역에 복제됨
	- 스토리지는 자동으로 10GB 단위로 최대 64TB까지 증가
- DocumentDB는 초당 수백만 건의 요청이 있는 워크로드로 확장될 수 있도록 설계됨
- 시험 볼 때 몽고DB 관련 내용이 보이면 DocumentDB를 기억
	- 만약 NoSQL 데이터베이스와 관련된 내용이 나오면 DocumentDB와 DynamoDB를 고려

# Neptune

- Neptune은 완전 관리형 그래프 데이터베이스
	- 그래프 데이터 셋의 예는 소셜 네트워크
		- 소셜 네트워크를 보면 사람들은 친구를 맺고, 좋아요를 누르고, 연결되고, 댓글을 읽는 등의 활동
		- 사용자에는 친구가 있고, 게시물에는 댓글이 있고, 댓글에는 좋아요가 있죠, 사용자는 게시물을 공유하고 좋아요를 누름
		- 이 모든 것들이 서로 연결되고 그래프를 만듬
- 따라서 그래프 데이터 셋에서는 Neptune을 데이터베이스로 선택하는 게 좋음
- Neptune은 3 AZ에 걸쳐 최대 15개의 읽기 전용 복제본으로 복제
- Neptune은 소셜 네트워크처럼 고도로 연결된 데이터 셋을 사용하는 애플리케이션을 구축하고 실행하는 데 사용됨
- Neptune은 그래프 데이터 셋에서 복잡하고 어려운 쿼리를 실행하기에 최적화됨
	- 데이터베이스에 최대 수십억 개의 관계를 저장할 수 있고, 그래프를 쿼리할 때 지연시간은 밀리초
- 여러 가용 영역에 걸친 애플리케이션에서도 매우 가용성이 높으며 지식 그래프를 저장하는 데도 뛰어남
	- 예를 들어 모든 위키피디아 기사들은 서로 연결되어 있으니까, 위키피디아 데이터베이스는 지식 그래프죠, 사기 탐지, 추천 엔진, 소셜 네트워킹도 그러함
- 따라서 시험에서 그래프 데이터베이스와 관련된 게 나오면 Neptune만 생각

# Keyspaces

- Keyspaces는 AWS의 관리형 Apache Cassandra를 보조
	- Cassandra는 오픈 소스 NoSQL 분산 데이터베이스이고 Keyspaces를 사용하면 클라우드에서 AWS가 Cassandra를 직접 관리
- 서버리스 서비스이며 확장성과 가용성이 높으며 AWS 완전 관리형
	- 애플리케이션 트래픽에 따라 테이블을 자동으로 확장/축소
- 테이블 데이터는 여러 AZ에 걸쳐 세 번 복제됨
- Keyspaces에서 쿼리를 수행하려면 Cassandra 쿼리 언어(CQL)을 사용하면 됨
- 어떤 규모에서도 지연 시간이 10밀리초 미만으로 짧고 초당 수천 건의 요청을 처리
- 두 가지 용량 모드
	- 온디맨드 모드
	- 프로비저닝 모드:  크기가 자동 조정됨
- 암호화와 백업 기능을 제공하고 최대 35일까지 지정 시간 복구가 가능
- 사용 사례로는 IoT 장치 정보와 시계열 데이터 저장 등이 있음
- Apache Cassandra가 시험에 나오면 Amazon Keyspaces만 떠올리면 됨

# QLDB

- QLDB는 퀀텀 레저 데이터베이스의 약자
	- 원장은 금융 트랜잭션을 기록하는 장부
	- 따라서 QLDB는 금융 트랜잭션 원장을 갖게 됨
- 완전 관리형 데이터베이스이며, 서버리스고, 가용성이 높고, 3개의 가용 영역에 걸쳐 데이터를 복제할 수 있음
- 애플리케이션 데이터의 시간에 따른 모든 변경 내역을 검토하는 데 사용됨
	- 따라서 장부라고 함
- 불변 시스템임
	- 즉, 데이터베이스에 무언가를 쓰면, 삭제하거나 수정할 수 없음
	- 정말로 아무 것도 삭제되지 않았는지 확인하기 위해, 암호화 서명을 하기도 함
	- 내부적으로 저널이 있고, 저널에는 수정 시퀀스가 있음
		- 따라서 수정이 일어날 때마다, 암호화 해시가 계산됨
			- 아무 것도 삭제되거나 수정되지 않도록 보장
		- 데이터베이스를 사용하는 모든 사람이 확인할 수 있음
	- 금융 트랜잭션에 매우 유용
		- 왜냐하면 어떤 금융 트랜잭션도 데이터베이스에서 사라지게 하고 싶지 않기 때문
- 일반 원장 블록체인 프레임워크보다 2-3배 더 나은 성능을 얻을 수 있음
- SQL을 사용하여 데이터를 관리할 수도 있음
- QLDB와 관리형 블록체인의 차이점은, QLDB에는 탈중앙화 개념이 없다는 것
	- 즉, Amazon 소유의 중앙 데이터베이스에서만 저널을 작성할 수 있음
	- 많은 금융 규제 규칙들을 따름
	- 즉, QLDB와 관리형 블록체인의 차이점은, QLDB에는 중앙 권한 구성 요소가 있다는 것
- 금융 트랜잭션이나 원장이 나오면 QLDB를 생각

# Timestream

- Amazon Timestream은 시계열 데이터베이스
- 완전 관리형이며 빠르고 확장성 있는 서버리스 서비스
- 시계열이란 시계열은 시간 정보를 포함하는 포인트의 모음
	- 연도별 그래프가 시계열의 예시
- Timestream를 사용하면 데이터베이스의 용량을 자동으로 확장, 축소할 수 있고 매일 수조 건의 이벤트를 저장 및 분석할 수 있음
- 시계열 데이터가 있을 때는 관계형 데이터베이스보다 시계열 데이터베이스를 사용하는 것이 훨씬 빠르고 저렴
	- 시계열 데이터베이스의 특성이자 장점
- 쿼리를 예약하고 다중 척도 레코드도 얻을 수 있음
- SQL과도 완벽히 호환됨
- 최신 데이터는 메모리에 저장되고 과거 데이터는 비용 효율적인 스토리지 계층에 저장됨
- 시계열 분석 기능이 있어 거의 실시간으로 데이터를 분석하고 패턴을 찾을 수 있음
- AWS의 다른 데이터베이스들처럼 전송 중 데이터와 저장 데이터의 암호화를 지원
- Timestream의 사용 사례로는 IoT 애플리케이션이 있음
	- 운영 애플리케이션, 실시간 분석 등 시계열 데이터베이스와 관련된 모든 곳에서 사용할 수 있음

![dbetc](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/dbetc.png)

- Timestream은 AWS IoT 즉, 사물 인터넷에서 데이터를 받을 수 있고, Kinesis Data Streams의 데이터도 Lambda를 통해 받을 수 있음
- Prometheus, telegraf와 통합할 수 있음
- Apache Flink용 Kinesis Data Analytics는 Kineis Data Stream과 Amazon MSK의 데이터를 Amazon Timestream에 전달
- Timestream과 연결 가능한 것으로는 대시보드를 빌드할 수 있는 Amazon Quicksight, 기계 학습을 할 수 있는 Amazon SageMaker, Grafana가 있음
- 데이터베이스에 표준 JDBC가 연결돼 있으므로 JDBC, SQL과 호환 가능한 애플리케이션은 Amazon Timestream을 활용할 수 있음
- 시험에 대비해서는 Timestream을 전반적으로만 알고 있으면 됨
