# AWS Lambda
## What's serverless?

- 서버리스는 새로운 서비스
- 서버리스 서비스를 사용하는 개발자는 서버를 관리할 필요가 없음
- 서버가 없다는 것은 아니고 관리할 필요가 없다는 뜻
- 그냥 코드를 배치하면 됨
	- 함수를 배치하는 것
- 원래 서버리스는 FaaS, 즉 Function as a Service를 뜻했지만 지금의 서버리스는 더 많은 것을 의미
- 서버리스가 처음 개발된 건 AWS Lambda에서였는데, 현재는 원격 관리되는 것을 모두 포함
	- 데이터베이스, 메시징, 스토리지 등 서버를 프로비저닝 하지 않는 모든 것들을 포함
- 서버리스란 서버가 없는 게 아니라 서버가 보이지 않거나 서버를 프로비저닝 하지 않는 것

## Serverless in AWS

![svl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/svl1.png)

- AWS에서의 서버리스
- 예시
	- 사용자가 S3 버킷에서 정적 콘텐츠를 얻는데 웹 사이트 혹은 CloudFront 와 S3로 전달됨
	- 그리고 Cognito에 로그인하는데 사용자 신원 정보를 보관하는 곳임
	- API Gateway를 통해 사용자는 REST API를 호출하고, API Gateway는 람다 함수를 호출하고, 람다 함수는 DynamoDB에서 데이터를 저장하고 회수
- AWS에는 Lambda, DynamoDB Cognito, API Gateway, Amazon S3, SNS와 SQS 등이 있음
	- SQS와 SNS에서도 서버를 관리하지 않고 자동으로 스케일링
	- Kinesis Data Firehose도 처리량에 맞춰 스케일링하고 사용한 만큼 지불하며 서버를 프로비저닝 하지 않음
	- Aurora 서버리스는 관리 서버 없이도 데이터베이스가 온디맨드로 스케일링
	- 단계 함수와 Fargate의 경우 Fargate는 ECS의 서버리스 기능이며 여기서는 도커 컨테이너를 실행할 인프라를 프로비저닝하지 않음

## Why AWS Lambda?

- Amazon EC2
	- 알다시피 Amazon EC2는 클라우드의 가상 서버라서 프로비저닝이 필요
	- 따라서 프로비저닝을 할 메모리와 CPU 크기가 제한됨
	- 지속적으로 실행되어야 함
		- 그래서 최적화를 하려면 효율적으로 시작하고 중단해야 함
		- 그렇지 않으면 인스턴스에 어떤 일이 생기든 관계없이 EC2는 지속적으로 실행됨
	- 오토 스케일링 그룹으로 스케일링할 수 있는데 다시 말해 자동으로 서버를 추가하고 제거하는 그런 작업을 해야 한다는 뜻
- AWS Lambda
	- 람다는 가상의 함수
	- 관리할 서버 없이 코드를 프로비저닝하면 함수가 실행됨
	- 제한 시간이 있어서 실행 시간이 짧다고 하지만 최대 15분은 그렇게 짧지 않음
	- 온디맨드로 실행됨
		- 즉 Lambda를 사용하지 않으면 람다 함수가 실행되지 않고 비용 역시 함수가 실행되는 동안만 청구되며 호출을 받으면 온디맨드로 실행됨
	- 마지막으로 스케일링이 자동화됨
		- 더 많은 람다 함수를 동시에 필요로 하는 경우 AWS가 자동으로 프로비저닝해서 람다 함수를 늘려줌

## Benefits of AWS Lambda

- Lambda의 장점
- 가격 책정이 아주 쉬워짐
- Lambda가 수신하는 요청의 횟수에 따라 과금
	- 호출 횟수와 컴퓨팅 시간, 즉 Lambda가 실행된 시간만큼 청구됨
	- 프리 티어에서도 Lambda를 넉넉하게 제공해 주는데, Lambda 요청 1백만 건과 40만 GB-초의 컴퓨팅 시간이 포함됨
