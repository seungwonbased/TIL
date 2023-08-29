# Amazon ECS
## What is Docker?

- 도커는 앱 배포를 위한 소프트웨어 개발 플랫폼
	- 컨테이너 기술
- 컨테이너에 앱이 패키징되는데 컨테이너는 표준화되어있어서 아무 운영체제에나 실행할 수 있음
	- 다시 말해 앱이 컨테이너에 패키징되면 어느 운영체제에서든 같은 방식으로 실행됨
	- 아무 머신이나 상관 없이 호환성 문제가 없음
	- 행위 특성도 예측 가능해서 작업을 덜어주고 유지 및 배포가 쉽고 언어, 운영체제, 기술에 상관 없이 실행이 가능
- 사용 사례
	- 마이크로서비스 아키텍처
	- 온프레미스에서 클라우드로 앱을 리프트-앤-시프트
	- 컨테이너를 실행하는 어떤 경우에도 사용할 수 있음

## Docker on an OS

![ecs](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/ecs1.png)

- 서버가 있음
	- EC2 인스턴스를 예로 들었는데, 어떤 유형의 서버든 똑같음
- 도커 에이전트를 실행하면 도커 컨테이너를 시작할 수 있음
- 첫 번째 도커 컨테이너는 Java 애플리케이션을 포함하고, 두 번째 도커 컨테이너에는 Node.js 애플리케이션 있다고 가정
- 다수의 도커 컨테이너가 동시에 실행될 수 있어서 Java 애플리케이션을 가진 여러 도커 컨테이너가 있을 수 있음
	- Node.js 애플리케이션을 가진 도커 컨테이너도 여러 개 있을 수 있음
	- 도커 내에서 MySQL 등의 데이터베이스도 실행 가능하니 아주 다용도로 활용됨
- 서버 관점에선 모두 도커 컨테이너

## Where are Docker images stored?

- 도커 이미지는 도커 리포지토리에 저장됨
- 여러 옵션
	- Docker Hub
		- 아주 유명한 퍼블릭 리포지토리로 많은 기술에 맞는 기본 이미지를 찾을 수 있음
		- Ubuntu나 MySQL과 같은 OS용 기본 이미지도 마찬가지
	- Amazon ECR
		- Private Repository: Amazon ECR
		- Public Repository: Amazon ECR Public Gallery

## Docker vs Virtual Machines

![ecs](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/ecs2.png)

- 도커와 가상 머신의 차이점
- 도커 역시 가상화 기술의 일종이긴 하지만 순전히 가상화 기술은 아님
- 리소스가 호스트와 공유되어 한 서버에서 다수의 컨테이너를 공유할 수 있음
- 가상 머신의 아키텍처
	- 인프라와 호스트 운영체제가 있으며, 그 위에 하이퍼바이저가 있고 앱과 Guest 운영 체제가 있음
		- EC2의 원리임
		- 다시 말해 EC2 머신은 하이퍼바이저에 실행되는 가상 머신
		- 그래서 Amazon이 EC2 인스턴스를 다양한 소비자에게 제공할 수 있으며 가상 머신의 EC2 인스턴스는 각자 분리되어 있음
			- 리소스를 공유하지 않음
- 도커 컨테이너
	- 인프라와 EC2 인스턴스 같은 호스트 OS가 있고 도커 Daemon 위에 많은 컨테이너가 있음
		- 도커 Daemon에서 가볍게 실행되는 컨테이너라 공존할 수 있음
	- 네트워킹이나 데이터 등을 공유할 수도 있음
	- 소위 말해 가상 머신보다 덜 안전하지만 하나의 서버에 많은 컨테이너를 실행할 수 있기 때문에 도커 컨테이너를 많이 사용

## Getting Started with Docker

![ecs](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/ecs3.png)

- 도커를 시작하려면 우선 Dockerfile을 작성해야 함
	- 도커 컨테이너를 구성하는 파일
- 베이스 도커 이미지에 몇 가지 파일을 추가해서 구축하면 도커 이미지가 됨
- 도커 이미지는 푸시(push)를 해서 도커 리포지토리에 저장할 수 있음
	- 퍼블릭 리포지토리인 Docker Hub에 푸시하거나 Amazon 버전의 도커 리포지토리인 Amazon ECR에 푸시
- 도커 리포지토리에서 이미지를 가져와서 실행하게 되는데 도커 이미지를 실행하면 도커 컨테이너가 되고 도커를 구축할 때 사용했던 코드를 실행할 것임

