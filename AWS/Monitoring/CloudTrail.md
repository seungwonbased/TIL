# Amazon CloudTrail

- CloudTrail는 AWS 계정의 거버넌스, 감사 및 규정 준수를 도움
- CloudTrail은 기본적으로 활성화돼 있고 콘솔, SDK, CLI뿐만 아니라 기타 AWS 서비스에서 발생한 AWS 계정 내의 모든 이벤트 및 API 호출 기록을 받아 볼 수 있음
- 모든 로그가 CloudTrail에 표시됨
- 또한 CloudTrail의 로그를 CloudWatch Logs나 Amazon S3로 옮길 수 있음
	- 전체 또는 단일 리전에 적용되는 트레일을 생성해 모든 리전에 걸친 이벤트 기록을 한곳으로 모을 수 있음
	- 예를 들면 S3 버킷에 모을 수 있음
- CloudTrail를 사용하면 누군가가 AWS에서 무언가를 삭제했을 때 대처할 수 있음
	- 예를 들어 EC2 인스턴스가 종료됐을 경우 누가 했는지를 알고 싶으면 CloudTrail을 들여다보면 됨
	- CloudTrail에 해당 API 호출이 남아 있으므로 누가 언제 문제를 일으켰는지 알아낼 수 있음

![trail](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/trail1.png)

도식으로 살펴보죠 CloudTrail이 중심에 있고

SDK, CLI, 콘솔, IAM 사용자 IAM 역할, AWS 서비스의 작업이

CloudTrail 콘솔에 표시됩니다

이를 분석하면 무슨 일이 일어났는지 알 수 있죠

모든 이벤트를 90일 이상 보관하려면

CloudWatch Logs 또는 S3 버킷으로 보내면 됩니다

CloudTrail을 좀 더 자세히 알아보죠

CloudTrail에는 세 종류의 이벤트가 있습니다

첫 번째는 관리 이벤트로 이는 AWS 계정의

리소스에서 수행되는 작업을 나타냅니다

예를 들어 누군가가 보안 설정을 구성하면 IAM AttachRolePolicy라는

API를 사용하게 되는데 이것이 CloudTrail에 표시됩니다

서브넷을 생성해도 표시되고 로깅을 설정해도 기본으로 표시됩니다

리소스나 AWS 계정을 수정하는 모든 작업이 CloudTrail에 표시됩니다

또한 트레일은 기본적으로 관리 이벤트를 기록하도록 구성되어 있습니다

관리 이벤트는 두 종류로 구분할 수 있습니다

먼저 리소스를 수정하지 않는 읽기 이벤트가 있어요

IAM의 모든 사용자 목록 생성하거나

EC2의 모든 EC2 인스턴스 목록을 생성할 경우에는

리소스를 수정할 수 있는 쓰기 이벤트를 분리해야 합니다

DynamoDB 테이블 삭제 또는 삭제 시도 등이 여기에 속합니다

당연하지만 쓰기 이벤트의 중요도가 훨씬 높습니다

여러분의 AWS 인프라를 망칠 수 있기 때문입니다

반면 읽기 이벤트는 정보를 얻기 위한 활동으로

중요하긴 하지만 덜 파괴적입니다

그리고 별도로 데이터 이벤트가 있습니다

데이터 이벤트는 고볼륨 작업이므로 기본적으로 로깅되지 않습니다

데이터 이벤트란 무엇일까요?

GetObject, DeleteObject PutObject와 같은

Amazon S3 객체 수준 작업은

S3 버킷에서 빈번히 발생하는 이벤트이므로

기본적으로 로깅되지 않고

읽기 이벤트와 쓰기 이벤트로 분리할 수 있습니다

GetObject는 읽기 이벤트로

DeleteObject나 PutObject는 쓰기 이벤트로 분리하는 거죠

CloudTrail의 또 다른 이벤트 종류로는

AWS Lambda 함수 실행 작업이 있습니다

누군가 Invoke API를 사용할 때마다

Lambda 함수가 몇 번 활용되었는지 알 수 있습니다

이 역시 Lambda 함수가 많이 실행되면 볼륨이 커질 수 있습니다

CloudTrail의 세 번째 이벤트 종류는 CloudTrail Insights 이벤트입니다

CloudTrail Insights는 다음 슬라이드에서 자세히 살펴보죠

CloudTrail Insights를 살펴봅시다

모든 유형의 서비스에 걸쳐 너무 많은 관리 이벤트가 있고

계정에서 다수의 API가 매우 빠르게 발생해서

무엇이 이상하거나 특이한지 파악하기 어려울 때

CloudWatch Insights를 활용하면 됩니다

비용을 지불하고 CloudTrail Insights를 활성화하면

이벤트를 분석해서 계정 내의 특이한 활동을 탐지해 줍니다

특이한 활동에는 부정확한 리소스 프로비저닝

서비스 한도 도달 AWS IAM 작업 버스트

주기적 유지 관리 작업 부재가 있습니다

CloudTrail이 정상적인

관리 활동을 분석해서 기준선을 생성한 다음

무언가를 변경하거나 변경하려고 시도하는 모든 쓰기 유형의 이벤트를

지속적으로 분석해서 특이한 패턴을 탐지하게 됩니다

간단히 말하자면 CloudTrail Insights는

관리 이벤트를 지속적으로 분석해서 뭔가를 탐지하면 인사이트 이벤트를 생성합니다

이상 상황, 즉 인사이트 이벤트는 CloudTrail 콘솔에 표시됩니다

원한다면 Amazon S3로 전송할 수도 있고

이메일을 보내는 CloudTrail Insights에

자동화를 추가하면 EventBridge 이벤트가 생성됩니다

CloudTrail Insights의 개념이었고요

마지막으로 CloudTrail의 이벤트 보존 기간을 알아봅시다

이벤트는 CloudTrail에 기본적으로 90일 간 저장되고 이후에는 삭제됩니다

그러나 감사 목적으로 일 년 정도 전에 일어난 일을

살펴보기 위해 이벤트를 더 오래 저장하고 싶을 수도 있습니다

기본 기간 이상으로 이벤트를 보존하려면

S3로 전송해서 S3에 로그를 기록하고

Athena를 사용해 분석하면 됩니다

간단히 말해 모든 관리 이벤트 데이터 이벤트, 인사이트 이벤트는

CloudTrail에 90일 동안 보존되고

그 후 장기 보존을 위해서는 S3 버킷에 로깅해야 합니다

분석할 준비가 되면 Athena 서비스를 사용합니다

S3의 데이터를 쿼리하는 서버리스 서비스로

관심 있는 이벤트를 찾고 더 많은 것을 알 수 있게 해 줍니다

CloudTrail 통합 중에 API 호출을 가로채는

Amazon EventBridge와의 통합은 꼭 알아야 합니다

사용자가 테이블 삭제 API 호출을 사용해서

DynamoDB의 테이블을 삭제할 때마다 SNS 알림을 받고 싶다고 가정해 보죠

AWS에서 API 호출을 실행할 때마다

API 호출 자체가 CloudTrail에 로깅되죠

모든 API 호출이 로깅됩니다

그리고 모든 API 호출은

Amazon EventBridge에 이벤트로 기록됩니다

여기서 특정 테이블 삭제 API 호출을 찾아 규칙을 생성하는 겁니다

규칙에는 대상이 필요하죠

Amazon SNS를 대상으로 설정하면 경고를 생성할 수 있습니다

아주 간단하죠

어떤 API 호출에도 적용할 수 있어요

CloudTrail 관련 주제를 모두 살펴보았습니다

유익한 강의였길 바라며 다음 강의에서 뵙겠습니다