- 다양한 AWS 서비스와 통합되기도 함
- Lambda에 여러 가지 프로그래밍 언어를 사용할 수 있어서 상당히 자유로운 편
- CloudWatch와의 모니터링 통합도 쉬움
- 함수당 더 많은 리소스를 프로비저닝 하려면 함수당 최대 10GB의 램을 프로비저닝 할 수 있음
	- 만약 함수의 RAM을 증가시키면 CPU 및 네트워크의 품질과 성능도 함께 향상될 것
	- 이 부분도 기억해야 함

## AWS Lambda Language Support

- JavaScript의 Node.js
- Python
- Java, 즉 Java 8 호환이나 Java 11
- C# .NET Core
- Golang
- C#
- Powershell
- Ruby
- 또 웬만한 언어는 모두 Lambda에서 사용 가능
- 커뮤니티가 지원하는 사용자 지정 런타임 API 덕분
	- 예를 들어 Rust 언어 함수를 Lambda에서 사용하는 것도 오픈 소스 프로젝트가 있어서 가능
- 그리고 Lambda 컨테이너 이미지를 지원
	- Lambda 컨테이너 이미지는 아주 특별한 것인데, 컨테이너 이미지 자체가 Lambda의 런타임 API를 구현해야 함
	- 즉 아무 컨테이너 이미지나 Lambda에서 실행되지는 않고 컨테이너 이미지를 만들 때 전제 조건이 필요
	- ECS와 Fargate는 계속 임의의 도커 이미지를 실행할 때 더 많이 사용됨
		- 따라서 시험에서 Lambda에 컨테이너를 실행해야 할 경우 해당 컨테이너가 Lambda 런타임 API를 구현하지 않으면 ECS나 Fargate에서 컨테이너를 실행해야 함

## AWS Lambda Integrations Main Ones

![svl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/svl2.png)

- Lambda가 다양한 AWS 서비스와 통합된다고 했는데, 어떤 방식으로 통합이 이루어지는지 예시
- API Gateway
	- REST API를 생성
	- 그리고 람다 함수를 호출
- Kinesis
	- Lambda를 이용해 바로 데이터를 변환
- DynamoDB
	- 트리거를 생성할 때 사용되는데 데이터베이스에 어떤 일이 생기면 람다 함수가 작동되도록 함
- Amazon S3
	- 언제든 람다 함수를 작동시킴
	- 예를 들면 S3에 파일이 생성되거나 할 때
- CloudFront
	- CloudFront용 람다는 Lambda@Edge
- CloudWatch 이벤트와 EventBridge
	- AWS의 인프라에 어떤 일이 생기고 그 상황에 대응하고자 할 때, 예를 들어 파이프라인이 끊기거나, 상태가 바뀌는 경우 등 상황에 따라 자동화를 실행하려고 람다 함수 사용
- CloudWatch 로그
	- 어디든 해당 로그를 스트리밍
- SNS
	- 알림과 SNS 토픽에 대처할 수 있음
- SQS
	- SQS 대기열 메시지를 처리할 수 있음
- Cognito
	- 예를 들어 사용자가 데이터베이스에 로그인할 때마다 응답

## Example: Serverless Thumbnail Creation

![svl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/svl3.png)

- 서버리스 섬네일 생성 예시
- S3 버킷이 있음
	- 여기서 바로 섬네일을 생성하고 싶음
- Amazon S3에 새 이미지가 업로드되는 이벤트가 생기고 S3 이벤트 알림을 통해 람다 함수가 작동됨
	- 그리고 이때 람다 함수에는 섬네일을 생성하는 코드가 있음
- 해당 섬네일은 다른 S3 버킷이나 같은 S3 버킷으로 푸시 및 업로드됨
	- 원래 이미지보다 작은 크기로 푸시 및 업로드됨
- 또한 람다 함수는 몇몇 데이터를 DynamoDB에 삽입할 수 있음
	- 이미지 이름, 크기, 생성 날짜 등 이미지의 메타 데이터가 삽입됨
