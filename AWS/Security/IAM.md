# IAM (Identity and Access Management)

- **AWS 리소스에 대한 액세스를 안전하게 제어할 수 있는 웹 서비스**
- 리소스에 대한 개별적인 접근 제어와 권한을 가지도록 계정 또는 그룹을 생성, 관리하는 서비스
- Global service → 지역 선택 필요 X
- 유저는 한 조직의 한 사람에 해당, 원한다면 그룹화 가능
	- 예
		- 개발자 그룹 → 3 유저, 오퍼레이터 그룹 → 2 유저, 개발자와 오퍼레이터 둘 다 속한 그룹 → 1 유저 등등
	- 루트 유저: 모든 권한
		- 위험하므로 따로 관리자 계정 생성 추천
	- AWS 허용을 위한 권한 부여 목적
		- 권한이 JSON 형식으로 정의됨

# Policy

## IAM Policies Inheritance

![IAMPoliciesInheritance.png](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/IAMPoliciesInheritance.png)

## IAM Policies Structure

![IAMPoliciesStructure.png](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/IAMPoliciesStructure.png)

# 보안 정책
## Password Policy
## MFA (Multi Factor Authentication)

- AWS에서는 이 매커니즘을 필수적으로 사용하도록 권장됨
- 유저는 권한에 따라 구성을 변경하거나 리소스를 삭제하는 등의 작업 가능
- 적어도 루트 계정은 보안을 철저히 유지해야 하고 IAM 유저 또한 그러함
- MFA = Password + 보안 장치

### MFA Device Options in AWS

- Virtual MFA device
	- 가상화 서비스
	- Google authenticator (phone only)
	- Authy (multi-device)
- U2F (Universal 2nd Factor) security key
	- 물리적 장치
	- YubiKey by Yubico (3rd party)
- Hardware Key Fob MFA device
	- 물리적 장치
	- Gemalto의 3rd party 장치
- Hardware Key Fob MFA device for AWS GovCloud
	- 미국 정부의 클라우드인 AWS CovCloud를 사용할 때 필요한 장치

# How can users access AWS?

- To access AWS, there are 3 options
	- AWS Management Console (Protected by password + MFA)
	- AWS Command Line Interface (CLI) (Protected by access keys)
	- AWS Software Developer Kit (SDK) (For code: Protected by access keys)
- Access keys는 AWS console을 통해 생성됨
- 유저들은 각자의 Access key를 manage
- Access keys는 비밀이므로, 절대 공유해서는 안 됨
	- Access key ID ⇒ username
	- Access key secret ⇒ password

## What’s the AWS CLI?

- 명령줄 쉘에서 AWS 서비스와 인터랙션 할 수 있는 도구
- AWS 서비스의 public API에 직접 접근
- 리소스 관리를 위해 스크립트를 개발 가능

## What’s the AWS SDK?

- AWS Software Development Kit (SDK)
- Language-specific APIs (Set of libraries)
- 프로그래밍을 통해 AWS 서비스에 접근하고 관리할 수 있는 도구
- 애플리케이션에 내장하는 방식
- Supports
	- SDKs (JavaScript, Python, Ruby, Java, Go, …)
	- Mobile SDKs (Android, iOS, …)
	- IoT Device SDKs (Embedded C, Arduino, …)

# AWS 서비스에 대한 IAM Role
## IAM Role

- **`계정`에 생성할 수 있는, 특정 권한을 지닌 IAM 자격 증명**
- AWS에서 자격 증명이 할 수 있는 것과 없는 것을 결정하는 권한 정책을 갖춘 AWS 자격 증명이라는 점에서 IAM Role은 IAM User와 유사(하지만 다름)
- IAM Role은 해당 권한이 필요한 사용자에게 얼마든지 부여 가능
- AWS 서비스에 대한 Role을 생성 가능
- 일반적인 Role
	- EC2 Instance Roles
	- Lambda Function Roles
	- Roles for CloudFromation

# IAM security tools
## IAM Credentials Report (account-level)

