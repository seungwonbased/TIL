# 개요

## 확장성 (Scalability)

### 의미

- 애플리케이션 시스템이 조정을 통해 더 많은 양을 처리할 수 있음을 의미

### 수직 확장성 (Vertical scalability)

- 인스턴스의 크기를 확장하는 것
- 예시
	- 주니어 개발자 → 시니어 개발자처럼 Up-scaling
	- EC2 인스턴스에서 t2.micro → t2.large로 Up-scaling
- 언제 사용하는가?
	- 데이터베이스와 같이 분산되지 않은 시스템에서 흔히 사용
	- RDS, ElastiCache
- 확장할 수 있는 하드웨어 한계가 있음

### 수평 확장성 (Horizontal scalability, Elasticity)

- 애플리케이션 인스턴스나 시스템의 수를 늘리는 방법
- 예시
	- 주니어 개발자 한 명 → 주니어 개발자 세 명
	- EC2 인스턴스 한 개 → 다섯 개
- 수평 확장을 했다는 건 분배 시스템이 있다는 것을 의미
	- 웹이나 현대 애플리케이션은 대부분 분배 시스템으로 이뤄짐
	- 아닌 것도 있음

## 고가용성 (High availability)
### 의미

- 애플리케이션 또는 시스템을 적어도 둘 이상의 AWS의 AZ나 데이터 센터에서 가동 중이라는 것을 의미
- 종종 수평 확장과 같이 사용되는 개념 (항상 그런 것은 아님)

### 목표

- 데이터 센터에서의 손실에서 살아남는 것
- 센터 하나가 멈춰도 계속 작동이 가능하게끔 하는 것

### Passive, active

- Passive: Active 시스템의 장애 시 요청을 처리할 수 있는 시스템
	- RDS multi AZ를 갖추고 있다면 Passive high availability를 확보한 것
- Active: 평상시 요청을 받아 처리할 수 있는 구동 중의 시스템
	- 수평 확장을 하는 경우 Active high availability를 확보한 것

## High Availability & Scalability for EC2

- 수직 확장 → 인스턴스 크기 조정 (Scale up / down)
	- From t2.nano → to u-12tv1.metal
- 수평 확장 → 인스턴스 숫자 조정 (Scale out (늘림) / in (줄임))
	- Auto scaling group
	- Load balancer
- 고가용성 → 동일 애플리케이션의 동일 인스턴스를 다수의 AZ에 걸쳐 실행하는 경우를 의미
	- Auto scaling group (multi AZ가 활성화 된)
	- Load balancer (multi AZ가 활성화 된)

# Load Balancing

![LB1](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/LB1.png)

- Load balancer: 서버 혹은 서버셋으로 트래픽을 백엔드나 다운스트림 EC2 인스턴스 또는 서버들로 전달하는 역할

## 사용하는 이유

- 부하를 다수의 다운스트림 인스턴스로 분산하기 위함
- 애플리케이션에 DNS를 노출시켜 다운스트림 인스턴스의 장애를 원활히 처리
	- 로드 밸런서가 상태 확인 매커니즘으로 어떤 인스턴스로 트래픽을 보낼 수 없는지 확인해 줌
- 인스턴스의 상태를 정기적으로 체크 (Health check)
- SSL 종료 가능 → 암호화된 HTTPS을 가질 수 있음
- 쿠키로 고착성을 강화할 수 있음
- 영역에 걸친 고가용성을 가짐
- 클라우드 내에서 개인 트래픽과 공공 트래픽을 분리 가능

## Elastic Load Balancer (ELB)

- 관리형 로드 밸런서
	- AWS가 관리, 어떤 경우에도 작동할 것을 보장
	- AWS가 업그레이드와 유지 관리 및 고가용성을 책임짐
	- 로드 밸런서의 작동 방식을 수정할 수 있게끔 일부 구성 노브도 제공
- 무조건 쓰는 편이 좋음
	- 자체 로드 밸런서를 마련하는 것보다 저렴
	- 자체 로드 밸런서를 직접 관리하려면 확장성 측면에서 굉장히 번거로움
- 다수의 AWS 서비스와 통합되어 있음
	- EC2 인스턴스, Auto scaling groups, Amazon ECS
	- AWS Certificate Manager (ACM), CloudWatch
	- Route 53, AWS WAF, AWS Global Accelerator
	- 등등

## Health checks

