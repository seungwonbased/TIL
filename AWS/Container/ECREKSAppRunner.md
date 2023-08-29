# Amazon ECR

![eea](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/eea1.png)

- Amazon ECR은 Elastic Container Registry의 줄임말로 AWS에 도커 이미지를 저장하고 관리하는 데 사용됨
	- 지금까지 Docker Hub 등의 온라인 저장소를 활용했는데 이미지를 Amazon ECR에도 저장할 수 있는 것
- ECR의 두 가지 옵션
	1. 계정에 한해 이미지를 비공개로 저장하는 것
		- 여러 계정으로 설정할 수도 있음
	2. 퍼블릭 저장소를 사용해 Amazon ECR Public Gallery에 게시하는 방법
- ECR은 Amazon ECS와 완전히 통합되어 있고 이미지는 백그라운드에서 Amazon S3에 저장됨
- ECR 저장소에 여러 도커 이미지가 있는데, ECS 클러스터의 EC2 인스턴스에 이미지를 끌어오기 위해서는 EC2 인스턴스에 IAM 역할을 지정하면 됨
	- IAM 역할이 도커 이미지를 인스턴스에 끌어옴
	- ECR에 대한 모든 접근은 IAM이 보호하고 있음
	- ECR에 권한 에러가 생긴다면 정책을 살펴보면 됨
- EC2 인스턴스에 이미지를 끌어온 후에는 컨테이너가 시작됨
- ECS와 ECR이 이런 식으로 함께 작동
- Amazon ECR은 단순히 저장하는 리포지토리에 그치지 않고 이미지의 취약점 스캐닝, 버저닝 태그 및 수명 주기 확인을 지원
- 정리하자면, 도커 이미지를 저장할 때는 ECR을 기억
	- 시험에 출제될 수 있음

# Amazon EKS

- Amazon EKS는 Amazon Elastic Kubernetes Service의 약자
- AWS에 관리형 Kubernetes 클러스터를 실행할 수 있는 서비스
- Kubernetes는 오픈 소스 시스템으로 Docker로 컨테이너화한 애플리케이션의 자동 배포, 확장, 관리를 지원
- 컨테이너를 실행한다는 목적은 ECS와 비슷하지만 사용하는 API가 다름
- ECS는 오픈 소스가 아닌 반면 Kubernetes는 오픈 소스이고 여러 클라우드 제공자가 사용하므로 표준화를 기대할 수 있음
- EKS의 두 가지 실행 모드
	- EC2 시작 모드
		- EC2 인스턴스에서처럼 작업자 모드를 배포할 때 사용
	- Fargate 모드
		- EKS 클러스터에 서버리스 컨테이너를 배포할 때 사용
- 사용 사례
	- 회사가 온프레미스나 클라우드에서 Kubernetes나 Kubernetes API를 사용 중일 때 Kubernetes 클러스터를 관리하기 위해 Amazon EKS를 사용
	- 시험에 나올 수 있음
	- Kubernetes는 클라우드 애그노스틱으로 Azure, Google Cloud 등 모든 클라우드에서 지원됨
	- 따라서 클라우드 또는 컨테이너 간 마이그레이션을 실행하는 경우 Amazon EKS가 간단한 솔루션이 될 수 있음

## Amazon EKS - Diagram

![eea](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/eea2.png)

- VPC와 세 AZ가 있고 각 AZ는 퍼블릭 및 프라이빗 서브넷으로 나뉨
- EKS 작업자 노드를 생성하면 EC2 인스턴스가 구성될 것임
- 각 노드는 EKS 포드를 실행
	- ECS 태스크와 유사하지만, 이름에서 포드(Pod)를 발견하면 Amazon Kubernetes와 관련된 거라고 생각하면 됨
- EKS 포드가 실행되는 EKS 노드는 오토 스케일링 그룹으로 관리할 수 있음
- ECS와 유사하게 EKS 서비스나 Kubernetes 서비스를 노출할 때는 프라이빗 로드 밸런서나 퍼블릭 로드 밸런서를 설정해 웹에 연결해야 함

## Amazon EKS - Node Types

- Amazon EKS의 여러 노드 유형
- 관리형 노드 그룹
	- AWS로 노드, 즉 EC2 인스턴스를 생성하고 관리
	- 노드는 EKS 서비스로 관리되는 오토 스케일링 그룹의 일부
	- 온디맨드 인스턴스와 스팟 인스턴스를 지원
- 자체 관리형 노드
	- 사용자 지정 사항이 많고 제어 대상이 많은 경우 직접 노드를 생성하고 EKS 클러스터에 등록한 다음 ASG의 일부로 관리해야 함
	- 사전 빌드된 AMI인 Amazon EKS 최적화 AMI를 사용하면 시간을 절약할 수 있음
		- 자체 AMI를 빌드해도 되지만 복잡할 수 있음
	- 이 역시 온디맨드 인스턴스와 스팟 인스턴스를 지원
- Fargate 모드
	- 노드를 원치 않는 경우에는 Amazon EKS가 지원하는 Fargate 모드를 선택하면 유지 관리도 필요 없고 노드를 관리하지 않아도 되며 Amazon EKS에서 컨테이너만 실행하면 됨

## Amazon EKS - Data Volumes

![eea](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/eea3.png)

- Amazon EKS 클러스터에 데이터 볼륨을 연결하려면 EKS 클러스터에 스토리지 클래스 매니페스트를 지정해야 함
- **컨테이너 스토리지 인터페이스(CSI)** 라는 규격 드라이버를 활용하는데 시험에 나올 만한 키워드임
- 지원하는 볼륨
	- Amazon EBS
	- Fargate 모드가 작동하는 유일한 스토리지 클래스 유형인 Amazon EFS
	- Amazon FSx for Lustre
	- Amazon FSx for NetApp ONTAP

# AWS App Runner

![eea](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/eea4.png)

- AWS App Runner 서비스는 완전 관리형 서비스로 규모에 따라 웹 애플리케이션, API 배포를 도움
	- 이 서비스로는 누구나 AWS에 배포를 할 수 있음
	- 인프라나 컨테이너, 소스 코드 등을 알 필요가 전혀 없음
- 먼저 소스 코드나 Docker 컨테이너 이미지를 가지고 원하는 구성을 설정
	- vCPU의 수나 컨테이너 메모리의 크기 오토 스케일링 여부 상태 확인을 설정
	- 웹 애플리케이션이나 API에 들어갈 기본 설정을 설정하는 것
- 다음 작업은 자동으로 이뤄짐
	- App Runner 서비스가 웹 앱을 빌드하고 배포
	- 컨테이너가 생성되고 배포됨
- API나 웹 앱이 배포된 다음엔 URL을 통해 바로 액세스할 수 있음
- 이처럼 배후에서 어떤 작업이 이뤄지는지 전혀 몰라도 배포할 수 있음
- 배후에서 AWS 서비스가 사용되는 것이겠지만 사용자는 굳이 몰라도 빠른 배포를 할 수 있음
- App Runner의 장점
	- 오토 스케일링이 가능하고 가용성이 높으며 로드 밸런싱 및 암호화 기능을 지원
	- 애플리케이션, 즉 컨테이너가 VPC에 액세스할 수도 있어서 데이터베이스와 캐시 메시지 대기열 서비스에 연결할 수 있음
- 사용 사례
	- 빨리 배포해야 하는 웹 앱, API 그리고 마이크로서비스
	- 신속한 프로덕션 배포가 필요할 때
