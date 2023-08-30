# Big Data Ingestion Pipeline

- 빅 데이터 수집 파이프라인의 또 다른 아키텍처
- 애플리케이션 수집 파이프라인이 완전히 서버리스이면서 AWS가 100% 관리해 준다면 더할 나위 없이 좋을 것
- 요구 사항
	- 실시간으로 데이터를 수집하길 원함
	- 데이터를 변형하고 변형된 데이터를 SQL을 통해 요청
	- 이러한 쿼리를 사용해 생성한 보고서가 S3에 저장됨
	- 이후 데이터를 데이터 웨어하우스에 등재해 대시보드를 생성하고자 함

![bdpl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/bdpl1.png)

- 데이터 생산자가 IoT 장치라고 가정
- Amazon 클라우드 서비스 중 IoT Core는 매우 유용한 기능으로 IoT 장치 관리를 도움
	- 시험 볼 때 꼭 기억
- IoT Core는 장치에서 실시간으로 전송받은 데이터를 Kinesis Data Stream으로 직접 전달
- Kinesis의 데이터 스트림은 빅 데이터가 실시간으로 Kinesis 서비스에 전송되도록 허용
- Kinesis Data Firehose는 Kinesis와 통신해서 거의 1분마다 Amazon S3 버킷에 데이터를 입력하고 오프로드
	- 이를 수집 버킷이라고 함
- 이렇게 해서 여러 장치에서 많은 데이터를 실시간으로 얻을 수 있는 파이프라인이 마련됨
- S3 버킷에 데이터를 매분 입력해 줄 뿐만 아니라 람다 함수를 이용해 빠른 속도로 데이터를 정리하거나 변형하도록 도와주기도 하는데, 이때 람다 함수는 Kinesis Data Firehose와 직접 연결된 상태
- SQS 대기열을 작동할 수 있는데 이건 선택 사항임
	- SQS 대기열은 람다 함수를 실행
	- S3 버킷이 Lambda를 직접 작동할 수 있기 때문에 선택 사항이라고 한 건데 어쨌든 SQS 호출이 가능하긴 함
- Lambda가 Amazon Athena SQL 쿼리를 실행하면 Athena 쿼리는 수집 버킷에서 데이터를 가져와 SQL 쿼리를 생성하는데, 전부 서버리스임
	- 이런 서버리스 쿼리 출력값을 보고 버킷으로 이동
	- 다시 Amazon S3에 있는 다른 버킷으로 이동
- 이 과정에서 데이터를 보고하고 정리, 분석까지 마침
- QuickSight를 통해 직접 시각화할 수도 있음
	- QuickSight는 Amazon S3 버킷으로 데이터를 시각화하거나 Amazon Redshift 같은 데이터 웨어하우스에 데이터를 입력해 분석하기도 함
		- Redshift는 서버리스가 아니라는 점 기억해야 함
		- Redshift 데이터 웨어하우스는 QuickSight의 엔드 포인트로 작용
- 빅 데이터 수집 파이프라인이 뭔지 핵심을 짚어 보자면, 실시간 데이터 수집과 변형, 서버리스 Lambda와 Redshift 활용, 데이터 웨어하우스, QiuckSight 활용 시각화를 꼽을 수 있음

## Big Data Ingestion Pipeline discussion

- 이 파이프라인을 좀 더 알아보자면
- IoT Core는 여러 IoT 장치에서 데이터 수집을 허용
- Kinesis는 실시간 데이터 수집에 적합하고, Firehose는 실시간에 가깝게 S3로 데이터를 운반하도록 도움
- 선택 가능한 최저 간격은 1분
- Lambda는 Firehose를 도와 데이터를 변형하고 이후 Amazon S3가 SQS, SNS, Lambda에 알림을 실행
- Lambda는 SQS를 구독할 수 있는데 S3를 람다에 연결하기도 가능
- Athena는 서버리스 SQL 서비스로 Athena 결과는 S3에 직접 저장됨
- 보고 버킷은 분석된 데이터를 보관
- 추가로 분석이 필요하면 시각화에 쓰이는 QuickSight 혹은 Redshift 등의 보고 도구를 사용하면 됨