- 모든 유저와 자격 증명의 상태를 리스트로 나타낸 report

## IAM Access Advisor(user-level)

- 유저에게 부여된 서비스 권한과 그 서비스가 언제 마지막으로 엑세스 되었는지에 대한 정보

# IAM Guidelines & Best practice

- AWS 계정 설정을 할 때 빼고는 루트 계정을 사용하지 말 것
- 하나의 AWS 사용자 = 한 명의 실제 사용자
	- 만약 친구가 내 AWS를 사용하고 싶다면 계정을 (나의 자격 증명을) 주지 말고 새로운 유저를 생성해서 줄 것
- 사용자를 그룹에 넣어 해당 그룹에 권한을 부여 가능
	- 그룹 수준에서 권한을 관리 가능
- 비밀번호 정책을 강력하게 할 것
- MFA를 사용할 수 있다면 해커들로부터 계정을 지킬 수 있음
- AWS 서비스에 권한을 부여할 때마다 Role을 만들고 사용해야 함
	- 가상 서버인 EC2 인스턴스를 사용할 때도 마찬가지
- AWS를 프로그래밍할 경우, 즉 CLI나 SDK를 사용할 경우 반드시 액세스 키를 만들어야 함
- 계정의 권한을 감시할 때는 Security Tools를 사용
- 절대 IAM user, Access key를 공유하지 말 것

# AWS Organizations

![iam](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/iam1.png)

- AWS Organizations은 글로벌 서비스로 다수의 AWS 계정을 동시에 관리할 수 있게 해줌
- 조직을 생성하면 조직의 메인 계정이 관리 계정이 됨
- 조직에 가입한 기타 계정이나 조직에서 생성한 계정은 멤버 계정이라고 부르며 멤버 계정은 한 조직에만 소속됨
- Organizations의 멋진 점은 모든 계정의 비용을 통합 결제할 수 있다는 점
	- 따라서 관리 계정에 하나의 지불 방법만 설정해 두면 조직 전체의 비용을 지불할 수 있음
- Organizations를 사용하면 조직 내 모든 계정에 대해 집계된 사용량에 기반한 비용 할인을 받을 수 있음
	- 모든 계정에서 EC2 또는 Amazon S3를 많이 사용한다면 모든 계정의 사용량이 합쳐져서 계산되므로 큰 할인 혜택을 받을 수 있음
	- 또한 계정 간에 예약 인스턴스와 Savings Plans 할인이 공유됨
	- 어떤 계정에서 사용하지 않는 예약 인스턴스가 있을 때 다른 계정이 해당 인스턴스를 사용할 수 있음
	- 할인이 조직 전체에 걸쳐 적용되므로 비용 절감에 도움을 줄 수 있음
- Organizations 내 계정 생성을 자동화할 수 있는 API가 있어 Organizations를 사용하면 계정을 쉽게 생성할 수 있음

![iam](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/iam2.png)

- 작동 원리
- 루트 조직 단위(OU)라는 개념이 있음
	- 이것이 전체 계정에서 제일 외곽에 있음
- 이 안에 관리 계정이 있고, 서브 OU를 생성할 수 있음
	- 예를 들어 개발 계정 OU를 생성하고 그 안에 멤버 계정을 생성하거나 프로덕션 계정 OU를 생성하고 멤버 계정을 추가할 수 있음
	- 원하는 만큼 자유롭게 구성할 수 있음
	- 프로덕션 OU 안에 HR 멤버 계정 OU를 만들 수도 있고 재무 멤버 계정 OU를 만들 수도 있죠 원하는 대로 조직이 가능
- 다양한 OU 활용 예시가 있음
	- 비즈니스 단위에 따라 OU를 조직하는 경우
		- 관리 계정 아래에 세일즈, 리테일 및 재무 OU를 두고 OU별로 멤버 계정을 생성할 수 있음
	- 환경에 따라 OU를 구성할 수도 있음
		- 프로덕션 OU 테스트 OU, 개발 OU를 만들고 각 OU에 계정을 만드는 것
	- 프로젝트별 OU와 멤버 계정을 생성하는 프로젝트 기반 OU도 가능

