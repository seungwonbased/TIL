# Resources and Access in the Cloud

# Google Cloud 리소스 계층

![rna](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/rna1.png)

- `Policy`는 프로젝트, 폴더, 조직 노드 수준에서 정의될 수 있음
    - 일부 Google Cloud 서비스에서는 개별 리소스에도 Policy를 적용할 수도 있긴 함
    - Policy는 아래로 상속됨

## Level 01: `Resource`

- VM, Cloud Storage bucket, BigQuery table, Google Cloud의 기타 항목 등

## Level 02: `Project`

- Google Cloud 서비스를 사용 설정하고 사용하기 위한 기반
- API 관리, 결제 사용 설정, 공동 작업자 추가 및 제거, 기타 Google 서비스 사용 설정 등
- 각 리소스는 정확히 하나의 프로젝트에만 속함
- 프로젝트는 별도로 청구되고 관리되므로 소유자와 사용자가 다를 수 있음
- 식별 속성
    - `Project ID`: 전역적으로 고유한 식별자로 Google에서 할당 후 변경 불가능
    - `Project name`: 사용자가 만드는 것으로 고유하지 않아도 되고 언제든 변경 가능
    - `Project number`: Google Cloud가 고유한 프로젝트 번호로, Programmatic하게 프로젝트 관리 가능하게 함 (RPC API, REST API 등을 통해)

## Level 03: `Folder`

- 원하는 세부사항 수준에서 리소스에 Policy를 할당 가능
- 폴더에는 프로젝트, 다른 폴더 또는 이 두 가지 조합이 포함될 수 있음
- 폴더의 리소스는 해당 폴더에 할당된 정책 및 권한을 상속 받음
- 계층 구조의 조직에 속한 프로젝트를 그룹화 가능
- 팀에 관리 권한을 위임하여 독립적으로 작업하도록 할 수 있음
    - 한 팀에서 관리하는 두 개의 다른 프로젝트가 있는 경우 Policy를 공통 폴더에 넣어 프로젝트에 동일한 권한을 갖게 할 수 있음

## Level 04: `Organization node`

- Google Cloud 계층 구조의 최상위 리소스
- 해당 계정에 연결된 모든 항목은 본 노드에 속하게 됨
    - 폴더, 프로젝트, 기타 리소스 등
- 선택 사항이지만 기업이나 조직 내에서는 효과적 관리를 위해 거의 사용함
- 역할
    - 조직 정책 관리자를 지정해 권한이 있는 사람만 Policy를 변경할 수 있도록 할 수 있음
    - 프로젝트 생성자 역할
- Workspace 도메인이 있는 경우 Google Cloud 프로젝트는 자동으로 조직 노드에 속하게 됨
    - 그렇지 않다면 Cloud ID로 조직 노드를 만들 수 있음
        - Cloud ID는 ID, 액세스, 애플리케이션, 엔드포인트를 관리하는 Google의 플랫폼
- 새 조직 노드를 만들면 도메인의 모든 사람이 프로젝트와 결제 계정을 만들 수 있음

# IAM (Identity and Access Management)

- 조직 노드에 많은 폴더, 프로젝트 및 리소스가 포함된 경우 누가 무엇에 액세스할 수 있는지 제한
    - IAM을 사용하여 관리자는 누가 어떤 리소스에서 무엇을 할 수 있는지 정의하는 정책을 적용
- 상위 레벨에서 정의된 Policy는 하위 레벨에 상속됐을 때 하위 레벨에서 Override 가능
- Role 권한 모음

## Who

- `Google account`
- `Google group`
- `Service account`
- `Cloud ID domain`

## What

- `Role`이라는 것에 의해 정의됨
    - 예를 들어 프로젝트에서 VM 인스턴스를 관리하려면 VM의 생성, 삭제, 시작, 중지 및 변경이 있어야 하는데 이러한 권한은 이해하기 쉽고 관리하기 쉽도록 Role로 그룹화
- 사용자, 그룹 또는 서비스 계정에 특정 요소에 대한 Role이 부여된 경우 리소스 계층에서 결과 Policy는 선택한 요소와 계층에서 그 아래에 있는 모든 요소에 모두 적용됨
- `Role` 유형
    - `Basic IAM role`
        - 범위가 매우 광범위
        - Google Cloud 프로젝트에 적용하면 해당 프로젝트의 모든 리소스에 영향
        - Owner
            - 리소스에 액세스하고 변경할 수 있음
            - 연결된 역할 및 권한을 관리하고 결제를 설정할 수 있음
        - Editor
            - 리소스에 액세스하고 변경할 수 있음
        - Viewer
            - 리소스에 액세스할 수 있지만 변경할 수 없음
        - Billing Admin
            - 리소스에 액세스하고 결제를 설정할 수 있음
    - `Predefined IAM role`
        - 특정 Google Cloud 서비스는 사전 정의된 Role 집합을 제공하면 이러한 Role을 적용할 수 있는 위치도 정의
        - 예
            - Compute Engine
                - 특정 프로젝트, 특정 폴더 또는 전체 조직의 Compute Engine 리소스에 ‘instanceAdmin’과 같은 사전 정의된 Role을 적용할 수 있음
                - 이러한 Role을 부여받은 사람은 누구든지 미리 정의된 특정 작접 집합을 수행 가능
    - `Custom IAM role`
        - 더 구체적인 권한이 있는 역할을 할당
        - 많은 회사에서 조직의 각 개인의 ‘최소 권한’ 모델을 사용
            - 작업을 수행하는 데 필요한 최소한의 권한이 부여됨
        - Role 생성을 시작하기 전 중요한 세부 정보
            - Custom IAM role을 정의하는 권한을 관리해야 함
            - 프로젝트 수준, 조직 수준에만 적용할 수 있고 폴더 수준에는 적용할 수 없음

