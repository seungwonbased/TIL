# Amazon Inspector

![inspector](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/inspector1.png)

- Amazon Inspector는 몇 군데에서 자동화된 보안 평가를 실행할 수 있는 서비스
- For EC2 Instance
	- EC2 인스턴스에서 시스템 관리자 에이전트를 활용하면 Amazon Inspector가 해당 EC2 인스턴스의 보안을 평가하기 시작할 것
	- 의도되지 않은 네트워크 접근 가능성에 대해 분석하고, 실행 중인 운영 체제에서 알려진 취약점을 분석
	- 이건 연속적으로 수행됨
- For Container Images push to Amazon ECR
	- 또한 Amazon Inspector는 컨테이너 이미지를 Amazon ECR로 푸시할 때 실행됨
	- 예를 들어 도커 이미지
	- 컨테이너 이미지가 Amazon ECR로 푸시되면 Amazon Inspector가 알려진 취약점에 대해 검사
- For Lambda Functions	
	 - Amazon Inspector는 Lambda 함수에 대해서도 사용 가능
	 - Lambda 함수가 배포될 때 함수 코드 및 패키지 종속성에서 소프트웨어 취약성을 다시 분석
		 - 즉, 함수가 배포될 때 평가
- Amazon Inspector가 작업을 완료하면 결과를 AWS 보안 허브에 보고
- 또한 이러한 결과 및 결과 이벤트를 Amazon Event Bridge로 보냄
	- 이를 통해 인프라에 있는 취약점을 모아서 볼 수 있음
	- 그리고 Event Bridge로 일종의 자동화를 실행할 수 있음

## What does Amazon Inspector evaluate?

- Amazon Inspector는 무엇을 평가하는지?
- Inspector는 실행 중인 EC2 인스턴스, Amazon ECR의 컨테이너 이미지, Lambda 함수에만 사용된다는 점을 기억
- 필요할 때 인프라만 지속적으로 스캔
- 즉 취약성 데이터베이스, CVE를 살펴볼 것 
- EC2, ECR, Lambda에서 패키지 취약성 및 EC2에서 네트워크 도달성을 살펴볼 것
- 만약 CVE 데이터베이스가 업데이트된다면 Amazon Inspector는 자동으로 다시 실행되어 모든 인프라를 한 번 더 확실히 테스트
- 실행될 때마다, 우선 순위를 정하기 위해 위험 점수가 모든 취약성과 다시 연관됨

# AWS Macie

![Macie](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/macie1.png)

- Macie는 완벽히 관리되는 데이터 보안 및 데이터 프라이버시 서비스
- 머신러닝과 패턴 매칭을 이용해서 AWS에 있는 여러분의 민감한 데이터를 발견하고 보호
- 더 구체적으로 말하자면 개인식별정보, 즉 PII 같은 민감정보에 관해 경보를 제공
- 예시
	- S3 버킷에 PII가 있다면 그게 Macie에 의해 분석됨
	- Macie는 어떤 데이터를 PII로 분류할 수 있는지 알아내고, EventBridge를 통해 발견 결과를 알려줌
	- 그러면 그걸 SNS 토픽이나 람다 함수와 통합할 수 있음
- 클릭 한 번으로 활성화할 수 있음
	- 원하는 S3 버킷만 지정하면 그걸로 끝
