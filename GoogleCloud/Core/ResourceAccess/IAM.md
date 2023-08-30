# Identity and Access Management

# IAM

- **누가** 어떤 리소스에서 **무엇**을 할지 식별하는 방법

## Who

- User
- Group
- Application

## What

- 특정 권한 또는 작업

# IAM Objects

![iam](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/iam1.png)

# IAM resource hierarchy

![iam](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/iam2.png)

## Organization

- Google Cloud 리소스 계층 구조의 루트 노드
- 프로젝트 단독으로 리소스를 사용 가능하지만, 기업 환경에 적합하게 설계하고 싶은 경우 조직 구조에 맞게 매핑 가능
    - 프로젝트 단독 사용은 권장하지는 않음

### Organization roles

- Organization Admin: 조직에 속한 모든 리소스를 관리할 수 있는 액세스 권한 제공
- Project Creator: 조직 내에서 프로젝트를 생성할 수 있는 역할
    - 프로젝트 생성자가 조직 역할에 있는 이유는 조직 수준에서도 적용할 수 있기 때문
    - 이렇게 하면 이 역할은 조직 내 모든 프로젝트에 상속됨
- 등등

### Creating and managing organizations

- Google Workspace (G Suite) 또는 Google Identity 계정이 Google Cloud 프로젝트를 만들 때 조직이 생성됨
    - 자동으로 프로비저닝
    - 조직과 조직의 모든 리소스를 제어할 수 있기 때문에 이러한 최고 관리자 계정은 신중하게 사용
- G Suite & Cloud ID 최고 관리자 / Google Cloud Organization Admin은 설정 프로세스에서 조직 리소스의 수명 주기 제어를 위한 핵심 역할
    - 일반적으로 서로 다른 사용자나 그룹에 할당됨
    - **Google Workspace와 Cloud ID 최고 관리자의 책임**
        - 일부 사용자에게 조직 관리자 (Organization Admin) 역할 할당
        - 복구 문제가 발생할 경우 담당자 역할 수행
        - 조직 리소스에서 Google Workspace 또는 Cloud ID 계정의 수명 주기 제어
    - **조직 관리자(Organization admin)의 책임**
        - IAM 정책을 정의
        - 리소스 계층 구조의 구조 결정
        - IAM 역할을 통해 네트워킹, 결제, 리소스 계층 구조와 같은 중요한 구성 요소에 대한 책임을 위임
        - 포함되지 않는 것: 최소 권한 원칙에 따라 폴더 만들기와 같은 다른 작업을 수행할 수 있는 권한
            - 이 권한을 얻으려면 조직 관리자가 계정에 추가 역할을 할당해야 함

## Folder

- 폴더는 프로젝트 간의 추가 그룹화 매커니즘 및 경계 격리를 제공
- 회사 내의 다양한 법인, 부서, 팀을 모델링하는 데 사용할 수 있음
- 폴더가 있으려면 조직이 먼저 필요
- 폴더는 프로젝트와 다른 폴더를 포함할 수 있음
- 폴더를 사용해 관리 권한을 위임할 수 있으므로 부서 책임자에게 해당 부서에 속하는 모든 Google Cloud 리소스의 완전한 소유권을 부여할 수 있음
- 리소스에 대한 액세스 권한은 폴더별로 제한할 수 있으므로 한 부서의 사용자는 해당 폴더 내의 Google Cloud 리소스만 액세스하고 만들 수 있음
- 정책은 위에서 아래로 상속됨

## Resource manager roles

- 정책은 위에서 아래로 상속됨
- 조직
    - Admin: 모든 리소스에 대한 모든 제어 권한
    - Viewer: 모든 리소스에 대한 읽기 권한
- 폴더
    - Admin: 폴더 안의 리소스에 대한 모든 권한
    - Creator: 관리자 역할 계층을 탐색하고 폴더를 생성할 수 있는 권한
    - Viewer: 폴더 안의 리소스에 대한 읽기 권한