## AWS Organizations - Advantages

- 장점
	- 다수의 계정을 가지므로 다수의 VPC를 가진 단일 계정에 비해 보안이 뛰어남
		- 계정이 VPC보다 독립적이기 때문
	- 또한 청구 목적의 태그 기준을 적용할 수 있고 한 번에 모든 계정에 대해 CloudTrail을 활성화해서 모든 로그를 중앙 S3 계정으로 전송할 수 있음
	- CloudWatch Logs를 중앙 로깅 계정으로 전송할 수도 있음
	- 자동으로 관리 목적의 계정 간 역할을 수립할 수 있어 관리 계정에서 모든 멤버 계정을 관리할 수 있음
- 보안 측면 장점
	- 서비스 제어 정책, 즉 SCP를 정의할 수 있음
		- 이는 특정 OU 또는 계정에 적용되는 IAM 정책으로 해당 사용자와 역할 모두가 계정 내에서 할 수 있는 일을 제한
		- SCP는 모든 대상에 적용되나 영구적인 관리자 권한을 갖는 관리 계정에는 적용되지 않음
			- 실수가 발생했을 때 누군가는 복구를 해야 하기 때문
		- SCP는 기본적으로 IAM처럼 아무것도 허용하지 않음
			- 구체적인 허용 항목을 설정해야 작동

### SCP Hierarchy

![iam](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/iam3.png)

- 예시를 통해 작동 방식을 분석
- 도식과 같은 조직이 있다고 가정
	- 루트 OU가 있고 관리 계정이 있음
	- 프로덕션 OU 안에 계정 A가 있음
	- 서브 OU인 HR OU, 재무 OU에는 각각 계정 B와 C가 들어있음
	- SCP가 각 계정의 권한에 미치는 영향
		- 일반적으로 루트 OU에는 FullAWSAccess(전체 AWS 액세스) SCP를 적용
			- 따라서 루트 OU 내의 계정은 모든 계정에 대한 전체 권한을 가짐
			- 관리 계정 프로덕션 계정의 계정 A, 계정 B, 계정 C에 대한 모든 권한을 가짐
		- 그리고 추가 SCP를 적용
	- 다른 방식도 있지만 가장 일반적인 방식
		- 관리 계정에는 DenyAccessAthena SCP를 적용
		- 프로덕션 OU에는 DenyRedshift SCP를 적용
		- 다른 SCP도 있음
- 각 계정의 관점에서 SCP들의 역할 분석
	- 관리 계정에 Athena 서비스 액세스를 제한하기 위해 DenyAccessAthena SCP를 적용하긴 했지만 관리 계정에는 SCP가 적용되지 않으므로 여전히 모든 것이 가능하고 모든 대상에 대해 관리자 권한을 가짐
	- 계정 A는 루트 OU에 있으므로 많은 일을 할 수 있음
		- 계정 A는 프로덕션 OU에 속하기도 함
		- 프로덕션 OU에는 DenyRedshift SCP가 추가되어 있음
		- 따라서 계정 A는 Redshift에 액세스할 수 없음
		- AuthorizedRedshift SCP가 계정 A에 추가되어 있지만 정책에 명시적 거부가 하나라도 포함되면 기본적으로 액세스가 거부됨
		- 따라서 계정 A는 Redshift 서비스 액세스 외의 모든 일을 할 수 있음
	- 계정 B는 프로덕션 OU 안에 있는 HR OU에 속하며 모든 것을 상속
		- 계정 B는 루트 OU로부터 FullAWSAccess SCP를 상속하고 프로덕션 OU로부터 DenyRedshift SCP를 HR OU로부터 DenyAWSLambda SCP를 상속
		- 따라서 Redshift 및 Lambda 액세스 외의 모든 일을 할 수 있음
	- 계정 C는 재무 OU에 속함
		- 재무 OU는 루트 OU의 프로덕션 OU에 속하므로 Redshift 액세스 외의 모든 일이 가능

