# IAM (Identity and Access Management)

---

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

---

# Policy

## IAM policies inheritance

![IAMPoliciesInheritance.png](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/IAMPoliciesInheritance.png)

## IAM policies structure

![IAMPoliciesStructure.png](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/IAMPoliciesStructure.png)

# 보안 정책
## Password policy
## MFA (Multi Factor Authentication)

- AWS에서는 이 매커니즘을 필수적으로 사용하도록 권장됨
- 유저는 권한에 따라 구성을 변경하거나 리소스를 삭제하는 등의 작업 가능
- 적어도 루트 계정은 보안을 철저히 유지해야 하고 IAM 유저 또한 그러함
- MFA = password + 보안 장치

### MFA device options in AWS

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
	- AWS Management Console (protected by password + MFA)
	- AWS Command Line Interface (CLI) (Protected by access keys)
	- AWS Software Developer Kit (SDK) (for code: protected by access keys)
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
- Language-specific APIs (set of libraries)
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
	- EC2 instance roles
	- Lambda function roles
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
- 계정의 권한을 감시할 때는 security tools를 사용
- 절대 IAM user, Access key를 공유하지 말 것