- 프로젝트
    - Creator: 새 프로젝트 생성(자동으로 프로젝트 소유자가 됨)과 새 프로젝트를 조직 안으로 마이그레이션 할 수 있는 권한
    - Deletor: 프로젝트를 삭제할 수 있는 권한

# Role

- ‘어떤 리소스에서 무엇을 할 수 있는지’를 정의

## Basic role

- Cloud Console에서 사용할 수 있었던 원래 역할이며 매우 광범위함
- **Google Cloud 프로젝트**에 이를 적용하면 해당 프로젝트의 모든 리소스에 영향
- 즉, IAM 기본 역할은 대략적인 고정 액세스 수준을 제공
- Owner role은 하위 Editor, Viewer role의 권한을 모두 보유
- 각 프로젝트에는 소유자, 편집자, 뷰어, 결제 관리자가 여러 명 있을 수 있음

### Owner

- 전체 관리 액세스 권한 보유
- 구성원 추가 및 삭제 권한 보유
- 프로젝트 삭제 권한 보유

### Editor

- 수정 및 삭제 액세스 권한 보유
    - 이를 통해 개발자는 애플리케이션을 배포하고 해당 리소스를 수정하거나 구성할 수 있음

### Viewer

- 읽기 전용 액세스 권한 보유

### Billing Administrator

- 결제를 관리하는 권한 보유
- 관리자를 추가 또는 삭제하는 권한 보유
- 프로젝트 리소스를 변경하는 권한은 없음

## Predefined role

- Google Cloud가 제공하는 사전 정의된 고유한 역할 모음
    - 해당 역할을 어디에 적용할 수 있는지도 정의되어 있음
- 구성원에게 특정 리소스에 대한 세부적인 액세스 권한을 부여하고 다른 리소스에 대한 무단 액세스를 방지
- 예시
    
    ![iam](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/iam3.png)
    
    - project_a에 대한 InstanceAdmin role을 특정 사용자 그룹에 부여: 해당 그룹의 사용자에게 Compute Engine 권한을 비롯해 다양한 권한이 부여됨
- 권한 자체는 API의 클래스 및 메서드
    - 예를 들어 ‘compute.instances.start’는 서비스, 리소스, 동사로 나눌 수 있음
    - 이 권한은 중지된 Compute Engine 인스턴스를 시작하는 데 사용됨
    - 이러한 권한은 일반적으로 작업의 해당 REST API와 일치

### Predefined Compute Engine IAM roles

- Compute Engine에는 사전 정의된 몇 가지 IAM 역할이 있음
- Compute Admin: 모든 Compute Engine 리소스를 관리할 수 있는 전체 권한 부여, 여기에는 compute로 시작되는 모든 권한 포함
- Network Admin: 방화벽 규칙, SSL 인증서, 인스턴스에 대한 읽기 전용 권한 (임시 IP 주소를 볼 수 있음) 부여
- Storage Admin: 디스크, 이미지, 스냅샷을 생성, 수정, 삭제할 수 있는 권한 부여
    - 예를 들어 회사에서 프로젝트 이미지를 관리하는 담당자에게 프로젝트에 대한 편집자 역할을 부여하지 않으려면 해당 계정에 프로젝트에 대한 스토리지 관리자 역할을 부여
- 이외에도 많은 사전 정의된 Compute Engine 역할이 있음

## Custom role

- 위의 역할로도 충분하지만 만약 필요한 권한이 없다면 커스텀 역할을 사용하면 됨
- 직접 권한을 선택해서 역할을 만들면 됨

## 최소 권한 모델

- 구성원들에게 꼭 필요한 액세스나 권한을 부여하여 최소한의 권한만을 갖게 하는 것

# Member

## Google 계정

- 개발자와 관리자, Google Cloud와 상호작용하는 모든 사람
- Google 계정과 연결된 모든 이메일 주소는 ID가 될 수 있음
- 신규 사용자는 Google 계정 가입 페이지로 이동해 Google 계정에 가입할 수 있음

## 서비스 계정

- 개별 최종 사용자가 아니라 **애플리케이션에 속한 계정**
- Google Cloud에서 호스팅되는 코드를 실행할 때 코드를 실행하는 계정을 지정
- 애플리케이션의 다양한 논리적 구성 요소를 나타내는 데 필요한 개수대로 서비스 계정 생성 가능