### SCP Examples  Blocklist and Allowlist Strategies

![iam](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/iam4.png)

- SCP에는 '차단 목록'과 '허용 목록' 두 가지 전략이 있음
- 차단 목록은 계정이 사용하지 못하게 할 서비스를 지정
	- Allow, \*(별표)로 모든 서비스의 모든 작업을 허용한 다음 Deny 문을 추가해 DynamoDB에 대한 액세스를 거부하는 식
- 허용 목록은 모든 액션을 허용하지 않고 가령 EC2와 CloudWatch만 허용하는 방식
	- 해당 SCP가 적용된 계정에서는 EC2와 CloudWatch만이 허용되며 다른 서비스는 사용할 수 없고 사용하려면 명시적 허용이 필요

# IAM Conditions

![iam](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/iam5.png)

- IAM 조건은 IAM 내부의 정책에 적용되며 사용자 정책일 수도 있고 예를 들면 S3 버킷에 대한 리소스 정책일 수도 있음
	- 엔드포인트 정책 등 어떤 정책도 될 수 있음
- aws:SourceIp
	- API 호출이 생성되는 클라이언트 IP를 제한하는 데 사용되는 조건
	- 정책을 보면 모든 작업과 리소스를 거부(Deny)하지만 조건이 있음
		- 두 개의 IP 주소 범위에 포함되지 않는 IP 주소는 거부하는 것
		- 이는 클라이언트가 해당 IP 주소 내에서 API 호출을 하지 않으면 API 호출이 거부된다는 뜻
		- 예를 들어 회사 네트워크에서만 AWS를 사용하도록 제한하여 회사 내에서만 AWS 환경에 액세스할 수 있게 설정할 수 있음
- aws:RequestedRegion
	- AWS로 시작하므로 글로벌 조건이며 API 호출 리전을 제한
	- 예시에서는 eu-central-1와 eu-west-1 리전에서 오는 EC2, RDS, DynamoDB 호출을 제한
		- 특정 리전에서 특정 서비스에 대한 액세스를 거부
	- 조건을 조직 SCP에 보다 글로벌하게 적용해서 특정 리전의 액세스를 허용하거나 거부할 수 있음

![iam](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/iam6.png)

- ec2:ResourceTag
	- 태그의 접두사가 EC2
	- 따라서 이 조건은 EC2 인스턴스의 태그에 적용됨
	- 여기서는 ResourceTag/Project가 DataAnalytics일 때 모든 인스턴스의 시작과 종료를 허용
		- EC2 인스턴스가 올바른 태그를 갖고 있으면, 즉 Project가 DataAnalytics이면 허용됨
- aws:PrincipleTag
	- 사용자 태그에 적용됨
	- EC2 인스턴스 태그가 아닌 사용자 태그
	- 상기 작업을 수행하려면 사용자의 Department가 Data여야 함
- aws:MultiFactorAuthPresent
	- 멀티팩터 인증을 강제
	- 정책을 보면 사용자는 EC2에서 모든 작업을 할 수 있지만 멀티팩터 인증이 있어야만 인스턴스를 중지하고 종료할 수 있음
	- MFA가 false(거짓)일 때 거부됨

## IAM for S3

![iam](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/iam7.png)

- S3 버킷에 대한 IAM 정책
- 두 개의 선언문이 있음
- s3:ListBucket은 s3:::test라는 ARN에 적용됨
	- 버킷 수준의 권한이므로 버킷을 특정해야 함
- s3:GetObject, PutObject DeleteObject는 버킷 내의 객체에 적용되므로 ARN이 달라짐
	- 버킷 내의 모든 객체를 나타내는 /\*이 들어감
	- 객체 수준 권한이기 때문에 ARN이 달라진 것임
	- 시험에 나올 만한 내용

## Resource Policies & aws:PrincipalOrgID

![iam](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/iam8.png)

