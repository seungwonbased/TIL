# Solutions Architectures: MyBlog.com
## Serverless Hosted Website: MyBlog.com

- 서버리스로 호스팅 되는 웹사이트, MyBlog.com
- 요구 사항
	- 이 웹사이트는 글로벌 스케일 아웃이 가능해야 함
		- 보통은 블로그를 작성하는 빈도보다 읽는 빈도가 높을 것임
		- 블로그는 주로 쓰이기보다는 읽힘
	- 웹사이트는 대부분 정적 파일로 구성되고 일부는 동적인 REST API로 구성될 것임
	- 캐시를 적용해서 비용을 절감하고 응답 속도도 높이고 좋은 UX를 제공
	- 블로그에 처음으로 방문하는 사람에겐 따뜻한 환영 이메일을 발송
	- 서버리스로 구현블로그에 업로드 되는 사진은 섬네일이 생성됨
		- 이것도 서버리스로 구현

## Serving Static Content, Globally

![myb](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/myb1.png)

- 콘텐츠를 서빙해야 함
- 콘텐츠는 정적이고 글로벌을 대상으로 서빙해야 함
- 클라이언트가 있고, 콘텐츠는 Amazon S3에 저장돼 있음
- 콘텐츠를 어떻게 제공하면 되는지?
	- 버킷으로 하면 됨
		- Amazon S3는 특정 리전에 있음
		- 어떻게 하면 글로벌을 대상으로 노출시킬 수 있을지?
- 전 세계를 대상으로 서비스되는 CDN 서비스인 Amazon CloudFront를 이용하면 됨
	- 클라이언트는 Amazon CloudFront의 엣지 로케이션과 통신을 하게됨
- Amazon S3에게 받은 데이터를 캐시로 저장

## Serving Static Content, Globally, Securely

![myb](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/myb2.png)

- 이 모든 작업을 안전하게 진행해야 함
- 클라이언트는 글로벌 배포를 위한 CloudFront와 인터랙트
- 이제 오리진 액세스를 제한해서 CloudFront로만 Amazon S3 버킷에 접근할 수 있도록 할 것임
- 이걸 위해서 CloudFront만 인가할 수 있도록 버킷 정책을 추가
- Amazon S3에 바로 접근하는 사용자는 인가를 받지 못하고 Amazon S3 버킷은 보호됨

## Adding a Public Serverless REST API

![myb](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/myb3.png)

- 퍼블릭 서버리스 REST API 추가
- REST HTTPS로 Amazon API 게이트웨이와 통신하고, 게이트웨이는 람다 함수를 호출하고, 람다 함수는 Dynamo DB에 쿼리를 날리고, 읽기가 많이 발생하니 DAX를 캐시 레이어로 쓰면 좋음

## Leveraging DynamoDB Global Tables

![myb](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/myb4.png)

- 글로벌로 서빙할 때 지역에 따라 발생하는 지연을 줄이기 위해 Dynamo DB의 글로벌 데이터베이스를 사용
	- 전체 인프라와 이 구성의 속도를 향상시켜 주는 좋은 방법

## User Welcome Email Flow

![myb](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/myb5.png)

- 신규 방문자에게 보낼 환영 이메일
	- 방문자가 구독을 하면 '반갑습니다, 안녕하세요'라는 인사를 담은 메일을 보내고 싶음
- 이걸 위해 Dynamo DB에서 변경사항을 전송하기 위해 Dynamo DB 스트림을 이용하고 스트림은 람다 함수를 호출
	- 이 람다 함수는 좀 특별
	- IAM 역할을 부여 받아 Amazon SES를 사용할 수 있게 해줌
		- Amazon Simple Email Service
		- 이메일을 발송해 주는 서비스
	- Amazon 람다 함수가 AWS SDK를 이용해 Amazon SES가 이메일을 발송
- 관리해야 할 인프라도 없고, 작동도 잘하고 확장도 매우 잘 됨

## Thumbnail Generation Flow

![myb](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/myb6.png)

- 이미지를 업로드하면 섬네일을 생성
- 클라이언트는 사진을 CloudFront에 업로드하고 CloudFront는 사진을 Amazon S3 버킷으로 전달하게 됨
	- 이런 방식을 S3 Transfer Acceleration이라고 함
	- 사진은 바로 S3로 올리거나 S3 Transfer Acceleration을 이용하거나 할 수 있음
- S3에 파일이 추가되면 람다 함수를 호출하게 함
	- S3가 람다를 호출할 수 있고, 람다는 섬네일을 생성하고 섬네일을 S3 버킷에 넣는데, 다른 버킷에 넣을 수도 있음
	- Amazon S3는 SQS와 SNS를 호출할 수도 있음
		- 선택사항, SQS나 SNS로 원하는 작업을 하면 됨
		- Amazon S3는 람다, SQS, SNS를 호출할 수 있고 솔루션을 자유롭게 구성할 수도 있고, 또 서버리스로 구성할 수도 있고, 편한 방법으로 구성할 수 있음

## 정리

- 서버리스로 구성했고, 글로벌하게 확장도 가능
- 정적 콘텐츠를 CloudFront와 S3로 배포하는 방법을 살펴봤고 서버리스인 REST API를 붙이는 방법도 봤음
- 이번에는 API가 퍼블릭이어서 Cognito가 필요 없었음
- 데이터를 글로벌로 서빙하기 위해 Dynamo DB를 붙임
	- 오로라 글로벌 데이터베이스를 사용할 수도 있었지만 이 사례가 서버리스가 아닌 프로비저닝 오로라를 사용했어야 했을 것
- Dynamo DB 스트림을 활용함
	- 사용자 테이블에 발생하는 변경사항을 알려주는 용도, 람다 함수 호출 용도
- 호출되는 람다 함수엔 IAM 역할을 부여해서 SES, 즉 simple email service를 이용하도록 함
	- 서버리스 방식으로 이메일을 발송하기 위한 방법
- S3는 이벤트 알림을 위해 SQS, SNS, 람다를 호출할 수 있는 것도 다룸