## Docker Containers Management on AWS

- Amazon ECS (Elastic Container Service)
	- 도커 관리를 위한 Amazon의 전용 플랫폼
- Amazon EKS (Elastic Kubernetes Service)
	- Kubernetes의 관리형 버전으로 오픈 소스 프로젝트
- AWS Fargate
	- Amazon의 서버리스 컨테이너 플랫폼
	- ECS와 EKS 둘 다 함께 작동할 수 있음
- Amazon ECR
	- 컨테이너 이미지를 저장하는 데 사용

## Amazon ECS - EC2 Launch Type

![ecs](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/ecs4.png)

- EC2 시작 유형(Launch Type)
- ECS는 AWS에서 컨테이너를 실행하면 ECS 클러스터에 이른바 ECS 태스크를 실행하는 것
- ECS 클러스터에는 EC2 시작 유형을 사용하면 EC2 인스턴스가 들어있을 것임
- EC2 시작 유형으로 EC2 클러스터를 사용할 때는 인프라를 직접 프로비저닝하고 유지해야 함
	- 즉 Amazon ECS 및 ECS 클러스터가 여러 EC2 인스턴스로 구성됨
- 이때 ECS 인스턴스는 특별하게 각각 ECS 에이전트(Agent)를 실행해야 함
	- 그럼 ECS 에이전트가 각각의 EC2 인스턴스를 Amazon ECS 서비스와 지정된 ECS 클러스터에 등록
- 이후에 ECS 태스크를 수행하기 시작하면 AWS가 컨테이너를 시작하거나 멈춤
	- 즉 새 도커 컨테이너가 생기면 도식에서 볼 수 있듯 시간에 따라 EC2 인스턴스에 지정됨
- ECS 태스크를 시작하거나 멈추면 자동으로 위치가 지정됨
- 도커 컨테이너는 미리 프로비저닝한 Amazon EC2 인스턴스에 위치함

## Amazon ECS - Fargate Launch Type

![ecs](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/ecs5.png)

- Fargate 시작 유형
- 마찬가지로 AWS에 도커 컨테이너를 실행하는데 이번에는 인프라를 프로비저닝하지 않아 관리할 EC2 인스턴스가 없음
	- 서버리스
	- 서버를 관리하지 않아 서버리스라 부르는데 서버가 없는 건 아님
- Fargate 유형은 ECS 클러스터가 있을 때 ECS 태스크를 정의하는 태스크 정의만 생성하면 필요한 CPU나 RAM에 따라 ECS 태스크를 AWS가 대신 실행
	- 즉 새 도커 컨테이너를 실행하면 어디서 실행되는지 알리지 않고 그냥 실행됨
	- 작업을 위해 백엔드에 EC2 인스턴스가 생성될 필요도 없음
	- 확장하려면 간단하게 태스크 수만 늘리면 됨
	- EC2 인스턴스를 관리할 필요가 없음
	- 시험에서는 서버리스인 Fargate를 사용하라는 게 자주 나옴
		- EC2 시작 유형보다 관리가 쉽기 때문

## Amazon ECS - IAM Roles for ECS

![ecs](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/ecs6.png)

- ECS 태스크의 IAM 역할
- EC2 시작 유형
	- 예시로 EC2 인스턴스가 도커에 ECS 에이전트를 실행한다고 가정
	- EC2 시작 유형을 사용한다면 EC2 인스턴스 프로파일을 생성할 것임
		- ECS 에이전트만이 EC2 인스턴스 프로파일을 사용하며 그 EC2 인스턴스 프로파일을 이용해 API 호출을 할 것
	- 그럼 인스턴스가 저장된 ECS 서비스가 CloudWatch 로그에 API 호출을 해서 컨테이너 로그를 보내고 ECR로부터 도커 이미지를 가져옴
		- Secrets Manager나 SSM Parameter Store에서 민감 데이터를 참고하기도 함
- ECS 태스크는 ECS 태스크 역할을 가짐
	- 이는 EC2와 Fargate 시작 유형에 모두 해당되며 두 개의 태스크가 있다면 각자에 특정 역할을 만들 수 있음
	- 도식의 모자처럼 태스크 A는 EC2 태스크 A 역할을 맡고 태스크 B는 EC2 태스크 B 역할을 맡는 것