- aws:PrincipalOrgID와 리소스 정책
- aws:PrincipalOrgID라는 조건은 AWS 조직의 멤버 계정에만 리소스 정책이 적용되도록 제한함
- 다수의 계정이 포함된 조직이 있고 다음과 같은 정책이 적용된 S3 버킷이 있음
- 이에 따르면 PrincipleOrgID의 계정에서 API 호출이 생성된 경우에만 PutObject, GetObject 작업을 할 수 있음
- 조직 내의 멤버 계정만 S3 버킷에 액세스할 수 있고 조직 외부의 사용자는 이 S3 버킷 정책에 의해 액세스가 거부됨

# IAM Roles vs Resource Based Policies

![iam](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/iam9.png)

- IAM 역할과 리소스 기반 정책의 근본적인 차이점
- 교차 계정의 경우, 예를 들면 S3 버킷 교차 계정에서 API 호출을 수행하려면 두 가지 방법이 있음
	- S3 버킷에 S3 정책을 적용하는 것처럼 리소스에 리소스 기반 정책을 추가할 수도 있음
	- 리소스에 액세스할 수 있는 역할을 사용할 수도 있음
- 두 방법의 예시 분석
	- 이 예시에서는 계정 A의 사용자가 Amazon S3 버킷에 대한 액세스 권한이 있는 계정 B의 역할을 사용
	- 아니면 계정 A의 사용자가 계정 B의 Amazon S3 버킷에 적용된 버킷 정책을 통해 S3 버킷에 액세스할 수도 있음
	- 두 방법 모두 유효하지만 약간의 차이가 있음
	- 첫 번째는 IAM 역할이 S3 버킷에 액세스했고 두 번째는 S3 버킷 정책이 사용자의 액세스를 허용함
- 역할을 맡으면 기존의 권한을 모두 포기하고 해당 역할에 할당된 권한을 상속하게 됨
	- 매우 중요
	- 예를 들어 어떤 작업을 수행하는 역할을 맡게 되면 해당 역할에 부여된 작업은 할 수 있지만 기존의 권한은 사용할 수 없음
- 대신 리소스 기반 정책을 사용하면 본인이 역할을 맡지 않으므로 권한을 포기할 필요가 없음
	- 예를 들어 계정 A의 사용자가 본인 계정의 DynamoDB 테이블을 스캔해 계정 B의 S3 버킷에 넣을 경우에는 리소스 기반 정책을 사용하는 것이 좋음
	- 역할을 맡을 필요가 없으므로 DynamoDB 테이블을 스캔하고 다른 계정의 S3 버킷에 쓰기 작업도 할 수 있기 때문
	- 리소스 기반 정책을 지원하는 AWS 서비스와 리소스가 점점 늘어나고 있음
	- Amazon S3 버킷 SNS 주제, SQS 대기열, Lambda 함수 등에서 지원됨

## Amazon EventBridge - Security

- 이 차이는 Amazon EventBridge에서 사용할 때 가장 크게 드러남
- Amazon EventBridge에는 원하는 작업을 하려면 대상에 대한 권한을 요구하는 규칙이 있음
- 두 가지 유형의 대상이 있음
	- 리소스 기반 정책을 지원하는 대상에는 Lambda, SNS, SQS, CloudWatch Logs API Gateway 등이 있음
		- 이 경우 대상 리소스를 변경해야 함
		- 리소스 기반 규칙을 적용해 EventBridge가 원하는 작업을 수행할 수 있도록 허용해야 함
	- 두 번째 유형은 Kinesis Data Streams, Systems Manager Run Command나 ECS 태스크를 시작할 때는 IAM 역할이 필요
		- IAM 역할을 EventBridge 규칙에 추가하면 Kinesis에 쓰기 작업을 수행할 권한을 갖게 됨
- 정리
	- SNS, SQS, Lambda에는 리소스 기반 정책을 사용
	- Kinesis Data Streams에는 IAM 역할을 사용

# IAM Permission Boundaries

![iam](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/iam10.png)

