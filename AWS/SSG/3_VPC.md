# VPC (Virtual Private Cloud) 🌟

- AWS 사용자 계정 전용 가상 네트워크
- AWS 클라우드에서 다른 가상 네트워크와 논리적으로 분리
- 한 AWS 리전 안에서만 존재할 수 있고, 한 리전에 만든 VPC는 다른 리전에서 보이지 않음
- 생성시 RFC 1918에 지정된 프라이빗 주소 체계 사용을 권장
	- 10.0.0.0/8   ⇒ 10.0.0.0 - 10.255.255.255
	- 172.16.0.0/12  ⇒ 172.16.0.0 - 172.31.255.255
	- 192.168.0.0/16 ⇒ 192.168.0.0 - 192.168.255.255

## VPC 구성 옵션

> https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/create-vpc.html

- 가용 영역
- CIDR 블록
- DNS 옵션
- 인터넷 게이트웨이
- 이름
- NAT 게이트웨이
- 라우팅 테이블
- 서브넷
- Tenancy

## 기본 VPC (Default VPC)

> https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/default-vpc.html

- Amazon VPC를 사용하기 시작하는 경우 각 AWS 리전에 기본 VPC가 있음
- 기본 VPC는 각 가용 영역의 퍼블릭 서브넷, 인터넷 게이트웨이 및 DNS 확인 활성화 설정과 함께 제공됨
- 따라서 기본 VPC로 Amazon EC2 인스턴스를 즉시 시작할 수 있음
- 기본 VPC에서 Elastic Load Balancing, Amazon RDS, Amazon EMR 같은 서비스를 사용할 수도 있음
- 대부분이 허용되어 있기 때문에 보안적으로 권장되지 않음

### 기본 VPC 구성 요소

- 아래 리소스는 사용자 대신 아마존이 생성
	- 사용자가 이러한 작업을 수행하는 것이 아니므로 여기에는 IAM 정책이 적용되지 않음
- IPv4 CIDR 블록의 크기가 `/16`인 VPC를 만듬 (`172.31.0.0/16`)
	- 이는 최대 65,536개의 프라이빗 IPv4 주소를 제공
- 각 가용 영역에 크기 `/20`의 기본 서브넷을 생성
	- 이렇게 하면 서브넷당 최대 4,096개의 주소가 제공되며, 그중 몇 개는 내부용으로 예약되어 있음
- [인터넷 게이트웨이](https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/VPC_Internet_Gateway.html)를 만들어 기본 VPC에 연결
- 기본 라우팅 테이블에 모든 트래픽(`0.0.0.0/0`)이 인터넷 게이트웨이로 전달되는 경로를 추가
- 기본 보안 그룹을 만들어 기본 VPC와 연결
	- SSH (22) 허용
- 네트워크 ACL(액세스 제어 목록)을 생성하여 기본 VPC와 연결
- AWS 계정에서 설정된 기본 DHCP 옵션을 기본 VPC와 연결

## Subnet

- VPC 내 논리적인 구분
- 인스턴스는 서브넷 안에 위치
	- EC2 인스턴스를 배치하는 장소
	- 한 번 서브넷에 인스턴스를 생성하면 다른 서브넷으로 옮길 수 없음
		- 인스턴스를 종료하고 다른 서브넷에 새 인스턴스를 만들어야 함
- 인스턴스를 서로 격리하고, 인스턴스 간의 트래픽 흐름을 제어하고, 인스턴스를 기능 별로 묶을 수 있음
- **서브넷은 하나의 가용 영역 (AZ) 내에서만 존재할 수 있음**
- 서브넷 CIDR 블록
	- VPC의 일부로, VPC 내에서는 유니크해야 함
	- 모든 서브넷에서 처음 4개의 IP와 마지막 한 개는 예약되어 있으므로 인스턴스에 할당할 수 없음
		- 서브넷 CIDR이 172.16.100.0/24인 경우
			- 사용 불가능한 IP
				- 172.16.100.0: 네트워크 주소
				- 172.16.100.1: AWS에서 VPC 라우터용으로 예약
				- 172.16.100.2: DNS 서버 주소
				- 172.16.100.3: AWS에서 나중에 사용하려고 예약
				- 172.16.100.255: 네트워크 브로드캐스트 주소

### Public vs Private subnet

- Public Subnet
	- 서브넷이 인터넷 게이트웨이로 향하는 라우팅이 있는 라우팅 테이블과 연결됨
- Private Subnet
	- 서브넷이 인터넷 게이트웨이로 향하는 라우팅이 있는 라우팅 테이블과 연결됨


- 서브넷이 인터넷 게이트웨이로 향하는 라우팅이 없는 라우팅 테이블과 연결된 경우

## ENI (Elastic Network Interface)

- 물리 서버의 네트워크 인터페이스(네트워크 카드)와 같은 기능을 수행
- VPC에서 가상 네트워크 카드를 나타내는 논리적 네트워크 구성 요소

## IGW (Internet Gateway)