## Google 그룹

- Google 계정과 서비스 계정을 모아 이름을 붙인 계정 컬렉션
- 그룹마다 그룹에 연결된 고유 이메일 주소가 있음
- Google 그룹으로 여러 사용자의 컬렉션에 액세스 정책을 쉽게 적용 가능
    - 정책을 전체 그룹에 부여하고 변경 가능

## Google Workspace 도메인

- Workspace 계정에서 생성된 모든 Google 계정의 가상 그룹
- Workspace 도메인은 조직의 Workspace 계정에서 생성된 모든 Google 계정의 가상 그룹을 나타냄
- Workspace 도메인은 조직의 인터넷 도메인 네임을 나타냄
    - example.com
- Workspace 도메인에 사용자를 추가하면 해당 가상 그룹 내에 이 사용자에 대한 새로운 Google 계정이 ‘username@example.com’과 같이 생성됨

## Cloud ID 도메인

- Workspace 고객기 아닌 Google Cloud 고객은 Cloud ID를 통해 Workspace 도메인과 동일한 기능 사용 가능
- Cloud ID를 사용하면 Cloud Console을 사용해 사용자와 그룹을 관리할 수 있지만 Gmail, Docs, Drive와 같은 Workspace의 공동 작업 제품에 대한 비용을 지불하지 않으며 제품이 제공되지도 않음
- 한 가지 유의해야 할 점: 사용자나 그룹을 생성 또는 관리하는 데 IAM을 사용할 수 없음
    - 대신 Cloud ID나 Workspace로 사용자를 관리할 수 있음

# Policy

- 리소스에 연결된 액세스 구문의 모음
- 바인딩의 목록으로 구성됨
- 바인딩: 구성원의 목록을 역할에 바인딩, 구성원과 역할 간의 연결
    - 역할 바인딩은 특정 구성원에게 특정 역할을 할당함으로써 권한 부여
    - 구성원 → Google 계정, Google 그룹, Google Workspace 도메인, 서비스 계정
    - 역할은 IAM에 의해 정의되는 명명한 권한 목록
- 각 정책에는 상위 항목으로부터 정책을 상속하는 리소스와 함께 역할 및 역할 구성원의 집합이 포함
- 리소스 정책은 리소스와 상위 리소스의 집합이며 덜 제한적인 상위 정책이 더 제한적인 리소스 정책보다 우선
- 리소스 계층 구조를 변경하면 정책 계층 구조도 변경됨
- 하위 정책은 상위 수준에서 부여된 액세스 권한을 제한할 수 없음
- 최소 권한의 원칙을 따르는 것이 좋음
    - ID, 역할, 리소스에 적용
    - 역할 권장 사항에 Recommander를 사용해 주 구성원의 초과 권한을 식별하고 삭제해 리소스의 보안 구성 개선 가능
        - 추천자는 정책 통계를 사용해 초과 권한을 식별

## IAM Condition

- IAM Condition을 사용하면 Google Cloud 리소스에 조건부 속성 기반 액세스 제어를 정의하고 적용할 수 있음
- 구성된 조건을 충족하는 경우에만 ID 혹은 구성원에게 리소스 액세스를 부여하도록 선택할 수 있음
- 조건은 리소스의 IAM 정책 중 역할 바인딩에 지정됨
- 조건이 있는 경우 조건 표현식이 true로 평가되어야만 액세스 요청이 승인됨
- 조건 표현식은 논리문의 집합으로 정의됨

## Organization Policy

- 조직에 대해 원하는 제한 사항으로 제약 조건을 구성해 정의
- 조직 정책은 조직 노드에 적용 가능
    - 그 안의 모든 폴더나 프로젝트에도 적용됨
- 조직 정책에 대한 예외는 조직 정책 관리자 역할이 있는 사용자만 만들 수 있음

# Google Cloud Directory Sync

