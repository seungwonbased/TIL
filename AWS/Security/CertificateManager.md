# AWS Certificate Manager (ACM)

![acm](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/acm1.png)

- AWS Certificate Manager(ACM)
- ACM은 TLS 인증서를 AWS에서 프로비저닝, 관리 및 배포하게 해줌
- TLS/SSL 인증서는 웹사이트에서 전송 중 암호화를 제공하는 데 쓰임
	- 웹사이트에 방문했을 때 볼 수 있는 HTTPS의 S는 보안(secure)을 의미하며 이를 통해 트랜잭션에 TLS 인증서가 포함되어 있음을 알 수 있음
- 작동 원리
	- 오토 스케일링 그룹에 연결된 ALB가 있다고 가정
	- 이때 애플리케이션 로드 밸런서(ALB)를 HTTPS 엔드 포인트로서 노출하려 함
	- 그러려면 ALB를 AWS Certificate Manager와 통합해 ALB에서 직접 TLS 인증서를 프로비저닝 및 유지관리하도록 하면 됨
	- 그러면 사용자가 HTTPS 프로토콜을 사용하는 웹사이트 또는 API에 액세스하게 됨
- ACM은 퍼블릭과 프라이빗 TLS 인증서를 모두 지원하며 퍼블릭 TLS 인증서 사용 시에는 무료로 이용할 수 있음
- 또한 인증서를 자동으로 갱신하는 기능도 있음
- 그리고 여러 AWS 서비스와 통합되어 있어 Elastic Load Balancer(ELB)에 TLS 인증서를 불러올 수 있음
	- 예를 들면 클래식 로드 밸런서(CLB)와 애플리케이션 로드 밸런서(ALB), 그리고 네트워크 로드 밸런서(NLB)가 있음
- 또는 CloudFront 배포나 API Gateway의 모든 API에서도 불러올 수 있음
- 다만 ACM을 사용할 수 없는 곳이 하나 있는데, 그건 바로 EC2 인스턴스
	- 퍼블릭 인증서일 경우 추출이 불가능
	- ACM을 통해 EC2 인스턴스에 대한 퍼블릭 인증서를 생성할 수 없다는 뜻

## ACM - Requesting Public Certificates

- 퍼블릭 인증서는 어떤 과정을 통해 요청되는지?
1. 우선 인증서에 포함할 도메인 이름을 나열
	- corp.example.com과 같은 전체 주소 도메인 이름(FQDN)도 될 수 있고, \*.example.com과 같은 와일드카드 도메인도 가능
	- 도메인 수에는 제한이 없음
2. 유효성 검증 방법
	- 즉 DNS 검증과 이메일 검증 중 어느 것을 거칠지 선택
	- 자동화를 목적으로 SSL 인증서를 자동 갱신하려면 DNS 검증을 쓰는 편이 나음
	- 이메일 검증을 사용한다면 ACM이 도메인에 등록된 연락처로 이메일을 보내 해당 인증서를 요청했는지 여부를 확인
	- DNS 검증을 사용하기로 했다면 DNS 구성에서 CNAME 레코드를 생성해 도메인 소유권을 증명해야 함
	- Route 53이 있다면 ACM과 자동으로 통합해 이러한 작업을 수행
3. 몇 시간 후 유효성 검증이 완료
4. 인증서가 발행되고 이 퍼블릭 인증서도 자동 갱신 목록에 추가됨
	- 즉, ACM에서 스스로 생성된 모든 인증서를 만료 60일 전에 자동으로 갱신해 주니 무척 편리

## ACM - Importing Public Certificates

![acm](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/acm2.png)

- ACM에서 퍼블릭 인증서는 어떻게 가져오면 되는지?
	- ACM 외부에서 생성된 인증서를 ACM으로 가져오는 옵션도 제공
	- ACM 외부에서 생성되었기 때문에 자동 갱신은 불가능
		- 그러니 인증서가 만료되기 전에 직접 새 인증서를 가져와야 함