- 역할을 다르게 하는 이유
	- 역할이 각자 다른 ECS 서비스에 연결할 수 있게 하기 때문
		- 예를 들어 EC2 태스크 A 역할은 태스크 A가 Amazon S3에 API 호출을 실행할 수 있도록 한다면 태스크 B 역할은 DynamoDB에 API 호출을 실행할 수 있도록 함
	- ECS 서비스의 태스크 정의에서 태스크의 역할을 정의
	- 이러한 EC2 인스턴스 프로파일 역할과 ECS 태스크 역할의 차이점을 기억해야 함

## Amazon ECS - Load Balancer Integrations

![ecs](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/ecs7.png)

- 로드 밸런서 통합
- EC2 시작 유형을 예로 들었지만 Fargate 시작 유형도 마찬가지임
- 예시
	- 여러 ECS 태스크들이 실행되며 ECS 클러스터 안에 있음
	- HTTP나 HTTPS 엔드 포인트로 태스크를 활용하기 위해 애플리케이션 로드 밸런서(ALB)를 앞에서 실행하면 모든 사용자가 ALB 및 백엔드의 ECS 태스크에 직접 연결됨
- ALB는 이 경우를 포함해, 대부분의 사용 사례를 지원하는 좋은 옵션
	- 한편 네트워크 로드 밸런서(NBL)는 처리량이 매우 많거나 높은 성능이 요구될 때만 권장
	- 구세대 Elastic Load Balancer(ELB)는 사용할 수는 있지만 권장하지는 않음
		- 고급 기능이 없을 뿐더러 ELB는 Fargate에 연결할 수 없기 때문
	- 반면 애플리케이션 로드 밸런서(ALB)는 Fargate와도 사용할 수 있음

## Amazon ECS - Data Volumes (EFS)

![ecs](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/ecs8.png)

- Amazon ECS의 데이터 지속성
- ECS 클러스터에 EC2 인스턴스와 Fargate 시작 유형 둘 다 구현했다고 가정
- EC2 태스크에 파일 시스템을 마운트해서 데이터를 공유하려고 함
- 그러면 Amazon EFS 파일 시스템을 사용하는 게 좋을 것임
	- 네트워크 파일 시스템이라 EC2와 Fargate 시작 유형 모두 호환이 되며 EC2 태스크에 파일 시스템을 직접 마운트할 수 있기 때문
	- 어느 AZ에 실행되는 태스크든 Amazon EFS에 연결되어 있다면 데이터를 공유할 수 있고 원한다면 파일 시스템을 통해 다른 태스크와 연결할 수 있기 때문
	- 따라서 Fargate로 서버리스 방식으로 ECS 태스크를 실행하고 파일 시스템 지속성을 위해서는 Amazon EFS를 사용하는 게 가장 좋음
	- EFS 역시 서버리스이기 때문에 서버를 관리할 필요 없고 미리 비용을 지불
		- 미리 프로비저닝하기만 하면 바로 사용 가능
- 사용 사례
	- EFS와 ECS를 함께 사용해서 다중 AZ가 공유하는 컨테이너의 영구 스토리지
- 하나 알아 두어야 할 것
	- Amazon S3는 ECS 태스크에 파일 시스템으로 마운트될 수 없음

## ECS Service Auto Scaling

- ECS 서비스 오토 스케일링
- 서비스에 ECS 태스크 수를 자동으로 늘리거나 줄일 수 있음
- AWS의 Auto Scaling이라는 서비스를 사용하면 세 개의 지표에 대해 확장이 가능
	- ECS 서비스의 CPU 사용률
	- 메모리 사용률
		- 즉 ECS 서비스의 RAM
	- ALB 관련 지표인 타겟당 요청 수를 확장할 수 있음
	- 세 개의 지표만 기억하면 다양한 오토 스케일링을 설정할 수 있음
- Target Tracking
	- 특정한 CloudWatch 지표를 타겟으로 스케일링
- Step Scaling
	- 특정한 CloudWatch 경보를 기반으로 스케일링
- Scheduled Scaling
	- 특정한 날짜나 시간을 기반으로 스케일링
- EC2 시작 유형이라면 태스크 레벨에서의 ECS 서비스 확장이 EC2 인스턴스 클러스터의 확장과 다르다는 사실을 기억
	- 따라서 EC2 오토 스케일링이 필요하지 않다면, 즉 백엔드에 EC2 인스턴스가 없다면 Fargate를 사용하는 것이 서비스 오토 스케일링에 도움이 될 것임
		- 전부 서버리스기 때문
		- 시험에서도 Fargate 사용을 권장

