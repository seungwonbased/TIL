# Solutions Architectures: MyTodoList
## Mobile App: MyTodoList

- MyTodoList라는 모바일 애플리케이션
- 요구 사항
	- HTTPS 엔드 포인트가 있는 REST API가 노출돼야 함
	- 서버리스 아키텍처
	- 사용자가 원한다면 스스로 데이터를 관리하도록 S3에 있는 폴더와 직접 상호작용이 가능해야 함
	- 사용자가 관리형 서버리스 서비스로 인증할 수 있어야 함
	- 사용자가 할 일을 읽고 쓸 수 있지만 대부분 읽기를 많이 하니까 관련 성능이 필요할 것임
	- 데이터베이스 계층은 확장할 수 있도록 구축해 읽기 처리량을 높여야 함

## REST API Layer

![mtl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/mtl1.png)

- 모바일 클라이언트
- REST, HTTPS를 얘기했으니까 Amazon API Gateway를 사용
- 일반적인 서버리스 API 방식으로 API Gateway가 람다 함수를 호출해서 확장을 허용하고 서버리스 인프라를 쓰도록 할 것
- Amazon Lambda는 데이터베이스에서 할 일을 저장하거나 읽어 내야 함
	- 서버리스이면서 확장이 잘 되는 건 DynamoDB
	- DynamoDB를 백엔드로 사용
- 인증 계층이 있어야 함
	- Amazon Cognito 같은 서버리스 기술을 쓸 수 있음
	- 모바일 클라이언트가 Cognito에 연결하고 인증하면 API Gateway는 Cognito와 함께 인증을 확인해 줄 것임
- 방금 생성한 건 전형적인 서버리스 API

## Giving Users Access to S3

![mtl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/mtl2.png)

- Amazon S3 버킷에 사용자 액세스 권한을 주려고 함
- Amazon Cognito에 인증을 요청하는 모바일 클라이언트가 있음
	- Cognito는 AWS STS를 통해 임시 자격 증명을 제공할 수 있음
	- 그리고 이 자격 증명을 모바일 클라이언트에게 반환
- 이 자격 증명은 클라이언트가 Amazon S3에서 파일을 저장하고 회수하며 S3에서 전용 공간에 액세스하도록 허용
	- 시험 단골 문제인데, 이를 어떻게 처리할지 묻는다면 오답은 이걸 고르면 됨
		- 오답: 'AWS 사용자 자격 증명을 모바일 클라이언트에 저장한다'
		- 이건 올바른 방법이 아님
		- Amazon Cognito와 STS Amazon S3를 임시 자격 증명에 사용해야 함

## High Read Throughput, Static Data

![mtl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/mtl3.png)

- 애플리케이션이 확장됨
- 사용자가 늘어나기 시작하고, 패턴을 살펴보면 읽기 처리량이 아주 높다는 걸 알 수 있는데 RCU는 많고 할 일이 많이 바뀌지는 않으니까 자주 편집할 필요는 없음
- 읽기 처리량을 늘리면서 전체 비용을 줄이려면 캐싱 계층으로 DAX를 사용하면 됨
	- DynamoDB 이전에 DynamoDB DAX를 사용해 캐싱 계층을 갖추는 것
	- 읽기를 많이 하는데, 읽어 낸 내용이 DAX 캐시에 저장돼 DynamoDB 읽기 용량 단위가 많이 필요하지 않게 되고 확장이 용이하며 비용도 줄 것임
	- 아주 많은 내용을 읽어 캐시에 저장하니까 서버리스 방식으로 꾸준히 아키텍처를 개선하기에 적합

## Caching at the API Gateway

![mtl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/mtl4.png)

- 캐싱에는 다른 방법도 있음
- DAX도 사용 가능하지만, 캐시에 응답을 저장할 때 Amazon API Gateway 레벨에서 진행할 수도 있는데, 답이 거의 바뀌지 않으면서 API 라우트에 대한 응답을 캐시에 저장하는 경우 유용한 방법
- 이런 아키텍처는 서버리스가 어떻게 이뤄지고 캐싱이 어디서 나타나는지 보여 줘서 흥미로움
- 따로 관리할 게 없음
	- 사용량에 따라 비용만 지불하면 됨
	- 데이터베이스 관리는 생각 안 해도 됨
		- AWS가 전부 대신 해 주기 때문

## 정리

- HTTPS, API Gateway Lambda, DynamoDB를 사용하는 전형적인 서버리스 REST API 아키텍처를 설계함
- 임시 자격 증명을 생성하는 데 제한된 정책에서 추가 버킷에 액세스를 제공하는 STS와 함께 Cognito를 사용해 봄
	- Cognito를 이용하면 똑같은 애플리케이션 패턴으로 애플리케이션이 DynamoDB나 Lambda에 직접 액세스할 수도 있음
	- 아주 흔한 패턴
- DAX로 DynamoDB 상에서 읽은 내용을 캐시에 저장하면 손쉽게 활성화하면서 성능을 개선하고 비용을 절감할 수 있음
- REST 요청 캐싱은 응답이 고정된 경우 API Gateway 레벨에서 가능
- 보안은 전부 Cognito로 가능한데, Cognito는 API Gateway와 직접 통합되어 있음