- 이미 다른 회사 디렉터리가 있는 경우, 사용자와 그룹을 Google Cloud로 가져오고 싶은 경우 Google Cloud Directory Sync를 사용해 이미 사용 중인 사용자 이름과 비밀번호 그대로 로그인하고 Google Cloud 리소스를 관리할 수 있음
- 이 도구는 기존 Active Directory 또는 LDAP 시스템의 사용자 및 그룹을 Cloud ID 도메인의 사용자 및 그룹과 동기화
- 동기화는 단방향
    - Active Directory나 LDAP 맵의 정보는 수정되지 않음
- Google Cloud Directory Sync는 동기화 규칙이 설정된 후 감독 없이 예약된 동기화 실행
- Single Sign-On (SSO, 싱글 사인 온) 인증 제공
    - 자체 ID 시스템이 있다면 SSO가 구성된 자체 시스템 및 프로세스를 계속 사용 가능
    - 사용자 인증이 필요한 경우 Google에서 시스템으로 리디렉션
    - 사용자가 시스템에서 인증되면 Google Cloud 액세스 권한이 부여됨
        - 그렇지 않을 경우에는 로그인하라는 메세지가 표시
    - Google Cloud에 대한 액세스 권한 취소도 가능
    - 기존 인증 시스템이 SAML2 SSO를 지원하는 경우 링크 3개와 인증서 1개만으로 간단히 SSO를 구성할 수 있음
    - ADFS, Ping, Okta와 같은 타사 솔루션을 이용하는 방법도 있음

# Service Account

- 구성원의 한 유형
- 최종 사용자가 아니라 애플리케이션에 속한 계정
- 사용자 인증 정보를 제공하지 않은 채 프로젝트에서 서버 간 상호작용을 수행하기 위한 ID 제공
- 예
    - Google Cloud Storage와 상호작용하는 애플리케이션을 작성하는 경우 먼저 Google Cloud Storage XML API 또는 JSON API에 인증해야 함
    - 그러려면 서비스 계정을 설정하고 애플리케이션을 실행하려는 인스턴스에서 그 계정에 읽기/쓰기 액세스 권한을 부여한 다음 서비스 계정에서 사용자 인증 정보를 가져오도록 애플리케이션을 프로그래밍
    - 애플리케이션은 인스턴스, 이미지, 애플리케이션 코드에 보안 비밀 키나 사용자 인증 정보를 삽입하지 않고도 API에 원활하게 인증됨
- 이메일 주소로 식별됨
    - 예: 123432889684-compute@project.gserviceaccount.com

## 유형

### 사용자 생성 (커스텀) 서비스 계정

- 기본 제공 서비스 계정보다 훨씬 유연하지만 더 많은 관리 필요
- 커스텀 서비스 계정을 필요한 만큼 생성하고 임의의 액세스 범위 또는 IAM 역할을 할당하고 서비스 계정을 가상 머신 인스턴스에 할당할 수 있음

### 기본 제공 서비스 계정

- 모든 프로젝트에는 Compute Engine 기본 서비스 계정 제공
- project-number-compute@developer.gserviceaccount.com이라는 이메일로 식별
- 프로젝트에 대한 편집자 역할이 자동으로 부여
- 새 인스턴스를 시작하면 해당 인스턴스에서 기본 서비스 계정이 사용 설정됨
    - 다른 서비스 계정을 지정하거나 인스턴스의 서비스 계정을 사용 중지하여 이 동작을 재정의 가능

### Google API 서비스 계정

- 기본 서비스 계정 외에 모든 프로젝트에는 특정 이메일로 식별할 수 있는 Google Cloud API 서비스 계정 제공
- project-number@cloudservices.gserviceaccount.comd이라는 이메일로 식별
- 사용자 대신 내부 Google 프로세스를 실행하도록 특별히 설계된 계정
- 프로젝트에 대한 편집자 역할이 자동으로 부여됨

## Autorization (승인)