## Identity and Access Management

[Identity and Access Management](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/Core/ResourceAccess/IAM.md)

# Service Account

- 사람이 아닌 Compute Engine 가상 머신에 권한을 부여할 때 사용하는 서비스 계정
    - 예
        - 가상 머신에서 실행 중인 애플리케이션이 Cloud Storage에 데이터를 저장한다고 했을 때, 인터넷 상 누구도 해당 데이터에 엑세스하지 못하고 특정 가상 머신만이 액세스하기를 원하면 서비스 계정을 만들어 VM을 Cloud Storage에 인증할 수 있음
- 이메일 주소로 명명됨
- 비밀번호 대신 암호화 키를 사용해 리소스에 액세스
- 서비스 계정에 Compute Engine의 인스턴스 관리자 역할이 부여되면 해당 서비스 계정으로 VM에서 실행 중인 애플리케이션이 다른 VM을 생성, 수정, 삭제할 수 있음
- 서비스 계정은 ID일 뿐만 아니라 리소스이기도 해서 연결된 자체 IAM 정책을 가질 수 있음

# Cloud Identity

- 새로운 Google Cloud 고객이 플랫폼을 사용할 때, Gmail 계정으로 Google Cloud Console에 로그인한 다음 Google 그룹스를 사용해 유사한 역할을 맡은 팀원과 공동 작업을 하는 것이 일반적
    - 이 방식은 시작하기는 쉽지만 팀의 ID가 중앙에서 관리되지 않기 때문에 나중에 문제가 발생할 수 있음
        - 예를 들어 누군가가 조직을 떠나는 경우 팀의 클라우드 리소스에서 사용자 액세스를 즉시 제거하는 쉬운 방법이 없음
- 조직은 `Cloud ID`라는 도구를 사용해 Policy를 정의하고 Google Admin Console을 사용해 사용자 및 그룹을 관리할 수 있음
- 관리자는 동일한 사용자 이름과 비밀번호를 입력하여 Google Cloud 리소스에 로그인하고 관리할 수 있음
- Cloud ID를 사용한다는 것은 누군가가 조직을 떠날 때 Google Admin Console을 사용해 계정을 비활성화하고 그룹에서 제거할 수 있음을 의미
    - 무료 버전
    - Premium
        - 모바일 장치 관리
        - Google Cloud 고객이면서 Google Workspace 고객인 경우 이 기능은 사용 가능

# Interacting with Google Cloud

## Google Cloud에 액세스하고 상호작용하는 네 가지 방법

### Google Cloud Console

- 간단한 웹 기반 인터페이스에서 프로덕션 문제를 배포, 확장 및 진단하는 데 도움이 되는 Google Cloud의 GUI
- 리소스를 쉽고 빠르게 찾고, 상태를 확인하고, 전체에 대한 관리 제어, 예산 설정 등 가능
- 브라우저에서 SSH를 통해 인스턴스에 연결할 수 있는 검색 기능도 제공

### Cloud SDK 및 Cloud Shell

- `Cloud SDK`
    - Google Cloud에 호스팅되는 리소스 및 애플리케이션을 관리할 수 있는 도구 모음
    - `gcloud`: Google Cloud 제품 및 서비스용 기본 CLI를 제공하는 도구
    - `gsutil`: 명령줄에서 Cloud Storage에 액세스하는 도구
    - `bq`: BigQuery용 명령줄 도구
    - Cloud SDK 내의 모든 도구는 bin 디렉터리 안에 있음
- `Cloud Shell`
    - 브라우저에서 직접 클라우드 리소스에 대한 명령줄 액세스 제공
    - 5GB의 영구 홈 디렉토리가 있는 Debian 기반 가상 머신
    - Google Cloud 프로젝트 및 리소스를 쉽게 관리 가능
    - 사용하면 Cloud SDK gcloud 명령어 및 기타 유틸리티가 항상 설치되고 최신 상태로 인증됨

### APIs

- Google Cloud를 구성하는 서비스는 개발자가 작성한 코드로 제어할 수 있도록 API를 제공
- Cloud Console에는 사용 가능한 API와 버전을 보여주는 Google API 탐색기라는 도구가 있음
- Google은 Client library와 Google API client libray를 여러 언어로 제공하여 코드에서 Google Cloud를 호출하는 작업에서 많은 수고를 덜어줌
    - Java, Python, PHP, C#, Go, Node.js, …

### Cloud Mobile App

- SSH를 시작, 중지, 사용하여 Compute Engine 인스턴스에 연결하고 각 인스턴스의 로그 조회 가능
- Cloud SQL 인스턴스를 중지하고 시작할 수 있음
- 오류 확인, 배포 롤백, 트래픽 분할 변경을 통해 App Engine에 배포된 여러 애플리케이션을 관리 가능
- 프로젝트에 대한 최신 결제 정보와 예산을 초과하는 프로젝트에 대해 경고를 받을 수 있음

<aside>
💡 LAMP: Linux, Apache, MySQL, PHP로 구성된 웹 스택

</aside>