- IAM 권한 경계
- 권한 경계(Permissions boundary)는 사용자와 역할만 지원하고 그룹은 지원하지 않음
- IAM 개체의 최대 권한을 정의하는 고급 기능
- IAM 권한 경계 예시
	- IAM 정책과 비슷해 보임
	- S3, CloudWatch, EC2를 모두 허용하고 있음
	- 이걸 IAM 사용자에 연결하면 권한 경계를 설정하는 것임
		- S3, CloudWatch, EC2 내의 작업만 할 수 있게 됨
	- 더불어 IAM 정책을 통해 IAM 권한을 지정해야 함
		- iam:CreateUser 액션과 * 리소스를 같은 사용자에 연결하면 권한 경계와 IAM 정책을 통한 권한이 완성됨
	- 결과적으로 아무 권한도 생기지 않음
		- IAM 정책은 IAM 권한 경계 밖에 있기 때문에 사용자가 다른 IAM 사용자를 생성할 수 없음
		- IAM 권한 경계에 포함되지 않았기 때문

![iam](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/iam11.png)

- IAM 권한 경계는 AWS Organizations SCP와 함께 사용될 수 있음
- 도식을 보면 유효(effective) 권한이 사용자나 그룹에 부여된 자격 증명 기반 정책과 그룹이 아닌 사용자나 역할에만 적용되는 권한 경계와 계정상 모든 IAM 개체에 적용되는 Organizations SCP 중앙에 있음
- 사용자에게 주어지는 권한은 교집합 부분
- 다음과 같은 사용 사례가 있음
	- 관리자가 아닌 사용자에 책임을 위임하기 위해 권한 경계 내에서 새 IAM 사용자를 생성하거나 개발자가 권한을 스스로 부여하고 관리하는데 특권을 남용하는 것을 막기 위해 스스로를 관리자로 만들지 못하게 할 수 있음
	- 또 계정에 SCP를 적용해서 계정의 모든 사용자를 제한하지 않고 AWS Organizations의 특정 사용자만 제한할 수도 있음

## IAM Policy Evaluation Logic

![iam](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/iam12.png)

- IAM 정책 평가 논리
- 이 도표를 보면 AWS 내에서 액션에 대한 권한 부여 방법을 알 수 있음
- 외울 필요는 없지만 이해하실 수 있으면 됨
- 전체 흐름 중에 각 단계마다 평가가 이루어짐
- 평가 거부
	- 가능한 모든 정책을 살펴보고 명시적인 거부가 있다면 자동으로 거부됨
- Organizations SCP
	- 허용된다고 평가되면 다음 단계로 넘어가고 그렇지 않다면 암시적 거부로 거부됨
- 리소스 기반 정책
	- S3 버킷에 적용되거나 SQS 대기열에 적용됐는지 평가
	- 리소스 기반 정책이 있는지 있다면 허용 여부에 따라 나뉨
- 자격 증명 기반 정책
	- 자격 증명 정책이 있는지 없는지 허용이 됐는지 아닌지에 따라 암시적으로 거부되거나 다음 단계로 넘어감
- IAM 권한 경계
- 세션 정책인데 STS 관련 내용이라 생략하
- 특정 IAM 액션을 할 때마다 모든 방면에서 평가된다는 걸 기억
	- 모든 방면에서 거부되지 않고 허용됐을 때만 마지막 허용 여부가 결정되고 액션을 진행할 수 있음

## Example IAM Policy

![iam](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/iam13.png)

- 액션에는 sqs:\*, 효과는 Deny 그리고 리소스는 \*
- 아래는 sqs:DeleteQueue에 Allow와 리소스 \*이 있음
- 첫 번째 질문: sqs:CreateQueue를 실행할 수 있는지?
	- 정답: 생성할 수 없음
		- sqs:\*에 Deny와 \*가 있는데, CreateQueue가 해당 블록에 포함되기 때문에 거부됨
