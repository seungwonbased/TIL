# IAM (Identity and Access Management)

- AWS 서비스 및 리소스에 대한 액세스를 안전하게 제어할 수 있는 웹 서비스
- User, Group, Role, Policy로 구성
- 리전에 속하는 서비스가 아니고, 글로벌 서비스

## IAM 보안 모범 사례

> https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/best-practices.html

- 루트 계정은 최초 사용자 생성 이후 가능하면 사용하지 말 것
- 사용자 계정으로 서비스를 사용하고, 사용자에겐 필요한 최소한의 권한만 부여
	- 최소 권한의 원칙
- 루트 계정과 개별 사용자 계정에 강력한 암호 정책과 MFA 적용
	- MFA: 2 Factor 인증

## User

- AWS의 기능과 자원을 이용하는 객체
- 사람 또는 애플리케이션

### 사용자를 식별하는 방법

- 사용자 생성 시 지정한 이름
- 사용자의 고유 식별자
- ARN (Amazon Resource Name)
	- arn:aws:\service:region:account-id:resource 형식
		- arn: ARN을 나타내는 고정값
		- aws: AWS를 나타내는 고정값
		- service: 리소스를 제공하는 AWS 서비스 가변값
		- region: 리소스가 위치한 AWS 지역 가변값 (선택사항)
		- account-id: AWS의 계정 ID
		- resource: 서비스별로 리소스를 고유하게 식별하는 값

### 사용자의 자격 증명 방법

- AWS Management Console 암호
	- 사람이 AWS 서비스를 사용하기 위해 로그인할 때 사용
- **Access Key** = Access Key ID + Secret Access Key
	- 프로그램 방식으로 서비스를 사용할 때 씀
- SSH 키
	- AWS에 생성한 EC2 인스턴스로 SSH 접속할 때 사용
- 서버 인증서

## Group

- 여러 사용자에게 **공통으로 권한을 부여하기 위해** 만든 개념
- 여러 사용자에게 일관된 권한 정책을 쉽게 부여하는 것이 가능

## 권한 (Permission)

- AWS의 서비스나 자원에 어떤 작업을 수행힐 수 있는지 여부를 명시한 규칙

## 정책 (Policy)

- 자격 증명이나 리소스와 연결될 때  해당 권한을 정의하는 AWS의 객체
- 권한의 모음으로, **사용자, 사용자 그룹, 역할에 적용** 가능
- **사용자, 사용자 그룹, 역할에 권한을 직접 적용할 수 없고 정책을 생성한 후 적용해야 함**
- AWS는 IAM 보안 주체(사용자 또는 역할)가 요청을 보낼 때 정책을 평가
	- 정책에서 권한은 요청이 허용되거나 거부되는지 결정
	- 대부분의 정책은 AWS에 JSON 문서로 저장됨
	- 자격 증명 기반 정책, 리소스 기반 정책, 권한 경계, 서비스 제어 정책 (SCP), 접근 제어 목록 (ACL), 세션 정책이라는 6가지 정책 유형을 지원

### 자격 증명 기반 정책

- 관리형 정책 (Managed Policy)
	- AWS 관리형 정책
	- 고객 관리형 정책	
	- AWS 계정에 속한 다수의 사용자, 그룹 및 역할에 독립적으로 연결할 수 있는 자격 증명 기반 정책
	- 독립적으로 존재할 수 있음
	- 여러 자격 증명에서 공유 가능
- 인라인 정책 
	- 단일 사용자, 그룹 또는 역할에 직접 추가하는 정책
	- 정책과 자격 증명을 정확히 1:1 관계로 유지
		- 자격 증명을 삭제하면 정책 또한 삭제됨

### 리소스 기반 정책

- 요청이 적용되는 AWS 리소스 측에 연결하는 정책
- 지정된 보안 주체에 해당 리소스에 대한 특정 작업을 수행할 수 있는 권한을 부여하고 이러한 권한이 적용되는 조건을 정의
- 리소스 기반 정책은 인라인 정책이며, 관리형 리소스 기반 정책은 없음
- 모든 리소스가 지원하는 것은 아님

### 권한 경계

- 관리형 정책을 사용해 자격 증명 기반 정책이 IAM 엔티티(사용자 또는 역할)에 부여할 수 있는 최대 권한을 설정하기 위한 고급 기능
- 엔티티의 권한 경계 설정을 통해 자격 증명 기반 정책 및 관련 권한 경계 모드에서 허용되는 작업만 수행할 수 있음
- 사용자에 부여할 수 있는 최대 권한 정책을 설정하는 것

### 서비스 제어 정책 (SCP, Service Control Policies)

- 조직의 권한을 관리하는 데 사용할 수 있는 조직 정책 유형
- 조직의 모든 계정에 사용 가능한 최대 권한에 대한 중앙 제어를 제공

### 접근 제어 목록 (ACL, Access Control List)

- 리소스에 액세스할 수 있는 다른 계정의 보안 주체를 제어할 수 있는 서비스 정책
- JSON 정책 문서 형식을 사용하지 않는 유일한 정책 유형
- S3, WAF, VPC 등의 서비스에 적용

### 세션 정책

- 역할 또는 페더레이션 사용자에 대해 임시 세션을 프로그래밍 방식으로 생성할 때 파라미터로 전달하는 고급 정책
- AssumeRole, AssumeRoleWithSAML, AssumeRoleWithWebIdentity API 작업을 사용하여 프로그래밍 방식으로 역할 세션을 생성하고 세션 정책을 전달

### JSON 정책 문서

> https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/access_policies.html#access_policies-json

### 안전한 권한 부여 방법, 최소 권한 부여

- IAM 정책을 생성할 때는 최소 권한 부여의 표준 보안 조언을 따르거나 작업 수행에 필요한 최소한의 권한만 부여
- 사용자(역할)가 수행해야 하는 작업을 파악한 후 사용자들이 해당 작업만 수행하도록 사용자에 대한 정책을 작성

## 역할 (Role)

- 어떤 행위를 하는 객체에게 여러 정책을 적용한다는 점에서는 사용자와 비슷하지만, 객체가 사용자가 아닌 서비스나 다른 AWS 계정의 사용자라는 차이가 있음
- 정책을 적용하는 대상이 사용자(사람, 프로그램)가 아니고 AWS가 제공하는 다른 서비스나 다른 AWS 계정의 사용자