# Storage in the Cloud

![stic](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/stic1.png)

# `Cloud Storage`

[Cloud Storage](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/Core/Storage/Storage.md)

- 개발자 및 IT 조직에 내구성과 가용성이 뛰어난 객체 스토리지를 제공하는 서비스

## Object Storage

- 컴퓨터 데이터 스토리지 아키텍처
- 데이터를 객체로 관리 (파일 스토리지인 파일 및 폴더 계층 구조나 블록 스토리지인 디스크 청크로 관리 X)
- 객체는
    - 실제 데이터 자체의 바이너리 형식
    - 생성 날짜, 작성자, 리소스 유형, 권한과 같은 메타데이터
    - 전역 고유 식별자
    - 를 포함하는 패키지 형식으로 저장됨
- 고유 식별자는 URL 형식으로 되어있어 웹 기술과 원활하게 상호작용할 수 있음
- 일반적으로 객체로 저장되는 데이터로는 동영상, 사진, 오디오 녹음본 등이 있음

## Cloud Storage

- Google Cloud가 제공하는 객체 스토리지 제품
- 고객은 원하는 만큼 데이터를 저장하고 필요할 때마다 데이터를 가져올 수 있음
- 확장 가능한 완전 관리형 서비스
- 사용 예
    - 정적 웹 사이트 콘텐츠 제공
    - 보관 처리 및 재해 복구를 위한 데이터 저장
    - 직접 다운로드 방식으로 최종 사용자에게 대량의 데이터 객체 배포
    - 등등
- 바이너리 대형 객체(BLOB) 스토리지가 필요할 때 주로 사용됨
    - 동영상 및 사진과 같은 온라인 콘텐츠
    - 데이터 백업 및 보관 처리
    - 중간 결과를 진행하고 워크플로를 진행하는 경우
- Cloud Storage는 Bucket으로 구성됨
    - Bucket
        - 전역 고유 식별자
        - 버킷이 저장될 특정 지리적 위치
            - 지연 시간을 최소화할 수 있는 사용자에게 가까운 Region이 이상적
- 스토리지 객체는 변경 불가능
    - 변경 사항이 있을 때마다 덮어써 새로운 버전이 생성됨
    - 버전 관리를 사용 설정해 특정 객체의 변경 사항을 자세히 기록해 놓을 수 있음
        - 필요에 따라 보관 처리된 버전의 객체를 나열하거나, 객체를 이전 상태로 복원하거나, 객체 버전을 영구히 삭제할 수 있음
- 대부분의 경우 개인 식별 정보가 데이터 객체에 포함될 수 있으므로 보안 및 개인 정보 보호를 유지하려면 저장된 데이터에 대한 액세스를 제한해야 함
    - IAM role을 사용한 액세스 제어
        - 대부분의 경우 IAM으로 충분
        - Role은 프로젝트에서 버킷, 객체로 상속됨
    - 액세스 제어 목록을 사용한 액세스 제어
        - 더 세밀하게 제어할 때 사용
        - 두 가지 정보로 구성됨
            - 범위: 작업에 액세스하고 수행할 수 있는 사용자 정의
            - 권한: 읽기 또는 쓰기와 같은 수행할 수 있는 작업 정의

# Cloud Storage: 스토리지 클래스 및 데이터 전송

## Storage Class

### 모든 스토리지 클래스에 적용되는 특성

- 최소 객체 크기 요건이 없는 무제한 스토리지
- 전세계적인 접근성과 위치
- 짧은 지연 시간과 높은 처리량
- 보안, 도구, API까지 이어지는 일관된 경험
- 데이터가 멀티 리전 또는 이중 리전에 저장되는 지리적 중복성
    - 물리적 서버를 지리적으로 다양한 데이터 센터에 배치하여 재난 이벤트 또는 자연 재해로부터 보호
    - 최적의 성능을 유지하기 위한 트래픽 부하 분산
- 최소 요금이 없는 이유는 사용한 만큼만 지불하고 용량을 사전에 프로비저닝할 필요가 없기 때문
- 보안
    - Cloud Storage가 추가 비용 없이 데이터를 디스크에 쓰기 전에 항상 서버 측에서 데이터를 암호화
    - 고객의 기기와 Google 사이를 이동하는 데이터는 전송 계층 보안인 HTTPS/TLS를 통해 암호화
- 여러가지 방법을 통해 Cloud Storage로 데이터를 가져올 수 있음
    - gsutil
        - Cloud SDK의 Cloud Storage 명령어로 자체 온라인 전송
    - Cloud Console에서 드래그 앤 드롭
    - Storage Transfer Service
        - 테라바이트 혹은 페타바이트 급의 데이터 업로드
        - 다른 클라우드 제공 업체의 Cloud Storage 리전 또는 HTTP(S) 엔드포인트에서 Cloud Storage로 일괄 전송을 예약하고 관리할 수 있음
    - Transfer Appliance
        - 랙 설치가 가능한 고용량 스토리지 서버
        - 하나의 Appliance에서 최대 페타바이트의 데이터까지 전송 가능
    - Google Cloud 서비스를 이용
        - Big Query와 Cloud SQL에서 테이블을 가져오고 내보낼 수 있음
        - Engine 로그, Firestore 백업, App Engine 애플리케이션에서 사용하는 이미지와 같은 객체도 저장
        - 인스턴스 시작 스크립트 저장
        - Compute Engine 이미지 저장
        - Compute Engine 애플리케이션에서 사용하는 객체 저장

