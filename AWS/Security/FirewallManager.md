# AWS Firewall Manager

- AWS Firewall Manager는 AWS Organizations에 있는 모든 계정의 방화벽 규칙을 관리하는 서비스
- 여러 계정의 규칙을 동시에 관리할 수 있음
- 보안 규칙의 집합인 보안 정책을 설정할 수 있는데 여기에는 ALB, API Gateway CloudFront 등에 적용되는 웹 애플리케이션 방화벽(WAF) 규칙이나 ALB, CLB, NLB, 엘라스틱 IP, CloudFront를 위한 AWS Shield 어드밴스드 규칙 등이 포함됨
	- EC2, 애플리케이션 로드 밸런서 VPC의 ENI 리소스를 위한 보안 그룹을 표준화하는 보안 정책과 VPC 수준의 AWS Network Firewall도 해당됨
	- 그리고 Amazon Route 53 Resolver DNS Firewall도 포함
- AWS Firewall Manager는 이와 같은 모든 방화벽을 한 곳에서 관리할 수 있도록 지원
- 정책은 리전 수준에서 생성되며 조직에 등록된 모든 계정에 적용됨
- 예를 들어 조직에서 애플리케이션 로드 밸런서에 대한 WAF 규칙을 생성한 다음 새 애플리케이션 로드 밸런서를 생성하는 경우, AWS Firewall Manager에서 자동으로 새 ALB에도 같은 규칙을 적용해 줌
	- 이것이 Firewall Manager의 장점

# WAF vs Firewall Manager vs Shield

- WAF, Firewall Manager, Shield에는 어떤 차이가 있는지?
- WAF, Shield, Firewall Manager는 모두 포괄적인 계정 보호를 위한 서비스
- WAF
	- 웹 ACL 규칙을 정의
	- 리소스별 보호를 구성하는 데에는 WAF가 적절
- Firewall Manager 
	- 하지만 여러 계정에서 WAF를 사용하고 WAF 구성을 가속하고 새 리소스 보호를 자동화하려면 Firewall Manager로 WAF 규칙을 관리하면 됨
	- Firewall Manager는 이러한 규칙들을 모든 계정과 모든 리소스에 자동으로 적용
	- Firewall Manager는 모든 계정에 Shield 어드밴스드를 배포하는 데도 도움
- Shield
	- Shield 어드밴스드는 디도스 공격으로부터 고객을 보호하고 WAF의 기능 외에도 더 많은 기능을 제공
	- 예를 들어 Shield 대응 팀 지원 고급 보고서 제공, 그리고 WAF 규칙 자동 생성 등의 기능을 추가로 이용할 수 있음
	- 디도스 공격을 자주 받는다면 Shield 어드밴스드를 사용하는 것도 좋은 선택지가 될 수 있음
