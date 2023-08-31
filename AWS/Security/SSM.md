# SSM Parameter Store

![ssm](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/ssm1.png)

- SSM Parameter Store
- 구성 및 암호를 위한 보안 스토리지
- 구성을 암호화할지 선택할 수 있으므로 KMS 서비스를 이용해 암호를 만들 수 있음
- SSM Parameter Store는 서버리스이며 확장성과 내구성이 있고 SDK도 사용이 용이
- 또한 매개변수를 업데이트할 때 구성과 암호의 버전을 추적할 수도 있음
- IAM을 통해 보안이 제공되며 특정한 경우에는 Amazon EventBridge로 알림을 받을 수도 있음
- CloudFormation과 통합도 됨
	- 즉 CloudFormation이 Parameter Store의 매개변수를 스택의 입력 매개변수로 활용할 수 있다는 뜻
- 예를 들어 애플리케이션과 SSM Parameter Store가 있을 때 평문 구성을 저장한다고 가정
	- 그러면 EC2 인스턴스 역할 같은 애플리케이션의 IAM 권한이 확인됨
- 아니면 암호화된 구성이 있다고 가정
	- 그러면 SSM Parameter Store가 KMS로 구성을 암호화
	- KMS 서비스가 암호화 및 복호화에 사용된다는 뜻
	- 물론 암호화와 복호화를 수행하려면 애플리케이션이 기본 KMS 키에 액세스할 수 있어야함

## SSM Parameter Store Hierarchy

![ssm](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/ssm2.png)

- 이렇게 계층 구조가 있는 Parameter Store에 매개변수를 저장할 수 있음
- 예를 들어 /my-department/를 경로로 정의
- 하위에 my-app/ 그 하위에 dev/가 있고 그 폴더 안에 db-url과 db-password가 있다고 가정
- 매개변수가 계층 구조 아래로 쭉 내려감
- 상위로 한 단계 올라가 prod/의 매개변수가 될 db-url, db-password도 저장할 수 있고, other-app/이나 /other-department/로 올라갈 수도 있음
- 원하는 방식으로 매개변수를 구조화할 수 있음
	- 구조화를 통해 IAM 정책을 간소화하여 애플리케이션이 /my-department/ 혹은 my-app/ 전체 경로에 혹은 my-app/ 또는 /my-department/ 환경의 특정 경로에만 액세스할 수 있도록 함
- 또한 사진에 보이는 레퍼런스를 사용해 Parameter Store로 Secrets Manager의 암호에 액세스할 수도 있음
- AWS에서 발행하는 퍼블릭 매개변수도 사용할 수 있음 
	- 예를 들어 특정 리전에서 Amazon Linux 2의 최신 AMI를 찾으려 할 때, Parameter Store에서 API 호출을 대신해 쓸 수 있음
	- 만약 Dev Lambda 함수라는 애플리케이션을 사용한다면 my-app/ 내 dev/의 db-url과 db-password에 액세스를 허용하는 IAM 역할을 함
- 여러 IAM 정책과 일부 환경변수 때문에 Prod Lambda 함수를 사용한다면 다른 경로에 있는 prod/의 db-url과 db-password에도 액세스할 수 있음

## Standard and Advanced Parameter Tiers

![ssm](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/ssm3.png)

- Systems Manager에는 두 가지 매개변수 티어가 있는데, 표준과 고급
- 가장 큰 차이는 용량으로 각각 4KB, 8KB
- 매개변수 정책 적용 여부 또한 다름
	- 표준 티어에선 적용되지 않고 고급 티어에선 적용됨	
- 고급 티어는 월 0.05달러를 지불해야 하지만 표준 티어는 무료

### Parameter Policies for Advanced Parameter

![ssm](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/ssm4.png)

- 고급 매개변수에서만 사용할 수 있는 매개변수 정책
- 한 번에 여러 정책을 할당할 수도 있음
- 매개변수 정책을 통해 만료 기한을 뜻하는 타임 투 리브(TTL)를 매개변수에 할당할 수 있음
	- 이렇게 사용자가 비밀번호 등의 민감한 정보를 업데이트 또는 삭제하도록 강제
- 매개변수를 삭제하는 만료 정책은 타임스탬프에서 이 매개변수를 반드시 삭제하도록 함
	- 그리고 EventBridge와 통합함으로써 EventBridge에서 알림을 받을 수 있음
	- 예시를 보면 매개변수가 만료되기 15일 전에 EventBridge 알림을 받게 됨
	- TTL로 매개변수가 삭제되기 전에 업데이트할 수 있을 만큼 기간 여유가 충분
- EventBridge는 변경이 없다는 알림도 제공