![stic](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/stic2.png)

### `Standard storage`

- 빈번하게 액세스되는 데이터나 핫 데이터에 가장 적합
- 짧은 기간만 저장되는 데이터에 적합

### `Nearline storage`

- 데이터의 읽기 또는 수정이 평균 한 달에 한 번 이하로 자주 액세스되지 않는 데이터를 저장하는 데 가장 적합
- 데이터 백업, 장기 멀티미디어 콘텐츠나 데이터 보관 처리 등

### `Coldline storage`

- 자주 액세스되지 않는 데이터를 저장하는 저렴한 옵션
- 데이터의 읽기 또는 수정이 최대 90일에 한 번 일어나는 데이터를 저장하는 용도

### `Archive storage`

- 가장 저렴한 옵션
- 데이터 보관 처리, 온라인 백업, 재해 복구용으로 적합
- 일 년에 한 번 미만으로 액세스하려는 데이터에 가장 적합한 옵션
- 데이터 액세스 작업 비용이 높고 최소 저장 기간이 365일

# `Cloud Filestore`

[Cloud Filestore](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/Core/Storage/Filestore.md)

# `Cloud SQL`

[Cloud SQL](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/Core/Storage/SQL.md)

- 완전 관리형 관계형 데이터베이스
- MySQL, PostgreSQL, SQL Server가 있음
- 일상적이지만 시간이 많이 소요되는 필수 작업은 Google에서 맡도록 설계되어 있음
    - 패치, 업데이트 적용, 백업 관리, 복제본 구성 등
- 소프트웨어 설치나 유지보수가 필요하지 않음
- 수직적 확장 가능, 즉 단일 인스턴스의 컴퓨팅 파워를 늘릴 수 있음
- 최대 64개 프로세서 코어와 400GB를 초과하는 RAM, 30TB의 저장 용량으로 확장 가능
- 자동복제 시나리오: Cloud SQL 기본 인스턴스, 외부 기본 인스턴스, 외부 MySQL 인스턴스 등을 통해 지원
- 관리형 백업 지원
    - 백업된 데이터가 안전하게 저장되며 복구할 때 액세스할 수 있음
    - 하나의 인스턴스 비용에 백업 7개 포함
- 고객 데이터를 Google 내부 네트워크에 있을 때와 데이터베이스 테이블, 임시 파일, 백업에 저장될 때 암호화
- 네트워크 방화벽이 포함되어 있어 각 데이터베이스 인스턴스에 대한 네트워크 액세스 제어
- Google Cloud 서비스와 외부 서비스에 액세스 가능

# `Cloud Spanner`

[Cloud Spanner](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/Core/Storage/Spanner.md)

- 수평적으로 확장되고 강력한 일관성을 유지하며 SQL을 사용하는 완전 관리형 관계형 데이터베이스
- ACID(Atomicity, Consistency, Isolation, Durability) 규정을 준수
- 다음과 같은 특성이 필요한 애플리케이션에 적합
    - JOIN 및 보조 인덱스가 내장된 SQL 관계형 데이터베이스 관리 시스템
    - 고가용성 내장
    - 강력한 글로벌 일관성
    - 초당 많은 입출력 작업

# `Cloud Firestore`

[Cloud Firestore](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/Core/Storage/Firestore.md)

- 유연하고 수평적으로 확장 가능한 NoSQL 클라우드 데이터베이스로, 모바일, 웹, 서버 개발용으로 사용
- 데이터가 문서로 저장된 다음 컬렉션으로 구성됨
    
    ![stic](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/stic3.png)
    
    - 하위 컬렉션 외에도 복잡한 중첩된 객체를 문서에 포함할 수 있음
- Firestore의 NoSQL 쿼리를 사용하면 개별 특정 문서를 가져오거나 쿼리 매개변수와 일치하는 컬렉션의 모든 문서를 가져올 수 있음
    - 쿼리에는 여러 개의 연결된 필터가 포함될 수 있으며 필터링 및 정렬 옵션의 결합도 가능
- 기본적으로 인덱스가 생성되어 쿼리 성능이 데이터 세트가 아닌 결과 세트의 크기에 비례
- 데이터 동기화를 사용해 연결된 모든 기기의 데이터를 업데이트
- 간단한 일회성 조회 쿼리도 효율적으로 진행하도록 설계
- 앱에서 많이 사용되는 데이터를 캐시하기 때문에 기기가 오프라인 상태여도 앱에서 데이터를 읽고 쓰고 수신 대기하며 쿼리할 수 있음
    - 온라인 상태로 전환되면 Firestore에서 모든 로컬 변경 사항을 다시 Firestore로 동기화