- 지정된 리소스 조합에 대해 인증된 ID 가 갖는 권한을 결정하는 프로세스
- Scope는 인증된 ID가 승인되었는지 여부를 결정하는 데 사용
- 예시
    
    ![iam](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/iam4.png)
    
    - 애플리케이션 A와 B에 인증된 ID나 서비스 계정이 포함, 두 애플리케이션 모두 Cloud Storage 버킷을 사용하려 한다고 가정
    - 각 애플리케이션은 Google 승인 서버에서 액세스를 요청하고 액세스 토큰을 받음
        - 애플리케이션 A는 읽기 전용 범위의 액세스 토큰을 수신하므로 Cloud Storage 버킷에서 읽기 작업만 가능
        - 애플리케이션 B는 읽기/쓰기 범위의 액세스 토큰을 수신하므로 Cloud Storage 버킷에서 데이터를 읽고 수정할 수 있음
- 기본 서비스 계정을 사용해 인스턴스를 만들 때 범위를 맞춤설정할 수 있음
- 범위는 인스턴스가 생성된 후에 인스턴스를 중지하여 변경할 수 있음
- Access scope는 VM에 권한을 지정하는 Legacy 방법
    - IAM 역할이 있기 전에는 액세스 범위가 서비스 계정에 권한을 부여하기 위한 유일한 방법이었음
    - 사용자 생성 서비스 계정의 경우 IAM 역할을 사용해 권한을 지정해라

## Service Account permissions

- 서비스 계정의 역할을 그룹이나 사용자에게 할당할 수도 있음
- 예시
    
    ![iam](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/iam5.png)
    
    - 먼저 InstanceAdmin 역할이 있는 서비스 계정을 생성 (가상 머신 인스턴스 및 디스크를 생성, 수정, 삭제할 권한이 있는 역할)
    - 그런 다음, 이 서비스 계정을 리소스로 취급하고 사용자 또는 그룹에 서비스 계정 사용자 역할을 제공하여 누가 이 계정을 사용할 수 있는지 결정
    - 이렇게 하면 해당 사용자는 주어진 서비스 계정 역할을 수행하여 가상 머신 인스턴스 및 디스크를 생성, 수정, 삭제 가능
- 서비스 계정 사용자 역할이 있는 사용자는 서비스 계정에서 액세스할 수 있는 모든 리소스에 액세스 가능
    - 따라서 사용자나 그룹에 서비스 계정 사용자 역할을 부여할 때는 주의
- 예시
    
    ![iam](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/iam6.png)
    
    - component_1을 실행하는 VM에는 서비스 계정 1을 사용하여 project_b에 액세스할 수 있는 편집자 액세스 권한이 부여됨
    - component_2를 실행하는 VM에는 격리된 서비스 계정 2를 사용하여 bucket_1에 대한 객체 뷰어 액세스 권한이 부여됨
    - 이렇게 하면 VM을 재생성하지 않고도 VM 권한 범위를 지정할 수 있음
- 기본적으로 Cloud IAM을 사용하면 각각의 마이크로 서비스를 나타내는 서비스 계정을 생성함으로써 상이한 리소스에 대한 액세스 권한을 갖는 서로 다른 마이크로 서비스로 프로젝트를 분할 가능
- VM이 생성될 때 서비스 계정을 VM에 할당하고 나면 사용자 인증 정보가 제대로 관리되는지 확인할 필요 없음
    - Google Cloud에서 보안을 관리하기 때문

### 서비스 계정이 인증되는 방식

- 기본적으로 Google Cloud 내에서 서비스 계정을 사용할 때 그러니까 Compute Engine이나 App Engine 내에서 사용할 때 Google은 서비스 계정의 키를 자동으로 관리
- Google Cloud 외부에서 서비스 계정을 사용하고 싶거나 다른 순환 주기를 원하는 경우 자체 서비스 계정 키를 수동으로 만들고 관리하는 것도 가능
- 모든 서비스 계정에는 Google이 관리하는 키 쌍이 있음, Google 관리형 서비스 계정 키를 사용할 경우 Google에서 키의 공개 부분과 비공개 부분을 모두 저장하고 정기적으로 순환시킴
    - 각 공개 키는 최대 2주 동안 서명에 사용될 수 있음
    - 비공개 키는 항상 에스크로에 안전하게 보관되며 직접 액세스할 수 없음