- Lambda 덕분에 기능이 자동화되고 또 S3에 새 이미지와 앱이 생성되는 이벤트에 대한 반응형 아키텍처를 얻음

## Example: Serverless CRON Job

![svl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/svl4.png)

- 서버리스 CRON 작업 예시
- CRON이란 EC2 인스턴스에서 작업을 생성하는 방법인데, 5분마다, 월요일 10시마다 등등을 지정
- 그런데 CRON은 가상 서버에 실행해야 함
	- EC2 인스턴스 등
- 즉 인스턴스가 실행되지 않거나 CRON이 아무 일도 안 하면 인스턴스 시간이 낭비됨
- 따라서 CloudWatch 이벤트 규칙 또는 EventBridge 규칙을 만들고 1시간마다 작동되게 설정해서 1시간마다 람다 함수와 통합되면 태스크를 수행할 수 있게 됨
- 서버리스 CRON을 만드는 방법임
- 이때 CloudWatch 이벤트는 서버리스이고, 람다 함수 역시 서버리스임

## Example: AWS Lambda Pricing

- Lambda 가격 책정 예시
- 웹 사이트에 모든 정보가 있음
- 여기 정보가 최신이 아니더라도 가격 책정 방식의 예시로 보면 됨
- 호출당 청구
	- 처음 1백만 건의 요청은 무료이고, 이후 1백만 건 요청마다 20센트가 과금됨
- 기간당 청구
	- 1ms 단위로 요금이 부과됨
	- 한 달간 첫 40만 GB-초 동안의 컴퓨팅 시간은 무료로 사용
		- 여기서 GB-초라는 것은 함수가 1GB RAM을 가질 때 실행 시간이 40만 초이며 다시 말해 실행 시간이 8배 늘어나려면 함수의 RAM은 8배 작은 128MB RAM이 되어야 함
	- 이후에는 60만 GB-초당 1달러가 과금됨

## AWS Lambda Limits to Know - per region

- 시험에 응시하려면 Lambda 한도를 알아야 함
	- 시험에 자주 나오는 내용
- 한도는 리전당 존재
- 한도에는 실행 한도와 배포 한도가 있음
- 실행 한도
	- 실행 시 메모리 할당량은 128MB에서 10GB이고 메모리는 1MB씩 증가
		- 메모리가 증가하면 더 많은 vCPU가 필요
	- 최대 실행 시간은 900초 즉 15분
		- 이를 초과하면 Lambda 실행에 바람직하지 않음
	- 환경변수는 4KB까지 가질 수 있는데 상당히 제한적인 공간이나 Lambda 함수를 생성하는 동안 큰 파일을 가져올 때 사용할 수 있는 임시 공간이 있음
		- /tmp 폴더에 용량이 있으며 크기는 최대 10GB
	- Lambda 함수는 최대 1,000개까지 동시 실행이 가능하며 요청 시 증가할 수 있지만 동시성은 미리 예약해 두는 것이 좋음
- 배포 한도
	- 압축 시 최대 크기는 50MB, 압축하지 않았을 때는 250MB
		- 이 용량을 넘는 파일의 경우 /tmp 공간을 사용해야 함
		- 시작할 때 크기가 큰 파일이 있으면 /tmp 디렉터리를 사용
	- 배포 시에도 환경변수의 한도는 4KB
- 이 한도를 알고 있어야 시험 문제를 풀 수 있음
	- 예를 들어 30GB의 RAM과 30분의 실행 시간이 필요하고, 3GB의 큰 파일을 있을 때는 워크로드 처리에 Lambda 함수가 적합하지 않다는 걸 판단할 수 있음

## Customization at the Edge

- 엣지에서의 사용자 지정
- 보통은 함수와 애플리케이션을 특정 리전에서 배포하지만, CloudFront를 사용할 때는 엣지 로케이션을 통해 콘텐츠를 배포
- 모던 애플리케이션에서는 애플리케이션에 도달하기 전에 엣지에서 로직을 실행하도록 요구하기도 함
	- 이를 **엣지 함수**라고 하며 CloudFront 배포에 연결하는 코드
	- 이 함수는 사용자 근처에서 실행하여 지연 시간을 최소화하는 것이 목적
