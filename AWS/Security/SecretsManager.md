# AWS Secrets Manager

- AWS Secrets Manager라는 아주 간단한 서비스
- 암호를 저장하는 최신 서비스로 SSM Parameter Store와는 다른 서비스
- Secrets Manager는 X일마다 강제로 암호를 교체하는 기능이 있어 암호 관리를 더 효율적으로 할 수 있음
- 또한 AWS Secrets Manager로 교체할 암호를 강제 생성 및 자동화할 수 있음
	- 이를 위해 새 암호를 생성할 Lambda 함수를 정의해야 함
- 또한 AWS Secrets Manager는 AWS가 제공하는 다양한 서비스와도 아주 잘 통합됨
	- Amazon RDS, MySQL PostgreSQL, Aurora 등
- AWS 외 여러 서비스와 데이터베이스에도 즉시 통합할 수 있음
	- 데이터베이스에 접근하기 위한 사용자 이름과 비밀번호가 AWS Secrets Manager에 바로 저장되고 교체 등도 가능
- 암호는 KMS 서비스를 통해 암호화됨
- RDS와 Aurora의 통합 혹은 암호에 대한 내용이 시험에 나오면 AWS Secrets Manager를 생각

## Secrets Manager - Multi-Region Secrets

![secrets](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/secrets1.png)

- 다중 리전 암호의 개념
	- 복수 AWS 리전에 암호를 복제할 수 있고 AWS Secrets Manager 서비스가 기본 암호와 동기화된 읽기 전용 복제본을 유지한다는 개념
- 두 리전이 있을 때 기본 리전에 암호를 하나 만들면 보조 리전에 동일한 암호가 복제됨
- 이렇게 하는 이유
	- 첫째, us-east-1에 문제가 발생하면 암호 복제본을 독립 실행형 암호로 승격할 수 있음
	- 그리고 여러 리전에 암호가 복제되니 다중 리전 앱을 구축하고 재해 복구 전략도 짤 수 있음
	- 한 리전에서 다음 리전으로 복제되는 RDS 데이터베이스가 있다면 동일한 암호로 동일한 RDS 데이터베이스 즉 해당 리전의 해당 데이터베이스에 액세스할 수 있음