- 인증서가 언제 만료되는지는 어떻게 알 수 있는지?
	- ACM 서비스가 만료 45일 전부터 매일 만료 이벤트를 EventBridge 서비스에 전송해줌
		- 며칠 전부터 알려줄지는 마음대로 설정 가능
		- 말하자면 EventBridge를 통해 매일 인증서 만료 이벤트가 발생한다고 할 수 있음
		- 그다음 EventBridge에서 Lambda 함수나 SNS 주제 또는 SQS 대기열을 트리거
	- 또는 AWS Config을 사용하는 방법도 있음
		- acm-certificate-expiration-check라는 관리형 규칙이 있는데, 만료된 인증서를 확인하는 규칙
			- 여기서 일수를 조정할 수도 있음
		- 즉, Config 서비스에 규칙을 설정하면 Config 서비스가 ACM 서비스를 검사해서 규칙에 위배되는 인증서가 있을 경우 규정 비준수 이벤트가 EventBridge로 전송
		- 그리고 아까처럼 Lambda나 SNS, SQS를 트리거
	- 자동 경고를 받는 방법은 이렇게 두 가지가 있음

## ACM - Integration with ALB

![acm](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/acm3.png)

- ACM 서비스는 어떻게 ALB와 통합되는 건지?
- 백엔드에 오토 스케일링 그룹이 있는 ALB와 ACM으로 프로비저닝 및 유지관리되는 TLS 인증서가 있다고 가정
- 여기엔 아주 좋은 특이성이 있음
	- ALB에서는 HTTP에서 HTTPS로의 리디렉션 규칙을 설정할 수 있음
	- 사용자가 HTTP 프로토콜에서 애플리케이션 로드 밸런서에 액세스할 때 ALB는 HTTPS로 리디렉션하는 요청을 반환하는 것
- 그 후 사용자는 HTTPS 프로토콜에서 애플리케이션 로드 밸런서에 액세스하며 AWS Certificate Manager(ACM)에서 나오는 TLS 인증서를 사용
- 그런 다음 요청이 HTTPS 프로토콜을 통과하면 오토 스케일링 그룹으로 곧바로 전달됨

## API Gateway - Endpoints Types

- ACM이 API Gateway와 어떻게 통합되는지 살펴보기 전 우선 엔드 포인트 유형부터 설명
- 엣지 최적화 (Edge-optimized)
	- 글로벌 클라이언트를 위한 유형으로 먼저 CloudFront 엣지 로케이션으로 요청을 라우팅하여 지연을 줄이는 방법
	- 하나의 리전에만 있는 API Gateway로 보내지는 경우
- 리전 (Regional)
	- 클라이언트가 API Gateway와 같은 리전에 있을 때 쓰임
	- 이 경우 CloudFront는 사용할 수 없지만 자체 CloudFront 배포를 생성하여 캐싱 및 배포 전략을 제어할 수는 있음
- 프라이빗(Private)
	- 인터페이스 VPC 엔드 포인트(ENI)를 통해 VPC 내부서에만 액세스할 수 있음
	- 또한 프라이빗 모드에서는 API Gateway에 대한 액세스를 정의하는 리소스 정책이 필요
- ACM은 엣지 최적화 및 리전 엔드포인트에 적합

## ACM - Integration with API Gateway

![acm](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/acm4.png)

- ACM을 API Gateway와 통합하려면 우선 API Gateway에 사용자 지정 도메인 이름이라는 리소스를 생성하고 구성해야 함
- 엣지 최적화 엔드 포인트
	- 요청이 CloudFront에서 라우팅되고 그런 다음 TLS 인증서가 CloudFront 배포에 연결되기 때문에 TLS 인증서가 반드시 CloudFront와 같은 리전인 us-east-1에 생성되어야 함
	- API Gateway가 한 리전에 있고 CloudFront를 통해 배포되는 모든 것과 ACM 인증서는 반드시 us-east-1 리전에 있어야 함
		- CloudFront가 us-east-1 리전에 있기 때문
		- CloudFront를 위한 인증서는 전부 us-east-1에 있음
	- 그리고 Route 53에 CNAME이나 별칭(A-Alias) 레코드를 설정하면 끝
- 리전 엔드 포인트
	- API Gateway와 리전이 같은 클라이언트를 위한 엔드포인트로 API Gateway만 있으므로 TLS 인증서를 API Gateway에 가져올 때는 같은 리전에 속해 있어야 함
		- 예시 그림을 보면 ACM이 ap-southeast-2에만 있음
	- 그리고 Route 53에서 CNAME이나 별칭 레코드가 여러분의 DNS를 가리키도록 설정하면 됨