- 필요한 경우 Google Cloud 외부에서 사용할 수 있는 사용자 관리형 키 쌍, 즉 '외부 키'를 하나 이상 만들 수 있음
    - Google에서는 사용자 관리형 키의 공개 부분만 저장
        - 수동으로든, 프로그래매틱 방식으로든 비공개 키의 보안과 키 순환과 같은 기타 관리 작업을 수행할 책임은 사용자에게 있음
    - 사용자는 원활한 키 순환을 위해 서비스 계정당 최대 10개의 서비스 계정 키를 만들 수 있음
        - 사용자 관리형 키는 Cloud IAM API, gcloud 명령줄 도구 또는 Cloud Console의 서비스 계정 페이지를 통해 관리
        - Google은 사용자 관리형 비공개 키를 저장하지 않으므로 분실한 경우 Google에서 복구를 도와줄 수 없음
    - 사용자 관리형 키는 최후의 수단으로 사용
        - 토큰 등의 단기 서비스 계정 사용자 인증 정보나 서비스 계정 가장과 같은 다른 대안을 고려

# IAM best practice

## 리소스 계층 구조를 이해하고 활용

- 프로젝트르 사용해 동일한 신뢰 경계를 공유하는 리소스를 그룹화
- 각 리소스에 부여된 정책을 확인하고 상속이 무엇인지 이해해야 함
- 상속을 이유로 권한을 부여할 때는 **최소 권한의 원칙** 사용
- Cloud Audit Log를 사용해 정책을 감시하고 정책에 사용되는 그룹의 멤버십을 감시

## 개인이 아닌 그룹에 역할을 부여하는 것이 좋음

- 이렇게 하면 IAM 정책을 변경하는 대신 그룹 멤버십을 업데이트 할 수 있음
- 그룹에 역할을 부여하는 경우 정책에서 사용되는 그룹의 멤버십을 감사
- Cloud IAM 정책에 사용되는 Google 그룹의 소유권을 제어
- 그룹을 여러 개 사용하면 제어하기가 좀 더 수월해짐
- 예시
    
    ![iam](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/iam7.png)
    
    - 네트워크 관리자 그룹에는 Cloud Storage 버킷에 대한 읽기/쓰기 역할이 필요한 구성원도 있고 읽기 전용 역할만 필요한 구성원도 있음
    - 세 그룹에서 개인을 추가 및 제거하면서 전체 액세스 권한을 제어할 수 있음
    - 이처럼 그룹은 작업 역할과 관련될 뿐만 아니라 역할 할당을 위해서 존재할 수도 있음

## 서비스 계정에서의 권장 사항

- 서비스 계정 사용자 역할을 부여할 때는 매우 주의해야 함
    - 서비스 계정이 액세스 가능한 모든 리소스에 액세스를 제공하기 때문
- 서비스 계정을 만들 때는 목적을 명확하기 식별할 수 있는 표시 이름을 지정하는 것이 좋음
    - 가능한 한 설정된 네이밍 컨벤션을 사용
- 키의 경우 serviceAccount.keys.list() 메서드로 키 순환 정책 및 메서드를 설정하고 키를 감사

## Cloud Identity-Aware Proxy (Cloud IAP)를 사용하는 것이 좋음

![iam](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/assets/iam8.png)

- HTTPS로 액세스되는 애플리케이션에 대해 중앙 승인 레이어를 설정할 수 있으므로 네트워크 수준의 방화벽을 사용하는 대신 애플리케이션 수준의 액세스 제어 모델을 사용할 수 있음
- Cloud IAP로 보호되는 애플리케이션 및 리소스는 적합한 Cloud IAM 역할을 가진 사용자와 그룹만이 프록시를 통해 액세스 가능
- Cloud IAP를 통해 애플리케이션 또는 리소스 액세스 권한을 사용자에게 줄 경우 VPN 없이 사용 중인 제품으로 구현된 미세 조정된 액세스 제어가 적용
- 사용자가 Cloud IAP 보안 리소스에 액세스를 시도할 때 Cloud IAP는 인증 및 승인 검사 수행