- 그럼 sqs:DeleteQueue는 실행할 수 있는지?
	- 위쪽에 Deny가 있고 아래에 Allow가 있음
		- Deny와 Allow가 동시에 있음
	- 하지만 조금 전 설명했듯이 명시적 거부가 있으면 바로 결정이 거부됨
		- sqs:\*에 Deny가 명시되어 있기 때문에 sqs:\* 안에 있는 sqs:DeleteQueue는 Allow가 있음에도 자동으로 거부됨
		- 두 번째 블록에 Allow가 명시되어 있어도 sqs:DeleteQueue도 실행할 수 없다는 걸 기억
- 그럼 ec2:DescribeInstances는 실행할 수 있을지?
	- 보다시피 IAM 정책에 EC2 관련 사항은 없음
	- 명시적 거부도 없지만 명시적 허용도 없기 때문에 해당 IAM 정책 내에서는 ec2:DescribeInstances를 실행할 수 없음

# AWS IAM Identity Center (Successor to AWS Single Sign-On)

- AWS IAM 자격 증명 센터는 전에 AWS 싱글 사인온 서비스라고 알고 계시던 것의 후속 제품
	- 이름만 변경된 동일한 서비스, 한 번만 로그인하면 되니까 싱글 사인온이라고 하는 것
	- AWS 조직이나 비즈니스 클라우드 애플리케이션의 모든 AWS 계정에서 싱글 사인온을 할 수 있음
	- 즉, Salesforce, Box, Microsoft 365 등에 연결할 수 있음
	- 또는 SAML2.0 통합이 가능한 어떤 애플리케이션에라도 연결할 수 있음
	- 마지막으로 EC2 Windows 인스턴스에 대해서도 싱글 로그인을 제공
	- 한 번만 로그인하면 모든 것에 액세스할 수 있다는 건 정말 정말 좋은 것
	- 시험에서는 아마 여러 AWS 계정을 한 번에 로그인하는 것에 대해 나올 것임
- ID 공급자
	- 이 로그인을 위해 사용자는 두 위치에 저장될 수 있음
		- IAM 자격 증명 센터에 내장된 ID 저장소나
		- 서드파티 ID 공급자에 연결할 수 있음
			- Active Directory(AD)이거나, OneLogin, Okta 등일 수 있음

## AWS IAM Identity Center - Login Flow

![iam](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/iam14.png)

- 이런 로그인 흐름이 있다고 가정
- 로그인 페이지로 이동하여 사용자 이름과 비밀번호를 입력하고 바로 AWS IAM 자격 증명 센터로 이동
- 보다시피 계정에 대해 활성화해뒀기 때문에 원하는 계정을 클릭하여 원하는 곳으로, 예를 들어 관리 콘솔로 바로 연결할 수 있음
- 해당 콘솔에 로그인되어 있고, 그 콘솔에 로그인하는 방법을 알 필요가 없음
- 계정, 비즈니스 애플리케이션 등에 액세스할 수 있음
- 현실에서 여러 개의 AWS 계정을 가지고 있다면 이 서비스를 사용하는 것을 100% 추천

## AWS IAM Identity Center 작동 방법

![iam](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/iam15.png)

- 브라우저 인터페이스는 AWS IAM 자격 증명 센터의 로그인 페이지에 연결됨
- 그리고 그걸 다른 사용자 스토어와 통합해야 함
- 클라우드 또는 온프레미스에 있는 Active Directory도 가능
- 여기에서 Active Directory를 사용하여 사용자와 그룹을 관리할 수 있음
- 또는 IAM 자격 증명 센터를 사용할 수도 있음
- 빌트인 자격 증명 스토어에서 사용자나 그룹을 정의할 수 있음
- 그리고 ID 센터를 AWS 조직이나 Windows EC2 인스턴스, 비즈니스 클라우드 애플리케이션 사용자 정의 SAML2.0 지원 애플리케이션 등과 통합
- 이 모든 것에 한 번만 로그인해도 된다는 게 정말 좋고, 흐름이 많이 단순화됨
	- 물론 로그인했다고 해서 모든 항목에 액세스할 수 있는 건 아님
	- 자격 증명 센터에서 권한 셋을 정의해서 어떤 사용자가 무엇에 액세스할 수 있는지 정의

