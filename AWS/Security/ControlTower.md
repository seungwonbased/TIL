# AWS Control Tower

- AWS Control Tower 서비스를 사용하면 모범 사례를 기반으로 안전하고 규정을 준수하는 다중 계정 AWS 환경을 손쉽게 설정하고 관리할 수 있음
- 다중 계정을 생성하기 위해 Control Tower 서비스는 AWS Organization 서비스를 사용해 계정을 자동 생성
- Control Tower 서비스의 장점
	- 클릭 몇 번으로 환경을 자동으로 설정할 수 있음
	- 원하는 모든 것을 미리 구성이 가능
	- 가드레일을 사용해 자동으로 지속적인 정책 관리를 할 수 있음
	- 정책 위반을 감지하고 자동으로 교정할 수 있음
	- 대화형 대시보드를 통해 모든 계정의 전반적인 규정 준수를 모니터링
- AWS Organization를 활용하는 또 다른 서비스

## AWS Control Tower - Guardrails

![act](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/act1.png)

- 가드레일
- AWS에서 다중 계정을 설정한다고 가정
- 이때 특정 항목에 관해 한 번에 모두 제한하거나 특정 유형의 규정 준수 사항을 모니터링하고자 함
- 이때 가드레일을 사용하면 Control Tower 환경 내의 모든 계정에 관한 거버넌스를 얻을 수 있음
- 가드레일의 두 가지 유형
	- 예방(Preventive) 가드레일
		- 계정을 무언가로부터 보호하는 것이므로 제한적이기 때문에 AWS Organization 서비스의 서비스 제한 정책인 SCP를 사용해 모든 계정에 적용
		- 예를 들어, 예방 가드레일을 생성해 모든 계정에서 리전을 제한하고 us-east-1과 eu-west-2의 리전에서만 작업하도록 할 수 있음
		- Control Tower에서 Organization에 SCPs를 사용하도록 하는 것
	- 탐지(Detective) 가드레일
		- 규정을 준수하지 않는 것을 탐지하는 것
		- 규정 준수에 대해 말한 것처럼 AWS Config 서비스를 사용
		- 가령, 계정에서 태그가 지정되지 않은 리소스를 식별한다고 하면 Control Tower로 탐지 가드레일을 설정하고 AWS Config를 사용하면 모든 멤버 계정에 Config가 배포되어 규칙과 태그가 지정되지 않은 리소스를 모니터링 하는데 규정을 준수하지 않으면 SNS 주제를 트리거 해서 관리자로서 알림을 받거나 SNS 주제도 Lambda 함수를 실행해 Lambda 함수가 자동으로 문제를 교정
			- 즉, 태그가 지정되지 않은 리소스에 태그를 추가
