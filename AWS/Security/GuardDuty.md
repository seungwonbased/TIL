# Amazon GuardDuty

![guard](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/guard1.png)

- GuardDuty를 사용하면 지능형 위협 탐지를 이용해서 AWS 계정을 보호할 수 있음
- GuardDuty에는 머신러닝 알고리즘이 있어서 이상 탐지를 수행
- 서드파티 데이터를 사용해서 위협을 탐지
- 이걸 활성화하려면 간단히 클릭 한 번만 하면 됨
	- 그럼 30일 무료체험
	- 별도로 소프트웨어를 설치할 필요는 없음
- 입력 데이터
	- CloudTrail Event Logs 같은 많은 입력 데이터를 확인해서 비정상적인 API 호출이나 무단 배포를 검색
		- 그리고 관리 이벤트와 데이터 이벤트를 검색
			- 관리 이벤트는 VPC 서브넷 생성 이벤트 등을 검색
		- S3 데이터 이벤트의 경우엔 GetObject, ListObject, DeleteObject 등을 검색
	- VPC Flow Logs의 경우엔 비정상적인 인터넷 트래픽을 검색
		- 비정상적인 IP 주소를 검색
	- DNS 로그의 경우엔 DNS 쿼리 안에서 인코딩된 데이터를 전송하는 EC2 인스턴스를 검색
		- 그건 인스턴스에 문제가 생겼다는 얘기
	- 옵션 기능이 있어서 EKS 감사 로그나 RDS 및 Aurora 로그인 이벤트, EBS, 람다, S3 데이터 이벤트 등 다른 입력 데이터 소스를 검색할 수 있음
- EventBridge 규칙을 설정해서, 발견된 결과가 있으면 자동으로 알림을 받을 수도 있음
	- 그 규칙은 AWS 람다나 SNS 토픽 같이 EventBridge가 타깃화할 수 있는 모든 걸 타깃화할 수 있음
- GuardDuty는 암호화폐 공격을 방어하기 위한 아주 좋은 도구
	- 전문적인 탐지 기능이 제공됨
	- 즉 그런 모든 입력 데이터를 분석하는 방법을 알고, 암호화폐 공격이 있는지 확인하게 됨
- 요약
	- GuardDuty에는 몇 가지 입력 데이터가 있음
		- VPC Flow Logs, CloudTrail Logs, DNS Logs 등 모든 게 GuardDuty로 들어가게 됨
		- S3 로그, EBS 볼륨, 람다 네트워크 활동, RDS 및 Aurora 로그인 활동, EKS 감사 로그나 런타임 모니터링 등 여러분이 활성화할 수 있는 몇 가지 옵션 기능도 있음
		- 시간이 지나면 더 많이 추가될 것임
	- 이 모든 것들로부터 GuardDuty는 탐지 결과를 생성할 수 있음
	- 탐지가 되면 Amazon EventBridge에서 이벤트가 생성됨
		- 그러므로 EventBridge로부터 규칙을 이용해서, 예를 들면 람다 함수라든지 SNS 알림 전송 같은 자동화를 트리거할 수 있어음