![iam](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/iam16.png)

- IAM 자격 증명 센터에서는 권한, 사용자, 그룹의 연관
- 조직 하나 있다고 가정
- 관리 계정에 IAM 자격 증명 센터를 설정
- 두 개의 OU가 있음
	- 개발 OU와 프로덕션 OU가 있고, 각각의 계정이 있음
- 회사에는 두 명의 개발자가 있다고 가정
	- Bob과 Alice
- 먼저 IAM에서 했던 것처럼 Bob과 Alice에 대해 개발자 그룹을 만듬
- Bob과 Alice가 개발 OU에 대해 전체 액세스 권한을 갖도록 하려고 함
- 따라서 권한 셋이라는 것을 만들고 거기에 관리자 액세스를 허용
- 그리고 이 권한 셋을 특정 OU와 연결해야 함
	- 즉, 개발 OU에 있는 사람과 연결
- 그리고 이 권한 셋을 개발자 그룹에 할당
- 그러면 Bob과 Alice는 전체 액세스를 허용하는 모든 개발 계정에서 역할을 맡을 수 있음
- 마찬가지로 프로덕션 OU에 대해 읽기 전용 액세스이라는 다른 권한 셋을 만들 수 있음
- 그것과 연결하고 다시 권한을 개발자 그룹에 할당
- 이런 식으로 사용자를 그룹, 권한 셋, IAM 자격 증명 센터의 특정 계정 할당에 연결할 수 있음

## AWS IAM Identity Center Fine-grained Permissions and Assignments

![iam](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/iam17.png)

- 세분화된 권한 및 할당
- 다중 계정 권한
	- 이 서비스를 사용하면 여러분의 조직에서 여러 계정에 대한 액세스를 관리할 수 있음
	- 또한 권한 셋을 사용하여 사용자를 그룹에 할당하는 하나 이상의 IAM 정책을 정의
		- AWS에서 사용자가 무엇에 액세스할 수 있는지 정의하기 위해서
	- 여기 IAM 자격 증명 센터는 조직과 통합되어 있음
	- 예를 들면 데이터베이스 관리자를 위한 권한 셋을 정의할 수 있음
		- 즉, IAM 정책 모음임
	- 예를 들어 Dev 계정의 RDS 또는 Aurora나 Prod 계정의 RDS 또는 Aurora에 액세스할 수 있다고 가정
		- 그러면 사융자에 대해 IAM 역할이 자동으로 생성됨
		- 따라서 데이터베이스 관리자가 예를 들어 그룹이라면, 즉 사용자가 그룹에 있다면 거기에 데이터베이스 관리자의 권한 셋을 할당하면, 사용자가 IAM 자격 증명 센터를 통해 로그인해서 Dev 계정 또는 Prod 계정의 콘솔에 액세스할 때 해당 계정에서 IAM 역할을 자동으로 위임
		- 그리고 받아야 하는 권한을 줌
- 애플리케이션 할당
	- 동일한 방식으로 작동
	- 어떤 사용자 또는 그룹이 어떤 애플리케이션에 액세스할 수 있는지 정의할 수 있음
	- 그러면 필요한 URL, 인증서, 메타데이터가 제공됨
	- 이 모든 것이 기본적으로 지원됨
- 속성 기반 액세스 제어
	- 즉, IAM 자격 증명 센터 스토어에 저장된 사용자 속성을 기반으로 세분화된 권한을 갖게 되는 것
	- 태그 같은 걸 갖게 되는 것
	- 이를 사용하여 사용자를 비용 센터에 할당하거나, 주니어나 시니어와 같은 직함을 주거나 로케일을 줘서 특정 리전에만 액세스하도록 할 수 있음
	- 사용 사례는 IAM 권한 셋을 한 번만 정의하고 이 속성을 활용하여 사용자 또는 그룹의 AWS 액세스만 수정하는 것
