# Amazon Cognito

- Cognito는 사용자에게 웹 및 모바일 앱과 상호 작용할 수 있는 자격 증명을 부여
	- 일반적으로 이 사용자들은 AWS 계정 외부에 있음
- 모르는 사용자들에게 자격 증명을 부여해 사용자를 인식(Cognito)
- Cognito에는 두 종류의 하위 서비스가 있음
	- Cognito User Pools
		- 앱 사용자에게 가입 기능을 제공
		- API Gateway 및 애플리케이션 로드 밸런서와 원활히 통합됨
	- Cognito Identity Pools (Federated Pools)
		- 앱에 등록된 사용자에게 임시 AWS 자격 증명을 제공해 일부 AWS 리소스에 직접 액세스할 수 있도록 해 주고 Cognito 사용자 풀과 원활히 통합됨
- Cognito vs IAM
	- IAM에 이미 사용자가 있지 않나 하는 의문이 떠오를 수 있음
		- 하지만 Cognito는 AWS 외부의 웹과 모바일 앱 사용자를 대상으로 함
	- '수백 명의 사용자' '모바일 사용자' 'SAML을 통한 인증' 같은 키워드가 나오면 Cognito를 설명하는 것
-  Cognito는 좀 복잡한 서비스
	- 거시적인 수준에서 개념만 이해
	- 웹이나 모바일 애플리케이션의 사용자 기반을 생성할 수 있고 세분화된 액세스 제어를 위해 DynamoDB에서 행 수준 보안을 활성화할 수 있음
	- Cognito 사용자 풀과 API Gateway 또는 애플리케이션 로드 밸런서를 통합할 수 있다는 점을 기억

## Cognito User Pools (CUP)

![cog.png](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/cog1.png)

- 웹 및 모바일 앱을 대상으로 하는 서버리스 사용자 데이터베이스
- 사용자 이름 또는 이메일, 비밀번호의 조합으로 간단한 로그인 절차를 정의할 수 있음
- 비밀번호 재설정 기능이 있고 이메일 및 전화번호 검증 및 사용자 멀티팩터 인증이 가능
- 또한 Facebook이나 Google과 통합할 수 있어 소셜 로그인도 가능
- Cognito 사용자 풀은 API Gateway나 애플리케이션 로드 밸런서와 통합됨
	- API Gateway를 예로 들면 사용자는 Cognito 사용자 풀에 접속해서 토큰을 받고 검증을 위해 토큰을 API Gateway에 전달
		- 확인이 끝나면 사용자 자격 증명으로 변환되어 백엔드의 Lambda 함수로 전달됨
		- Lambda 함수는 처리할 사용자가 잘 인증된 구체적인 사용자라는 사실을 인식
	- 다른 방법으로 완전히 동일한 작업이 가능한데, Cognito 사용자 풀을 애플리케이션 로드 밸런서 위에 배치하면 됨
		- 애플리케이션이 Cognito 사용자 풀에 연결한 다음 애플리케이션 로드 밸런서에 전달해서 유효한 로그인인지 확인
		- 유효하다면 요청을 백엔드로 리다이렉트하고 사용자의 자격 증명과 함께 추가 헤더를 전송
	- API Gateway나 ALB를 통해 사용자의 로그인을 한곳에서 확실히 검증할 수 있고 검증 책임을 백엔드로부터 백엔드의 로드를 밸런싱하는 실제 위치, 즉 API gateway 또는 ALB로 옮긴 것

## Cognito Identity Pools (Federated Pools)

![cog.png](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/cog2.png)

- Cognito의 또 다른 하위 서비스는 Cognito 자격 증명 풀
	- 페더레이션 자격 증명이라고도 함
- 사용자에게 자격 증명을 제공하지만 API Gateway나 애플리케이션 로드 밸런서를 통해서 애플리케이션에 액세스하지 않고 임시 AWS 자격 증명을 사용해 AWS 계정에 직접 액세스
	- 따라서 사용자는 Cognito 사용자 풀 내의 사용자가 될 수도 있고 타사 로그인이 될 수도 있음
- 직접 또는 API Gateway를 통해 서비스에 액세스할 수도 있음
- 자격 증명에 적용되는 IAM 정책은 Cognito 자격 증명 풀 서비스에 사전 정의되어 있음
	- user_id를 기반으로 사용자 정의하여 세분화된 제어를 할 수도 있음
	- 원한다면 기본 IAM 역할을 정의할 수도 있음
	- 게스트 사용자나 특정 역할이 정의되지 않은 인증된 사용자는 기본 IAM 역할을 상속하게 됨
- 예시
	- 웹이나 모바일 애플리케이션에서 S3 버킷 또는 DynamoDB 테이블에 직접 액세스하고 싶음
	- Cognito 자격 증명 풀을 사용
	- 웹, 모바일 앱이 로그인해서 토큰을 받음
		- Cognito 사용자 풀에 대한 로그인일 수도 있고, 소셜 자격 증명 제공자나 SAML OpenID Connect일 수도 있음
	- 그런 다음 이 토큰을 Cognito 자격 증명 풀 서비스에 전달해서 토큰을 임시 AWS 자격 증명과 교환
		- 이를 위해 Cognito 자격 증명 풀은 전달 받은 토큰이 올바른지, 즉 유효한 로그인인지 평가
	- 다음으로는 해당 사용자에게 적용되는 IAM 정책을 생성
		- 관련 IAM 정책이 적용된 임시 자격 증명 덕분에 AWS의 S3 버킷이나 DynamoDB 테이블에 직접 액세스할 수 있는 것
		- API Gateway나 애플리케이션 로드 밸런서를 거치지 않고도 가능

### Cognito Identity Pools  Row Level Security in DynamoDB

![cog.png](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/cog3.png)

- Cognito 자격 증명 풀을 사용하면 DynamoDB에 행 수준 보안을 설정할 수 있음
	- Cognito 자격 증명 풀에 화면과 같은 정책이 있을 때 이 안에 조건을 설정하는 것
	- 이 예시에서는 DynamoDB의 LeadingKeys가 Cognito 자격 증명 사용자 ID와 같아야 한다는 조건
		- 이 정책이 적용된 사용자는 DynamoDB 테이블의 모든 항목을 읽고 쓸 수 있는 것이 아니라 이 조건을 통해 액세스를 얻은 항목에만 액세스할 수 있음