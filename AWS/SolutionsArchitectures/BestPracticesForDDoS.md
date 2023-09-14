# AWS Best Practices for DDoS Resiliency
## Edge Location Mitigation (BP1, BP3)

![ddos](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/ddos1.png)

- DDoS 보호와 모범 사례
- 예시: 사진에 보이는 것과 같은 아키텍처
- BP1: CloudFront
	- CloudFront는 엣지 로케이션에서 사용
		- 웹 애플리케이션 전송도 엣지에서 일어난다는 뜻
	- SYN Flood나 UDP 반사 공격과 같은 DDoS 일반 공격은 Shield 설정으로 막을 수 있음
- BP2: Global Accelerator
	- Global Accelerator를 사용하면 전 세계에서 엣지를 통해 여러분의 애플리케이션에 액세스할 수 있음
	- Global Accelerator는 Shield와 완전히 통합되어 CloudFront가 백엔드와 호환되지 않는 경우 DDoS 공격 방어에 유용하게 쓰임
	- 이 경우 Global Accelerator를 앞에 두는데 이렇게 하면 어떤 백엔드를 사용하든 CloudFront나 Global Accelerator로 AWS 엣지에 완전 분산이 가능하며 엣지 로케이션을 DDoS 공격으로부터 보호할 수 있음
- BP3: Route 53
	- Route 53를 사용하는 경우 엣지에 도메인 이름 변환을 글로벌로 설정
	- 이렇게 하면 DNS에도 DDoS 보호 메커니즘을 적용할 수 있음
	- 따라서 엣지에 대한 DDoS 보호를 더 확실히 할 수 있음

## Best Practices for DDoS Mitigation

![ddos](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/ddos1.png)

- DDoS 완화 모범 사례
- 인프라 계층 방어 (BP1, BP3, BP6)
	- CloudFront, Global Accelerator Route 53, 엘라스틱 로드 밸런싱은 높은 트래픽으로부터 Amazon EC2 인스턴스를 보호
	- 이 서비스들을 사용하면 EC2 인스턴스에 도달하기도 전에 트래픽을 관리할 수 있음
- EC2 인스턴스에서 오토 스케일링 기능을 활성화 (BP7)
	- 오토 스케일링 그룹에 트래픽이 도달한다고 해도 자동으로 확장하여 애플리케이션에서 더 큰 로드를 수용할 수 있음
- 엘라스틱 로드 밸런싱을 사용 (BP6)
	- ELB가 여러 EC2 인스턴스 간 트래픽을 자동으로 분산
	- 이렇게 하면 EC2 인스턴스에 관리 가능한 양의 트래픽이 들어오면서 오토 스케일링 그룹이 이에 따라 확장하는 데에도 무리가 없음

## Application Layer Defense

![ddos](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/ddos1.png)

- 애플리케이션 계층 방어
- Detect and filter malicious web requests (BP1, BP2)
	- BP1과 BP2로 악성 요청을 감지 및 필터링하는 방식
	- CloudFront는 정적 콘텐츠 전송 시 엣지 로케이션에서 전송함으로써 백엔드를 보호
	- 애플리케이션 로드 밸런서나 CloudFront에 WAF를 사용하여 요청 서명에 따라 요청을 필터링 및 차단할 수 있음
		- 특정 IP나 특정 요청 유형만 차단할 수도 있음
	- WAF 속도 기반 규칙을 사용하면 악성 사용자의 IP를 자동으로 차단할 수 있음
		- WAF에 여러 관리형 규칙을 사용하면 평판에 따라 IP를 차단하거나 익명 IP 등을 차단할 수 있음
	- CloudFront로는 특정 지역을 차단할 수 있음
		- CloudFront와 WAF는 관리형 서비스로 요청을 필터링해줌
- Shield Advanced (BP1, BP2, BP6)
	- 이 서비스를 활성화하면 자동으로 WAF 규칙을 생성하여 계층 7 공격을 완화
		- 따라서 애플리케이션 계층 방어에 유용
	- 악성 요청이 들어오지 못하도록 하거나 그 수를 최소화하는 식으로 EC2 인스턴스를 보호할 수 있음

## Attack Surface Reduction

![ddos](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/ddos1.png)

- 공격 지점을 줄이는 방법
- AWS 리소스를 숨김 (BP1, BP4, BP6)
	- 본 아키텍처는 애플리케이션에 사용되는 백엔드 AWS 리소스가 숨겨져 있음
	- BP1, BP4, BP6가 그 예시
	- CloudFront, API Gateway, 엘라스틱 로드 밸런싱을 사용하면 백엔드 리소스를 숨길 수 있음
	- 공격자는 이 리소스가 Lambda 함수인지 EC2 인스턴스나 ECS 태스크인지 알 수 없음
- 보안 그룹과 네트워크 ACL 등을 설정 (BP5)
	- 특정 IP의 트래픽을 필터링할 수 있음
	- Elastic IP(탄력적 IP)도 AWS Shield Advanced로 보호할 수 있음
- API 엔드 포인트 보호
	- API Gateway를 사용하면 어떤 백엔드든 숨길 수 있음
	- 엣지 최적화 모드를 사용할 경우 이미 글로벌로 설정되어 있음
	- CloudFront에 리전 모드를 더해 사용한다면 DDoS 보호에 관한 제어 기능이 더 강화됨
	- API Gateway에 WAF를 사용하는 경우 모든 HTTP 요청을 필터링할 수 있음
		- API Gateway를 제대로 설정했다면 버스트 제한과 헤더 필터링을 할 수 있음
		- 사용자에게 API 키 사용을 강제할 수도 있음