- CloudFront에는 두 종류의 함수가 있음
	- CloudFront 함수와 Lambda@Edge
- 엣지 함수를 사용하면 서버를 관리할 필요가 없음
	- 전역으로 배포되기 때문
- 사용 사례
	- CloudFront의 CDN 콘텐츠를 사용자 지정하는 경우
- 사용한 만큼만 비용을 지불하며 완전 서버리스

## CloudFront Functions & Lambda@Edge Use Cases

- 웹사이트 보안과 프라이버시
- 엣지에서의 동적 웹 애플리케이션
- 검색 엔진 최적화(SEO)
- 오리진 및 데이터 센터 간 지능형 경로에 활용
- 엣지에서의 봇 완화
- 엣지에서의 실시간 이미지 변환
- A/B 테스트 
- 사용자 인증 및 권한 부여
- 사용자 우선순위 지정
- 사용자 추적 및 분석 
- 등등
- 다양한 사용자 지정에 CloudFront 함수와 Lambda@Edge가 활용됨

## CloudFront Functions

![svl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/svl5.png)

- CloudFront 함수의 활용과 원리
- 일반적인 CloudFront의 요청 처리 과정은 화면의 도식과 같음
- CloudFront에 클라이언트가 요청을 보내는 것을 뷰어 요청이라고 함
	- 클라이언트가 뷰어이기 때문
	- 그런 다음 CloudFront가 오리진 요청을 오리진 서버에 전송
	- 서버가 CloudFront에 오리진 응답을 보내고 CloudFront가 클라이언트에게 뷰어 응답을 전송
- CloudFront 함수는 JavaScript로 작성된 경량 함수로 뷰어 요청과 응답을 수정
- 확장성이 높고 지연 시간에 민감한 CDN 사용자 지정에 사용됨
- 시작 시간은 1밀리초 미만이며 초당 백만 개의 요청을 처리
- 뷰어 요청과 응답을 수정할 때만 사용됨
- CloudFront가 뷰어로부터 요청을 받은 다음에 뷰어 요청을 수정할 수 있고 CloudFront가 뷰어에게 응답을 보내기 전에 뷰어 응답을 수정할 수 있음
- CloudFront의 네이티브 기능으로 모든 코드가 CloudFront에서 직접 관리됨
- CloudFront 함수는 고성능, 고확장성이 필요할 때 뷰어 요청과 뷰어 응답에만 사용됨

## Lambda@Edge

![svl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/svl6.png)

- Lambda@Edge의 기능은 좀 더 많음
- 이 함수는 Node.js나 Python으로 작성하며 초당 수천 개의 요청을 처리할 수 있음
- 모든 CloudFront 요청 및 응답을 변경할 수 있음
- 뷰어 요청은 앞서 본 대로고 오리진 요청은 CloudFront가 오리진에 요청을 전송하기 전에 수정할 수 있음
- 오리진 응답은 CloudFront가 오리진에서 응답을 받은 후에 수정됨
- 뷰어 응답은 CloudFront가 뷰어에게 응답을 보내기 전에 수정됨
- 함수는 us-east-1 리전에만 작성할 수 있음
	- CloudFront 배포를 관리하는 리전과 같은 리전
- 함수를 작성하면 CloudFront가 모든 로케이션에 해당 함수를 복제

## CloudFront Functions vs Lambda@Edge

![svl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/svl7.png)

- CloudFront 함수와 Lambda@Edge의 비교표
- 가장 눈에 띄는 차이점은 런타임 지원
	- CloudFront는 JavaScript
	- Lambda@Edge는 Node.js와 Python
- 확장성
	- CloudFront 함수: 매우 높음, 수백만 개의 요청을 처리
	- Lambda@Edge: 수천 개 수준