- 퍼블릭 IP 주소를 갖는 인스턴스가 인터넷에 연결할 수 있도록 기능을 제공
- 처음 VPC를 만들면 인터넷 게이트웨이가 연결되어 있지 않으므로, 직접 인터넷 게이트웨이를 만들고 VPC와 연결해야 함
	- 기본 VPC는 인터넷 게이트웨이와 연결되어 있음
- 하나의 VPC는 하나의 IGW만 연결할 수 있음

## Routing

- VPC의 **네트워크 트래픽을 전달할 위치**를 결정하는 데 사용되는 규칙
- 트래픽을 전달할 IP 주소 범위(대상 주소)와 트래픽을 전송할 **게이트웨이, 네트워크 인터페이스 또는 연결(대상)** 을 지정
- VPC는 소프트웨어 함수로 구성되어 있는 IP 라우팅을 구현
	- 사용자는 라우팅 테이블만 관리하면 됨

### Routing Table

#### 라우팅 테이블
- 라우팅의 집합으로, 서브넷과 연결할 수 있음

#### 기본 라우팅 테이블
- VPC와 함께 자동으로 제공되는 라우팅 테이블
	- 기본 VPC인 경우, local 및 igw로의 라우팅을 포함
	- 기본 VPC가 아닌 경우, local 라우팅만 포함
- 예시

|대상 주소 (Target Address)|대상 (Target)|
|:---:|:---:|
|172.31.0.0/16|local|
|0.0.0.0/0|igw-0b7313723843100f9|

- 0.0.0.0: 인터넷 게이트웨이, 인터넷 상의 모든 호스트 IP를 기반
- 라우팅할 위치와 가장 근접하게 일치하는 항목을 기반으로 라우팅을 처리
- **라우팅 테이블의 항목들 간의 순서는 중요하지 않음**
- 198.51.100.50으로 패킷을 보내려고 하면
		- IGW로 라우팅됨
- 172.31.10.10으로 패킷을 보내려고 하면
	- local로 라우팅됨

## 보안 그룹 (Security Group)

- 방화벽과 같은 기능을 제공
	- 방화벽: IP와 Port를 기반으로 접근 제어
- **인스턴스의 ENI에 연결**되어 송수신하는 트래픽을 제어
- 모든 ENI는 최소 한 개 이상의 보안 그룹과 연결되어야 하고, 보안 그룹은 여러 ENI와 연결될 수 있음
- 생성할 때 보안 그룹 이름, 설명, 포함될 VPC를 지정하고, 생성 후에 인바운드, 아웃바운드 규칙을 지정
	- 트래픽을 허용
- **상태 저장 방화벽 역할**
	- 보안 그룹이 트래픽을 한 방향으로 전달하도록 허용할 때, 반대 방향에 응답 트래픽을 지능적으로 허용
	- 특정 포트로 들어오는 응답 트래픽은 해당 포트에서 나가는 트래픽으로 허용됨
		- 인바운드만 정의하면 아웃바운드는 따로 지정하지 않아도 됨

## NACL (Network Access Control List)

- 보안 그룹과 유사
- 원본 또는 대상 주소의 CIDR, 프로토콜, 포트를 기반으로 트래픽을 인바운드, 아웃바운드 규칙으로 제어
	- 방화벽과 같은 역할
- 일반적으로 각 서브넷은 하나의 NACL에 연결됨
	- 이 NACL이 해당 서브넷의 인바운드 및 아웃바운드 트래픽을 제어
	- 하나의 NACL로 서브넷 내에서 필요한 모든 트래픽을 관리하는 것이 보편적인 구성
- VPC에는 삭제할 수 없는 기본 NACL이 있음
- **서브넷에 연결**되어 해당 서브넷과 송수신되는 트래픽을 제어
- **상태 비저장**
	- NACL을 통과한 연결 상태를 추적하지 않음
	- 모든 인바운드와 아웃바운드 트래픽의 허용 규칙을 별도로 작성해야 함
- NACL 규칙은 보안 그룹보다 낮은 우선 순위를 가짐
	- 보안 그룹과 충돌하면 보안 그룹의 규칙이 우선 적용됨
- 규칙을 적용할 때 규칙 번호의 오름차순으로 처리
	- **순서 중요**

## 보안 그룹과 NACL 비교

|보안 그룹|네트워크 ACL|
|---|---|
|인스턴스 레벨에서 운영|서브넷 레벨에서 운영|
|인스턴스와 연결된 경우에만 인스턴스에 적용|연결된 서브넷에서 배포된 모든 인스턴스에 적용<br>(보안 그룹 규칙이 지나치게 허용적일 경우 추가 보안 계층 제공)|
|허용 규칙만 지원|허용 및 거부 규칙 지원|
|트래픽 허용 여부를 결정하기 전에 모든 규칙을 평가|트래픽 허용 여부를 결정할 때 가장 낮은 번호의 규칙부터 순서대로 규칙을 평가|
|상태 저장: 규칙에 관계없이 반환 트래픽이 허용됨|상태 비저장: 반환 트래픽이 규칙에 따라 명시적으로 허용되어야 함|