## EC2 Launch Type - Auto Scaling EC2 Instances

- EC2 시작 유형에서 백엔드의 EC2 인스턴스를 스케일링하는 여러 방법이 있음
- 오토 스케일링 그룹
	- CPU 사용률에 따라 ASG를 확장한다고 하면 CPU 사용률이 급등할 때 EC2 인스턴스를 추가할 수 있을 것임
	- 아니면 더 최신 기능인 ECS 클러스터 용량 공급자를 사용할 수 있음
- ECS 용량 공급자(Capacity Provider)
	- 새 태스크를 실행할 용량이 부족하면 자동으로 ASG를 확장
	- Capacity Provider는 오토 스케일링 그룹과 함께 사용되며 RAM이나 CPU가 모자랄 때 EC2 인스턴스를 추가
	- 이 방법을 사용하는 게 좋음
- 오토 스케일링 그룹과 ECS 클러스터 용량 공급자 중에 EC2 시작 유형의 경우 EC2 클러스터 용량 공급자를 사용

## ECS Scaling - Service CPU Usage Example

![ecs](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/ecs9.png)

- 서비스 A에 태스크 두 개가 실행 중이고 CPU 사용량이 나와있음
- 이 서비스는 AWS 애플리케이션 오토 스케일링으로 자동 확장됨
- 만약 사용자가 훨씬 많아져서 CPU 사용량이 급증하면 ECS 서비스 레벨에서 CPU 사용량을 모니터링하는 CloudWatch 지표가 CloudWatch 알람을 트리거
- 이어서 이 알람이 오토 스케일링을 또 트리거하면 ECS 서비스의 희망 용량이 증가하고 새 태스크가 생성
- 또 EC2 시작 유형에서 실행되는 서비스라면 ECS 용량 공급자가 EC2 인스턴스를 추가해 ECS 클러스터를 확장할 것

# Amazon ECS Solutions Architectures
## ECS tasks invoked by Event Bridge

![ecs](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/ecs10.png)

- EventBridge로 작동되는 ECS 태스크
- Amazon ECS 클러스터가 있고, 이와 함께 Fargate와 S3 버킷이 있는 예시
	- 사용자가 S3 버킷에 객체를 업로드
	- 이때 S3가 Amazon EventBridge와 통합되어 이벤트를 모두 보낼 수 있음
	- Amazon EventBridge에는 ECS 태스크를 실행하는 규칙을 만들 수 있음
	- 그럼 ECS 태스크가 생성되고 관련 ECS 태스크 역할이 주어질 것임
	- 태스크는 객체를 받아 처리하고 Amazon DynamoDB에 결과를 보내게 됨
		- ECS 태스크 역할이 결합된 덕분
	- 이렇게 도커 컨테이너를 이용해 S3 버킷으로부터 이미지나 객체를 처리하는 서버리스 아키텍처를 완성
- Amazon EventBridge와 ECS Fargate 모드, S3와 소통하는 ECS 태스크 역할, 또 Amazon DynamoDB를 사용함

## ECS tasks invoked by Event Bridge Schedule

![ecs](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/ecs11.png)

- EventBridge Schedule(일정)
- Fargate와 EventBridge를 사용하는 ECS 클러스터가 있다고 가정합
	- 한 시간마다 트리거되는 규칙의 일정을 만들면 Fargate에서 ECS 태스크를 실행
		- 즉 한 시간마다 Fargate 클러스터에 새 태스크가 생성됨
	- 태스크에는 자유롭게 Amazon S3에 접근 가능한 ECS 태스크 역할 등을 생성하면 태스크, 도커 컨테이너, 프로그램이 Amazon S3의 파일에 대해 한 시간마다 배치(Batch) 처리 (일괄 처리)
	- 이때 아키텍처는 서버리스

## ECS - SQS Queue Example

![ecs](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/ecs12.png)

- SQS 대기열
- ECS의 서비스에 두 개의 태스크가 있고 SQS 대기열에 메시지를 전송했다고 가정
	- 서비스 자체적으로 SQS 대기열로부터 메시지를 가져와서(poll) 처리
	- 여기에 ECS 서비스 오토 스케일링을 활성화하면 SQS 대기열에 메시지가 많아질수록 ECS 서비스에 태스크가 많아짐