- 상태 확인은 ELB가 EC2 인스턴스의 작동이 올바르게 되고 있는지의 여부를 확인하기 위해 사용
	- 트래픽 요청에 정상적으로 응답이 가능한 지 체크
- 포트와 라우트에서 이뤄짐

![LB2](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/LB2.png)

- 만약 200(OK)로 응답하지 않는다면 인스턴스 상태가 좋지 않다고 기록됨

## Types of load balancer on AWS

- Classic Load Balancer (v1 - old generation) - 2009 - CLB
	- HTTP, HTTPS, TCP, SSL (secure TCP)
	- AWS는 이제 이 로드 밸런서의 사용을 권장하지 않음
- Application Load Balancer (v2 - new generation) - 2016 - ALB
	- HTTP, HTTPS, WebSocket
- Network Load Balancer (v2 - new generation) - 2017 - NLB
	- TCP, TLS (secure TCP), UDP
- Gateway Load Balancer - 2020 - GWLB
	- Operates at layer-3 (Network layer) - IP protocol
- 결론적으로 더 많은 기능을 가지고 있는 신형 로드 밸런서를 사용하는 것이 권장됨
- 일부 로드 밸런서들은 내부에 설정될 수 있어 네트워크에 Private 접근이 가능
- 일부 로드 밸런서들은 외부에 설정될 수 있어 웹 사이트와 공공 애플리케이션 모두에 사용이 가능

## Load balancer security groups

![LB3](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/LB3.png)

- 유저는 HTTP나 HTTPS를 사용해 어디서든 로드 밸런서에 접근 가능
- EC2 인스턴스는 로드 밸런서를 통해 들어오는 트래픽만을 허용해야 하기 때문에 보안 그룹 규칙의 Source는 IP 범위가 아니라 로드 밸런서의 보안 그룹이 됨

→ EC2의 보안 그룹을 로드 밸런서의 보안 그룹으로 연결해 강력한 보안 정책

# Classic Load Balancer

- 이제 AWS에서 지원되지 않으며, 콘솔에서 사용 불가능
- 시험에서도 관련 레퍼런스가 모두 제외되었으므로 넘어가면 됨

# Application Load Balancer

<aside> 💡 Routing: 네트워크에서 최적의 경로를 선택하는 프로세스

</aside>

