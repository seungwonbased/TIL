# AWS Directory
## What's Microsoft Active Directory?

![mad](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/mad1.png)

- Microsoft Active Directory는 AD 도메인 서비스를 사용하는 모든 Windows 서버에 사용되는 소프트웨어
- 객체의 데이터베이스이며 사용자 계정, 컴퓨터, 프린터, 파일 공유, 보안 그룹이 객체가 될 수 있음
- 전체 Microsoft 생태계에서 관리되는 온프레미스의 모든 사용자는 Microsoft Active Directory의 관리 대상이 됨
- 또한 중앙 집중식 보안 관리로 계정 생성, 권한 할당 등의 작업이 가능
- 모든 객체는 트리로 구성되며 트리의 그룹을 포리스트라고 함
	- AD 용어니까 알아 둬야 함
- 예시
	- 도메인 컨트롤러에 계정을 생성
	- 사용자 이름은 John 비밀번호는 Password
	- 다른 모든 Windows 머신은 동일한 네트워크에 있고 도메인 컨트롤러에 연결됨
	- 첫 번째 머신에서 John과 Password를 사용하면 이 머신은 로그인 정보가 있는지 컨트롤러를 확인한 다음 로그인을 허용
	- 모든 머신은 도메인 컨트롤러에 연결되어 있으므로 사용자는 어떤 단일 머신에서도 액세스할 수 있음

## AWS Directory Service

![mad](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/mad2.png)

- AWS Directory 서비스
- AWS에 액티브 디렉터리를 생성하는 서비스로 세 가지 서비스가 있음
- 깊게 살펴보지는 않겠지만 차이점은 분명히 이해해야 함
- AWS 관리형 Microsoft AD
	- AWS에 자체 액티브 디렉터리를 생성하고 로컬에서 관리할 수 있으며 멀티팩터 인증을 지원
	- 독립 실행형 Active Directory로 사용자가 있는 온프레미스 AD와 신뢰 관계를 구축할 수 있음
	- 도식으로 설명하자면 AWS 관리형 AD가 온프레미스 AD와 상호 신뢰 관계를 구축하게 되는 것
	- AWS 관리형 AD의 인증된 사용자가 AWS 관리형이 아닌 계정을 사용할 때 온프레미스 AD에서 계정을 검색할 수 있고, 반대로 온프레미스 AD 사용자가 AWS 계정을 사용해 온프레미스 AD에서 인증하려 할 때도 신뢰 관계에 의거해 AWS AD에서 검색할 수 있음
	- 온프레미스와 AWS 액티브 디렉터리 간에 사용자가 공유되는 셈
- AD 커넥터
	- 디렉터리 게이트웨이, 다시 말해 프록시로 온프레미스 AD에 리다이렉트하며 MFA, 즉 멀티팩터 인증을 지원
	- 사용자는 온프레미스 AD에서만 관리됨
	- 예시
		- AD 커넥터는 프록시로 기능하므로 사용자가 AD 커넥터를 사용해 인증하려고 하면 온프레미스 AD에 요청을 프록시하고 찾아볼 뿐
		- 첫 번째 유형인 AWS 관리형 Microsoft AD에서는 AWS 관리형 AD와 온프레미스 AD 모두에 사용자가 있었는데 AD 커넥터는 이름처럼 연결하는 역할
		- 온프레미스 AD에 쿼리와 연결 요청을 프록시하므로 온프레미스 AD에서만 사용자를 관리할 수 있음
- Simple AD
	- AWS의 AD 호환 관리형 디렉터리로 Microsoft 디렉터리를 사용하지 않으며 온프레미스 AD와도 결합되지 않음
	- 온프레미스 AD가 없으나 AWS 클라우드에 액티브 디렉터리가 필요한 경우 독립형인 Simple AD 서비스를 사용
	- 액티브 디렉터리를 사용해 Windows를 실행할 EC2 인스턴스를 생성하고 이 Windows 인스턴스는 네트워크용 도메인 컨트롤러와 결합되어 모든 로그인 정보와 자격 증명 등을 공유
	- 이런 이유로 AWS에 디렉터리를 둠
		- Windows를 실행하는 EC2 인스턴스와 디렉터리를 가까이 위치시키려는 목적
- 정리
	- 온프레미스에서 사용자를 프록시한다면 AD 커넥터가 필요
	- AWS 클라우드에서 사용자를 관리하고 MFA를 사용해야 할 때는 AWS 관리형 AD가 필요
	- 온프레미스가 없을 때는 Simple AD를 선택

## IAM Identity Center - Active Directory Setup

![mad](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/mad3.png)

- IAM Identity Center와 액티브 디렉터리 통합 방법
- Directory 서비스를 통해 AWS 관리형 AD 연결할 경우 통합 방법은 간단
	- IAM Identity Center를 AWS 관리형 Microsoft AD에 통합하고 연결하도록 설정하면 됨
	- 
- 액티브 디렉터리를 클라우드에서 관리하지 않고 온프레미스에 자체 관리형 디렉터리가 있는 경우
	- 자체 관리형 디렉터리에 연결하는 방법은 두 가지
		- 첫 번째 방법은 AWS 관리형 Microsoft AD를 사용해 양방향 신뢰 관계를 구축하는 것
			- Directory 서비스를 이용하여 관리형 Microsoft AD를 생성하고 온프레미스에 있는 AD와 관리형 AD 간에 양방향 신뢰 관계를 구축한 다음 IAM Identity Center에서 싱글 사인온으로 간단히 통합하면 됨
		- 다른 방법은 AD 커넥터를 활용하는 것
			- AD 커넥터는 IAM Identity Center와 연결하는 역할을 하고 연결한 다음에는 자동으로 모든 요청을 자체 디렉터리로 프록시
		- 두 방법 중에 선택하면 됨
			- 클라우드에 있는 액티브 디렉터리에서 사용자를 관리하고 싶다면 첫 번째 솔루션이 나음
			- API 호출만 프록시하려면 두 번째 솔루션이 적합
				- 지연 시간이 길어지긴 함
