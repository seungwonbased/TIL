# Cloud Firestore

- 글로벌 규모의 모바일, 웹 및 IoT 앱용 데이터의 저장, 동기화, 쿼리를 간소화하는 클라우드 기반의 완전 관리형 고속 NoSQL 문서 데이터베이스
- 클라이언트 라이브러리는 실시간 동기화와 오프라인 지원 제공
- 보안 기능과 Firebase 및 Google Cloud와의 통합은 진정한 서버리스 앱 빌드를 가속화
- Cloud Firestore는 ACID 트랜잭션을 지원하므로 트랜잭션 작업이 하나라도 실패하고 다시 시도할 수 없는 경우 전체 트랜잭션이 실패
- 자동 멀티 리전 복제 및 Strong consistency를 통해 재해가 발생해도 데이터를 안전하게 보호하고 문제없이 사용할 수 있음
- 성능 저하 없이 NoSQL 데이터에 대해 정교한 쿼리 실행 가능
    - 따라서 데이터를 보다 유연하게 구성 가능

# Cloud Datastore과 호환

- Cloud Firestore는 차세대 Cloud Datastore
    - 따라서 Datastore 모드로 작동할 수 있으며 Cloud Datastore과 하위 호환됨
- Datastore mode
    - Datastore 모드에서 Cloud Firestore 데이터베이스를 만들면 Cloud Datastore 시스템 동작을 유지하면서 Firestore의 향상된 스토리지 레이어에 액세스 가능
        - 이렇게 하면 다음과 같은 Cloud Datastore 제한사항이 사라짐
            - 쿼리가 더 이상 Eventual consistency를 가지지 않고 Strong consistency를 가짐
            - 트랜잭션이 더 이상 25개 항목 그룹으로 제한되지 않음
            - 항목 그룹에 대한 쓰기가 더 이상 초당 1회로 제한되지 않음
- Native mode
    - Strong consistency를 갖춘 새로운 스토리지 레이어
    - 컬렉션 문서 및 데이터 모델
    - 실시간 업데이트
    - 모바일 및 웹 클라이언트 라이브러리
    - Cloud Firestore는 Datastore의 이전 버전과 호환되지만 새로운 데이터 모델, 실시간 업데이트, 모바일 및 웹 클라이언트 라이브러리 기능은 호환되지 않음
        - 따라서 새로운 모든 Firestore 기능에 액세스하려면 기본 모드의 Firestore를 사용해야 함
        - **일반적인 지침은 새 서버 프로젝트의 경우 Datastore 모드의 Firestore를 사용하고, 새 모바일 및 웹 앱의 경우 Native 모드를 사용하는 것**
- Cloud Firestore는 모든 Cloud Datastore API 및 클라이언트 라이브러리와 호환됨
- 기존 Cloud Datastore 사용자는 향후 Firestore로 자동 실시간 업그레이드됨

# Cloud Firestore 결정 트리

![fst](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/fst1.png)