- 트리거 발생 위치
	- Lambda@Edge는 뷰어와 오리진 모두에게 영향을 미침
	- CloudFront 함수는 뷰어에만 영향력이 있음
- 최대 실행 시간
	- CloudFront 함수의 최대 실행 시간은 1밀리초 미만
		- 아주 빠르고 간단한 함수
	- Lambda@Edge는 실행에 5~10초가 소요됨
		- 이 함수들로 여러 로직을 실행할 수 있음

## CloudFront Functions vs Lambda@Edge - Use Cases

- CloudFront 함수
	- 캐시 키를 정규화
		- 요청 속성을 변환하여 최적의 캐시 키를 생성
	- 헤더 조작
		- 요청이나 응답에 HTTP 헤더를 삽입, 수정, 삭제하도록 헤더를 조작
	- URL을 다시 쓰거나 리디렉션
	- 인증과 권한 부여 요청
		- 요청을 허용 또는 거부하기 위해 JWT를 생성하거나 검증하는 요청
	- 모든 작업이 1밀리초 이내에 이뤄짐
- Lambda@Edge
	- CPU와 메모리가 증가
		- 여러 라이브러리를 로드할 수 있고 타사 라이브러리에 코드를 의존시킬 수 있음
		- 가령 SDK에서 다른 AWS 서비스에 액세스할 수 있도록 할 수 있음
	- 네트워크 액세스를 통해 외부 서비스에서 데이터를 처리
		 - 대규모 데이터 통합도 수행할 수 있음
	- 파일 시스템이나 HTTP 요청 본문에도 바로 액세스할 수 있음
		- 다양한 사용자 지정이 가능
	- 실행 시간은 10초가 걸릴 수도 있음

## Lambda by Default

![svl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/svl8.png)

- 기본적으로 Lambda 함수를 시작하면 VPC 외부에서 시작됨
	- 따라서 VPC 내의 리소스에 액세스할 권한이 없음
	- RDS 데이터베이스, ElastiCache 캐시 내부 로드 밸런서를 시작하면 Lambda 함수가 해당 서비스에 액세스할 수 없음
	- Lambda 배포의 기본 설정임
- 인터넷의 퍼블릭 API에 액세스하는 것은 가능
- DynamoDB에 액세스할 수 있는 건 DynamoDB가 AWS 클라우드의 퍼블릭 리소스이기 때문
	- 하지만 프라이빗 RDS 데이터베이스에는 연결할 수 없음

## Lambda in VPC

![svl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/svl9.png)

- 이를 해결하려면 VPC 내에서 Lambda 함수를 시작하면 됨
- 이를 위해서는 VPC ID Lambda 함수를 시작하려는 서브넷을 지정하고 Lambda 함수에 보안 그룹을 추가해야 함
- 그러면 Lambda가 서브넷에 엘라스틱 네트워크 인터페이스를 생성해 여러분의 VPC에서 실행되는, 예를 들면 Amazon RDS에 액세스할 수 있게 됨
- 이렇게 하면 VPC 내 모든 항목에 비공개로 연결 가능

### Lambda with RDS Proxy

![svl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/svl10.png)

- VPC에서 Lambda를 사용하는 대표적인 사용 사례는 RDS 프록시
- 따라서 RDS 데이터베이스가 프라이빗 서브넷에 있어도 Lambda 함수로 직접 해당 DB에 액세스할 수 있음
- 그런데 이런 방법으로 RDS 데이터베이스에 직접 액세스하면 큰 문제가 발생
	- Lambda 함수의 수가 너무 많이 생성되었다 사라지길 반복하면 개방된 연결이 너무 많아서 RDS 데이터베이스의 로드가 상승해 시간 초과 등의 문제로 이어짐
	- 이를 해결하는 방법은 RDS 프록시를 시작하는 것
