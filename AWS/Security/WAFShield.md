# AWS Web Application Firewall (WAF)

- AWS 웹 애플리케이션 방화벽(WAF)은 계층 7에서 일어나는 일반적인 웹 취약점 공격으로부터 웹 애플리케이션을 보호
	- 계층 7은 HTTP이니 HTTP 취약점 공격을 막아주는 것
	- 계층 4는 TCP/UDP 프로토콜
-  웹 애플리케이션 방화벽(WAF)의 배포는 애플리케이션 로드 밸런서, API Gateway, CloudFront, AppSync, GraphQL, API Cognito 사용자 풀에 할 수 있음
	- WAF의 배포가 어디에 되는 지 꼭 기억
	- 시험에서 속이기 위해 예를 들어 WAF를 NLB에 배포한다는 틀린 문장이 나올 수 있음
- 이러한 서비스에 방화벽을 배포한 후에는 웹 액세스 제어 목록(ACL)과 규칙을 정의해야 함
	- 이 규칙이란 IP 주소를 기반으로 필터링하는 등의 규칙
	- IP 세트를 정의할 수 있으며 각 IP 세트는 최대 10,000개의 IP 주소를 가질 수 있음
		- 더 많은 IP 주소가 필요하면 규칙을 여러 개 두면 됨
	- 또한 HTTP 헤더와 본문에 기반해 필터링할 수도 있고 URI 문자열을 조건으로 두어 SQL 주입, 크로스 사이트 스크립팅(XSS) 등의 일반적인 공격을 차단할 수도 있음
	- 용량에 제약을 걸어 요청이 최대 2MB를 넘지 않게 하거나 지역 일치(Geo-match) 조건을 두어 특정 국가를 허용 또는 차단할 수도 있음
	- 또 속도 기반 규칙을 설정하면 IP당 요청 수를 측정하여 디도스 공격을 막을 수도 있음
		- 특정 IP에서 초당 11개 이상의 요청을 보내지 못하게 한다든지
- 이러한 웹 ACL은 리전에만 적용되며 CloudFront는 글로벌로 정의됨
- 규칙 그룹
	- 이는 여러 웹 ACL에 추가할 수 있는 재사용 가능한 규칙 모음
	- 규칙을 정리하기 위한 것

## WAF - Fixed IP while Using WAF with a Load Balancer

![waf](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/waf1.png)

- WAF가 유용한 사용 사례
- 애플리케이션에 고정 IP를 사용하면서 로드 밸런서와 함께 WAF를 사용하고 싶다고 가정
- WAF는 네트워크 로드 밸런서(NLB)를 지원하지 않음
	- NLB는 L4에서 WAF는 L7에서만 작동
- 따라서 WAF를 제공하려면 애플리케이션 로드 밸런서가 있어야 함
- 하지만 애플리케이션 로드 밸런서는 고정 IP가 없음
	- 이런 문제는 AWS Global Accelerator로 고정 IP를 할당받은 다음 ALB에서 WAF를 활성화하면 해결할 수 있음
- 아키텍처
	- ALB와 EC2 인스턴스가 있는 리전이 하나 있음
	- 이때 Global Accelerator를 ALB 앞에 두고 애플리케이션에서 사용할 고정 IP를 얻음
	- 그리고 웹 애플리케이션 방화벽과 웹 ACL을 연결하는데, 역시 애플리케이션 로드 밸런서와 동일한 리전에 배치하면 원하는 아키텍처가 완성됨

# Shield
## AWS Shield - Protect from DDoS Attack

- AWS Shield는 디도스 공격으로부터 스스로를 보호하기 위한 서비스
- 디도스란 분산 서비스 거부 공격이라는 뜻
	- 인프라에 동시에 엄청난 양의 요청이 세계 곳곳의 여러 컴퓨터에서 유입되는 공격
	- 그 목적은 인프라에 과부하를 일으키는 것으로 실제 사용자들에게 서비스를 제공할 수 없게 만듬
	- 즉 분산 서비스 거부가 일어남
- 이런 디도스 공격을 방어하려면 **AWS Shield 스탠다드 서비스**를 이용하면 되는데, 이 서비스는 모든 AWS 고객에게 무료로 활성화되어 있는 서비스로 SYN/UDP Floods나 반사 공격 및 L3/L4 공격으로부터 고객을 보호해 줌
- 고급 보호가 필요한 고객을 위한 **AWS Shield 어드밴스드 서비스**도 있는데, 어드밴스드는 선택적으로 제공되는 디도스 완화 서비스로 조직당 월 3,000달러를 지불해야 함
	- 어드밴스드에서는 더욱 정교한 디도스 공격을 막아주며 Amazon EC2, ELB, Amazon CloudFront, AWS Global Accelerator 그리고 Route 53를 보호
	- 또한 AWS 디도스 대응 팀이 항시 대기하고 있어 공격을 받았을 때 지원을 받을 수 있음
	- 따라서 디도스 공격으로 인한 요금 상승을 AWS Shield 어드밴스드를 통해 방지할 수 있음
	- 또 Shield 어드밴스드는 자동 애플리케이션 계층 디도스 완화도 제공하여 자동으로 WAF 규칙을 생성, 평가, 배포함으로써 L7 공격을 완화할 수 있음
		- 즉 웹 애플리케이션 방화벽이 L7에서 일어나는 디도스 공격을 완화하는 규칙을 자동으로 갖게 된다는 뜻
