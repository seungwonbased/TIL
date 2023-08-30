# Cloud Spanner

- 관계형 데이터베이스 구조와 비관계형 수평적 확장의 이점을 결합
- 페타바이트 규모의 용량 제공
- 글로벌 규모의 트랜잭션 일관성을 비롯해 스키마, SQL 및 고가용성을 위한 자동 동기식 복제 제공
- 월간 업타입 SLA
    - Multi-regional: 99.999%
    - Regional: 99.999%

# 사용 사례

- 관계형 데이터베이스 기술을 사용했던
    - 재무 애플리케이션
    - 인벤토리 애플리케이션
- 금융 서비스 및 소매 산업에서 거래 및 재고 관리를 위해 일관된 시스템을 구축하는 경우
- 등등

# 비교

![span](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/span1.png)

# Cloud Spanner architecture

![span](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/span2.png)

- 인스턴스는 N개의 클라우드 영역에서 데이터를 복제하며 이 영역들은 한 리전 안에 있거나 여러 리전에 걸쳐 있을 수 있음
- 데이터베이스 배치는 구성 가능
    - 즉, 데이터베이스를 배치할 리전을 선택할 수 있음
- 이 아키텍처는 고가용성과 전역 배치를 허용
- 데이터 복제는 Google의 글로벌 광섬유 네트워크를 사용해 영역 간에 동기화됨
- 원자 시계를 사용하면 데이터를 업데이트할 때마다 원자성이 보장됨
    - 원자성 (Atomicity): 데이터베이스 트랜잭션(Transaction)의 속성 중 하나로, 여러 개의 작업을 하나의 논리적인 작업 단위로 묶어서 모두 성공하거나 모두 실패하도록 보장하는 것

# Cloud Spanner 결정 트리

![span](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/span3.png)