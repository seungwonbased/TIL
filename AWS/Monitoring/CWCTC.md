# CloudWatch vs CloudTrail vs Config

- 자주 출제되는 문제 중 하나는 CloudWatch, CloudTrail Config를 각각 구분하는 것
- CloudWatch
	- CloudWatch는 지표, CPU 네트워크 등의 성능 모니터링과 대시보드를 만드는 데 사용됨
	- 이벤트와 알림을 받을 수도 있고 로그 집계 및 분석 도구도 사용할 수 있음
- CloudTrail
	- CloudTrail은 생소할 수도 있지만 기본적으로 계정 내에서 만든 API에 대한 모든 호출을 기록
		- 누구에 의한 호출이든 간에 기록
	- 특정 리소스에 대한 추적도 정의할 수 있음
		- 예를 들면 EC2에 대한 정보만 더 얻을 수도 있음
	- 글로벌 서비스임
- Config
	- Config는 구성 변경을 기록하고 규정 준수 규칙에 따라 리소스를 평가
	- 변경과 규정 준수에 대한 타임라인을 멋진 UI로 보여줌
- 세 가지 모두 전혀 다른 서비스

## For an Elastic Load Balancer

- ELB에서 일어난 일을 이해하는 데 세 서비스가 어떻게 도움을 주는지 일래스틱 로드 밸런서를 통해 분석
- CloudWatch
	- CloudWatch는 들어오는 연결 수를 모니터링하고 오류 코드 수를 시간 흐름에 따라 비율로 시각화할 수 있으며 로드 밸런서의 성능을 볼 수 있는 대시보드도 만들 수 있음
	- 글로벌 애플리케이션에 로드 밸런서가 여러 개 있다면 글로벌 대시보드를 만들 수도 있음
- CloudTrail
	- CloudTrail은 누가 API를 호출하여 로드 밸런서를 변경했는지 추적
	- 누군가 보안 그룹 규칙을 바꾸거나 혹은 SSL 인증서를 바꾸거나 삭제한다면 CloudTrail이 누가 그랬는지 알려줄 수 있음
- Config
	- 로드 밸런서에 대한 보안 그룹 규칙을 추적해 누구도 수상하게 행동하거나 무언가 변경하지 못하게 하려고 사용
	- 혹은 누군가 SSL 인증서 등을 수정하지는 않는지 감시하기 위해 로드 밸런서의 구성 변경을 추적하는 데 사용
	- 또한 SSL 인증서가 로드 밸런서에 항상 할당되어 있어야 한다는 규칙을 만들어 암호화되지 않은 트래픽이 로드 밸런서에 접근하지 못하게 할 수도 있음
- 로드 밸런서로 예시를 들었듯 이 서비스들은 상호 보완적