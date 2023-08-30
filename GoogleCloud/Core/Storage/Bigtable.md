# Cloud Bigtable

- 트랜잭션 일관성이 필요하지 않을 때 고려할 수 있음
- 완전 관리형 NoSQL 데이터베이스로 페타바이트 규모이며 지연 시간이 매우 짧음
- 처리량에 맞게 원활히 확장되며 특정 액세스 패턴에 적응하는 방법을 학습
- 실제로 Google 검색, 애널리틱스, 지도, Gmail을 비롯한 Google의 수많은 핵심 서비스를 운영하는 데이터베이스
- Cloud Bigtable이 IoT, 사용자 분석, 재무 데이터 분석을 포함해 운영 및 분석 애플리케이션에 적합한 이유는 지연 시간이 짧으면서도 높은 읽기 및 쓰기 처리량을 지원하기 때문
- 머신러닝 애플리케이션을 위한 훌륭한 스토리지 엔진이기도 함
- Hadoop, Cloud Dataflow, Cloud Dataproc 등 널리 사용되는 빅데이터 도구와도 쉽게 연동
- 업계 표준의 HBase API를 지원하기 때문에 개발팀이 쉽게 익힐 수 있음

# Cloud Bigtable storage model

![btb](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/btb1.png)

- Cloud Bigtable은 각각 정렬된 키 값 매핑으로 구성되어 있고 대규모로 확장 가능한 테이블에 데이터를 저장
- 이 테이블은 일반적으로 단일 항목을 기술하는 행과 각 행의 개별 값을 포함하는 열로 구성됨
- 각 행은 단일 Row key로 인덱스가 생성되고 서로 연관된 열은 일반적으로 Column family로 그룹화
- 각 열은 Column family와 Column family 내 고유 이름인 Column qualifier의 조합으로 식별됨
- 각 행 열 교집합에는 여러 타임스탬프가 기록된 다양한 셀 또는 버전이 포함될 수 있으므로 저장된 데이터가 시간에 따라 어떻게 변경되었는지에 대한 레코드를 얻게됨
- Cloud Bigtable 테이블은 희소 테이블이므로 셀에 데이터가 없으면 공간을 차지하지 않음
- 위 사진 예시
    - 미국 대통령을 위한 가상의 SNS를 가정한 것으로 각 대통령은 다른 대통령의 게시물을 팔로우할 수 있음
    - 이 테이블에는 Follows 그룹이라는 Column family가 하나 있음
        - 이 그룹에는 여러 개의 Column qualifier가 포함됨
        - Column qualifier는 데이터처럼 사용됨
    - 이러한 디자인 옵션은 Cloud Bigtable 테이블의 희소성과 데이터가 변경될 때 새로운 Column qualifier를 추가할 수 있는 기능 덕분에 가능함
    - 사용자 이름은 Row key로 사용됨
        - 사용자 이름에 알파벳이 고르게 사용되었다면 전체 테이블에서 데이터 액세스가 고르게 이뤄지는 것이 당연함

# 단순화한 Cloud Bigtable architecture

![btb](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/btb2.png)

- Frontend 서버 풀과 노드를 통해 수행되는 처리가 스토리지와는 별도로 처리됨
- Cloud Bigtable 테이블은 태블릿이라고 하는 연속된 행의 블록으로 분할되어 쿼리 워크로드를 분산
    - 태블릿은 Google file system인 Colossus에 SSTable 형식으로 저장됨
    - SSTable은 영구적이고 불변하며 정렬된 키-값 매핑을 제공하는데, 키와 값은 모두 임의 바이트 문자열

# Cloud Bigtable의 액세스 패턴 학습

![btb](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/btb3.png)

- Cloud Bigtable은 특정 액세스 패턴에 적응하는 방법을 학습
    - 특정 Bigtable 노드가 특정 데이터 하위 집합에 자주 액세스하는 경우 Cloud Bigtable은 여기서 볼 수 있는 것처럼 다른 노드가 해당 워크로드를 고르게 분배할 수 있도록 인덱스를 업데이트

# Throughput의 Linear한 확장

![btb](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/btb4.png)

- 처리량의 선형적인 확장
- 추가하는 모든 단일 노드에 최대 수백 개의 노드까지 처리량 성능이 선형으로 확장됨

# Cloud Bigtable 결정 트리

![btb](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/btb5.png)

- 1TB 이상의 구조화된 데이터를 저장해야 하거나
- 볼륨이 매우 크거나
- Strong consistency와 함께 10ms 미만의 읽기/쓰기 지연 시간이 필요하거나
- HBase API와 호환되는 스토리지 서비스가 필요한 경우 Cloud Bigtable 사용

# 기타

- 생성할 수 있는 가장 작은 Cloud Bigtable 클러스터에는 3개의 노드가 있고 초당 30,000개의 작업 처리 가능
- 애플리케이션에서 노드를 사용하는지 여부와 관계 없이 작동하는 노드에 대해 비용 지불