- Google Cloud의 강력한 인프라, 자동 멀티 리전 데이터 복제, 높은 일관성 보장, Atomic 일괄 작업, 실제 트랜잭션 지원을 활용
- 가격 책정
    - 수행하는 문서 읽기, 쓰기, 삭제 **건별로** 요금 부과
    - 쿼리가 데이터를 반환하는 지 여부와 상관 없이 쿼리당 하나의 문서를 읽는 요율로 청구
    - 데이터가 소모하는 스토리지의 양과 데이터에 액세스하는 데 사용되는 특정 종류의 네트워크 대역폭에 대한 요금 청구
    - 무료 기본 할당량 제공

<aside>
💡 Atomic batch operation: 여러 개의 쿼리나 작업을 하나의 단위로 묶어서 한 번에 실행하는 작업

</aside>

# `Cloud Bigtable`

[Cloud Bigtable](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/Core/Storage/Bigtable.md)

- Google의 NoSQL, 빅데이터, 데이터베이스 서비스
- Google 검색, 애널리틱스, 지도, Gmail 등 Google의 수많은 핵심 서비스를 운영하는 데이터베이스
- 대용량 워크로드를 일관된 짧은 지연 시간과 높은 처리량 수준에서 처리하도록 설계됨
    - 사물 인터넷, 사용자 분석, 회계 자료 분석을 비롯한 운영 및 분석 애플리케이션에 모두 적합
- 스토리지 옵션으로 Bigtable을 선택하는 고객 예시
    - 1TB를 초과하는 반구조화 또는 구조화 데이터 작업을 수행하는 경우
    - 데이터가 고속으로 처리되거나 빠르게 변화하는 경우
    - NoSQL 데이터를 작업하고 있는 경우
    - 데이터가 시계열이거나 자연 시맨틱 순서를 가지고 있는 경우
    - 빅데이터를 작업하고 데이터에 대한 비동기식 일괄 또는 동기식 실시간 처리를 실행하는 경우
    - 데이터에서 머신러닝 알고리즘을 실행하는 경우
- Cloud Bigtable은 다른 Google Cloud 서비스나 서드 파티 클라이언트와 상호작용 가능
    - API를 사용하면 관리형 VM, HBase REST 서버 또는 HBase 클라이언트를 사용하는 자바 서버와 같은 데이터 서비스 레이어를 통해 Cloud Bigtable에서 데이터를 읽고 쓸 수 있음
        - 이 방법은 일반적으로 애플리케이션, 대시보드, 데이터 서비스에 데이터를 제공하는 데 사용됨
    - 스트림 처리 프레임워크인 Dataflow Streaming, Spark Streaming, Storm 등을 통해 데이터를 스트리밍 할 수도 있음
    - 스트리밍이 불가능한 경우 Hadoop 맵리듀스, Dataflow, Spark와 같은 일괄 처리를 통해 Cloud Bigtable에서 데이터를 읽고 쓸 수 있음

# `Cloud Memorystore`

[Cloud Memorystore](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/Core/Storage/Memorystore.md)

# Comparing Storage Options

## `Cloud Storage`

- 큰 이미지나 영화와 같이 10MB보다 큰 변경 불가능한 blob을 저장해야 하는 경우
- 개체당 최대 단위 크기가 5TB인 페타바이트 용량을 제공

## `Cloud SQL`

- 온라인 트랜잭션 처리 시스템에 대한 완전한 SQL 지원이 필요한 경우
- 머신 유형에 따라 최대 64TB를 제공
- 사용자 자격 증명 및 고객 주문 저장과 같은 웹 프레임워크 및 기존 애플리케이션에 적합

## `Cloud Spanner`

- 온라인 트랜잭션 처리 시스템에 대한 완전한 SQL 지원이 필요한 경우
- 머신 유형에 따라 페타바이트 제공
- 읽기 복제본이 아니라 수평적 확장성이 필요한 경우

## `Cloud Firestore`

- 실시간 쿼리 결과 및 오프라인 쿼리 지원과 함께 대규모 확장 및 예측 가능성이 필요한 경우
- 엔터티당 최대 단위 크기가 1MB인 테라바이트 용량 제공
- 모바일 및 웹 앱용 데이터 저장, 동기화, 쿼리에 적합

## `Cloud Bigtable`

- 많은 수의 구조화된 개체를 저장해야 하는 경우
- SQL 쿼리를 지원하지 않으며 다중 행 트랜잭션도 지원하지 않음
- 최대 단위 크기가 셀당 10MB, 행당 100MB인 페타바이트 용량 제공
- AdTech, 금융 또는 IoT 데이터와 같이 읽기 및 쓰기 이벤트가 많은 분석 데이터에 가장 적합