- 이 프록시가 연결을 한곳으로 모으고 RDS 데이터베이스 인스턴스 연결의 수를 줄임
- Lambda 함수가 RDS 프록시에 연결되고 RDS 프록시가 RDS DB 인스턴스로 연결되므로 아키텍처상의 문제가 해결됨
- RDS 프록시의 세 가지 장점
	- 데이터베이스 연결의 풀링과 공유를 통해 확장성을 향상
	- 장애가 발생할 경우 장애 조치 시간을 66%까지 줄여 가용성을 향상시키고 연결을 보존
		- RDS와 Aurora 모두에 적용됨
	- RDS 프록시 수준에서 IAM 인증을 강제하여 보안을 높일 수 있고, 자격 증명은 Secrets Manager에 저장됨
- Lambda 함수가 RDS 프록시에 연결할 수 있으려면 VPC 내에서 Lambda 함수를 시작해야 함
	- RDS 프록시는 퍼블릭 액세스가 불가능하므로 Lambda 함수를 퍼블릭으로 시작하면 RDS 프록시에 네트워크 연결을 할 수가 없음

## Invoking Lambda from RDS & Aurora

![svl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/svl11.png)

- RDS, Aurora와 람다는 긴밀하게 통합되는지?
- 사실 어떤 경우에 데이터베이스 인스턴스 안에서 람다 함수를 호출할 수도 있음
	- 그러면 데이터베이스 안에서 일어나는 데이터 이벤트를 처리할 수 있게됨
	- 그리고 이건 RDS for PostgreSQL과 Aurora MySQL에 지원됨
- 예를 들어 사용자가 이벤트 데이터를 등록 테이블에 삽입
	- 그리고 RDS는 람다 함수를 직접 호출하도록 설정될 것임
	- 람다 함수는 사용자에게 환영 이메일을 보낼 수 있고, 사용자는 그걸 받게됨
	- 이건 데이터베이스에 접속하고 그 안에서 설정해야 하는 것들임
		- AWS 콘솔에서 설정하는 게 아님
- 그래서 RDS 데이터베이스 인스턴스로부터 람다 함수로 오는 인바운드 트래픽을 반드시 허용해야 함
	- 모든 방식으로 오는 트래픽을 허용해야 함
	- 왜냐면 람다가 퍼블릭 인터넷에 있는 경우에도 허용해야 함
	- 그럼 호출 또는 NAT 게이트웨이 또는 VPC 엔드포인트 등을 사용할 수 있음
	- 네트워크 연결성이 중요
- 그뿐만 아니라 데이터베이스 인스턴스가 람다 함수를 호출할 것이기 때문에 람다 함수를 호출할 필수적인 권한을 갖고 있어야 함
	- 그래서 데이터베이스 인스턴스가 적절한 IAM 정책을 갖고 있는지 확인해야 함

## RDS Event Notification

![svl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/svl12.png)

- RDS 이벤트 알림
- 이 알림들은 AWS 안에서 이루어짐
- 데이터베이스 인스턴스 자체에 대한 정보를 알려주는 알림들이 있음
	- 예를 들면 생성된 시각이나 정지된 시각, 시작된 시각 등을 알려줌
	- 그리고 데이터베이스 안의 데이터에 대한 정보는 전혀 없음
	- 그러니 함정에 빠지면 안됨
- 데이터베이스 인스턴스, 데이터베이스 스냅샷, 파라미터 그룹, 보안 그룹, 프록시 또는 커스텀 엔진 버전에 관한 이벤트를 구독할 수 있음
	- 하지만 데이터베이스 안의 데이터에 관한 정보는 받지 않음
- 전달 시간이 최대 5분인 근 실시간 이벤트를 받고, 그걸 SNS에 전송하거나 그것들을 EventBridge에서 가로챌 수도 있음
	- SNS에서 예를 들어 SQS 대기열이나 람다 함수로 그것들을 전송할 수 있음
		- EventBridge에는 예를 들어 람다 함수 같은 아주 많은 다양한 전송 대상이 있음
	- 하지만 데이터 이벤트 자체에 관한 정보는 받지 못한다는 점을 꼭 기억