- 7-layer, 즉 애플리케이션 계층 HTTP 전용 로드 밸런서
- 머신 간 다수 HTTP 애플리케이션의 라우팅에 사용 → 이런 머신들은 Target group이라는 그룹으로 묶임
- 동일 EC2 인스턴스 상의 여러 애플리케이션에 부하를 분산 (예: 컨테이너, ECS 사용)
- HTTP/2와 WebSocket을 지원
- 리다이렉트 지원 (ex: From HTTP to HTTPS)
- 경로 라우팅 지원 (타겟 그룹 종류들)
	- URL 대상 경로에 기반한 라우팅 ([example.com/users](http://example.com/users) & [example.com/posts](http://example.com/posts))
	- URL의 호스트 이름에 기반한 라우팅 ([one.exmaple.com](http://one.exmaple.com) & [other.example.com](http://other.example.com))
	- 쿼리 문자열과 헤더에 기반한 라우팅 ([example.com/users?id=123](http://example.com/users?id=123))
- ALB는 마이크로 서비스나 컨테이너 기반 애플리케이션에 가장 좋은 로드 밸런서 (ex: Docker, Amazon ECS)
	- 포트 매핑 기능이 있어서 ECS 인스턴스 동적 포트 리다이렉션을 가능하게 해줌

## Target groups

<aside> 💡 Target group: 대상을 오토 스케일링 할 수 있는 단위

</aside>

- 타겟 그룹이 될 수 있는 것
	- EC2 인스턴스 (can be managed by an Auto Scaling Group) - HTTP
	- ECS 작업 (ECS 자기 자신에 의해 관리됨) - HTTP
	- Lambda functions - HTTP 요청이 JSON 이벤트로 번역됨, Serverless의 기반
	- IP address - must be private IP addresses
- ALB는 여러 타겟 그룹으로 라우팅 가능
- Health check는 타겟 그룹 레벨에서 이뤄짐

## 작동 방식

### - HTTP based traffic

![LB4](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/LB4.png)

- 두 개의 독립된 마이크로 서비스가 서로 다른 작업을 처리
	- user 애플리케이션
	- search 애플리케이션
- 애플리케이션이 URL에서 사용되는 라우팅에 기반해 각 대상 그룹으로 지능적으로 라우팅하는 방법을 알고 있는 동일 애플리케이션 로드 밸런서의 뒤에 있음

### - Query string / parameter routing

![LB5](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/LB5.png)

- 예를 들어 ALB를 통해 요청을 처리하는 애플리케이션이 있다고 가정
- 첫 번째 타겟 그룹은 모바일 기반 트래픽 전체를 담당, 두 번째 타겟 그룹은 데스크탑 기반 트래픽 전체를 담당
- 클라이언트가 사용하려는 URL에 ?Platform=Mobile이 있는 경우 타겟 그룹 1로 리다이렉팅 되도록 ALB에서 리다이렉트 라우팅 규칙을 생성
- 클라이언트가 사용하려는 URL에 ?Platform=Desktop이 있는 경우 타겟 그룹 2로 리다이렉팅 되도록 ALB에서 리다이렉트 라우팅 규칙을 생성

## Goot to know

- 사용시 고정 호스트 이름 부여 ([XXX.region.elb.amazonaws.com](http://XXX.region.elb.amazonaws.com))
- 애플리케이션 서버는 클라이언트의 IP를 직접 보지 못함
	- 클라이언트의 실제 IP는 X-Forwarded-For라고 불리는 헤더에 삽입됨
	- X-Forwarded-Port를 사용하는 포트와 X-Forwarded-Proto에 의해 사용되는 프로토콜도 얻음

![LB6](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/LB6.png)

# Network Load Balancer

- 4-layer 로드 밸런서 (전송 계층)
	- 인스턴스에 TCP와 UDP 트래픽을 포워딩 할 수 있음
	- 성능이 매우 높음 (초당 수백만 건 요청 처리)
	- ALB에 비해 지연 시간 짧음
- 가용 영역별로 하나의 고정 IP를 가짐
	- 1~3개의 IP로만 액세스할 수 있는 애플리케이션을 만들라는 문제가 나오면 NLB를 고려
- NLB 사용은 Free tier에 포함되지 않음

**→ 고성능, TCP, UDP, 정적 IP가 나오면 NLB를 생각**

## 작동 방식

### - TCP based traffic

![LB7](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/LB7.png)

- 대상 그룹을 생성하면 NLB가 대상 그룹을 리다이렉트
- 백엔드, 프론트엔드 모두가 TCP를 사용하거나 백엔드에서는 HTTP를, 프론트엔드에서는 TCP를 사용할 수도 있음

## Target groups

![LB8](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/LB8.png)

- 타겟 그룹이 될 수 있는 것
	- EC2 인스턴스
	- IP addresses - must be private IPs (하드코딩 되어야 함)
		- 자체 EC2 인스턴스의 사설 IP를 보낼 수도 있지만 데이터 센터의 Private IP를 보낼 수도 있음
		- 동시에 두 개의 앞에 로드 밸런서가 위치할 수 있음
	- Application Load Balancer
		- 네트워크 로드 밸런서 덕분에 고정 IP 주소를 얻을 수 있고 애플리케이션 로드 밸런서 덕분에 HTTP 유형의 트래픽을 처리하는 규칙을 얻을 수 있음
	- NLB 타겟 그룹이 수행하는 Health check
		- TCP, HTTP, HTTPS 프로토콜 지원

# Gateway Load Balancer

![LB9](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/LB9.png)

<aside> 💡 Appliance: 각종 기업용 소프트웨어를 서버와 스토리지 하드웨어에 최적화해 통합한 장비

</aside>

- 배포 및 확장과 AWS의 타사 네트워크 가상 어플라이언스의 플릿 관리에 사용
- 예시
	- 모든 트래픽이 방화벽을 통과하게 하기
	- 침입 탐지 및 방지 시스템에 사용
- 3-layer 로드 밸런서 (네트워크 계층)
- 두 가지 기능
	- Transparent network gateway (투명 네트워크 게이트웨이): VPC의 모든 트래픽이 GWLB가 되는 단일 엔트리 / 출구를 통과
	- Load balancer: 타겟 그룹의 가상 어플라이언스 플릿에 전반적으로 그 트래픽을 분산해 로드 밸런서가 됨
- **6081번 포트의 GENEVE 프로토콜을 사용하라!**

## Target groups

![LB10](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/LB10.png)

- 타겟 그룹이 될 수 있는 것
	- EC2 인스턴스
	- IP addresses - must be private IPs
		- 예를 들어 자체 데이터 센터에서 가상 어플라이언스를 실행하면 IP로 수동 등록 가능