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
	- L4 로드 밸런서라 HTTP 헤더 해석 불가능
	- 인스턴스에 TCP와 UDP 트래픽을 포워딩 할 수 있음
	- 성능이 매우 높음 (초당 수백만 건 요청 처리)
	- ALB에 비해 지연 시간 짧음
	- 대규모 트래픽 처리에 적합
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
- **6081번 포트의 GENEVE 프로토콜을 사용하라**

## Target groups

![LB10](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/LB10.png)

- 타겟 그룹이 될 수 있는 것
	- EC2 인스턴스
	- IP addresses - must be private IPs
		- 예를 들어 자체 데이터 센터에서 가상 어플라이언스를 실행하면 IP로 수동 등록 가능

# Sticky Sessions (Session Affinity)

![LB11](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/LB11.png)

- 고정 세션 혹은 세션 밀접성
- 고정성 혹은 고정 세션을 실행하는 것으로 로드 밸런서에 두가지 요청을 수행하는 클라이언트의 요청에 응답하기 위해 백엔드에 동일한 인스턴스를 갖는 것
- 이 동작은 CLB와 ALB에서 설정 가능
- Cookie를 이용해 클라이언트에서 로드 밸런서로 요청의 일부로서 전송됨
	- 쿠키가 만료되면 클라이언트가 다른 EC2 인스턴스로 리디렉션 될 수 있음
- Use cases
	- 사용자의 로그인 같은 중요한 정보를 취하는 세션 데이터를 잃지 않기 위해 사용자가 동일한 백엔드 인스턴스로 연결됨
- 고정성(Stickiness)을 활성화하면 백엔드 EC2 인스턴스 부하에 불균형을 초래할 수 있음

## Cookie Names (Stickiness type)

- Application-based Cookies
	- 사용자 정의 쿠키
		- 타겟으로 생성됨
		- 애플리케이션에서 생성됨
		- 애플리케이션에 필요한 모든 사용자 정의 속성을 포함할 수 있음
		- 쿠키 이름은 각 대상 그룹별로 개별적 지정해야 함
		- AWSALB, AWSALBAPP, AWSALBTG라는 이름은 ELB에 의해 예약되어 있어 사용 금지
	- Application cookie
		- 로드 밸런서에서 생성되는 쿠키
		- 쿠키 네임은 AWSALBAPP
- Duration-based Cookies
	- 로드 밸런서에서 생성되는 쿠키
	- 쿠키 네임은 ALB에선 AWSALB CLB에선 AWSELB
	- 특정 기간을 기반으로 만료되며 그 기간이 로드 밸런서 자체에서 생성됨

# Cross-Zone Load Balancing

![LB12](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/LB12.png)

- 맞는 방식은 없고 쓰임새에 따라 결정해야 함
- Application Load Balancer
	- 교차 영역 로드 밸런싱이 기본으로 활성화되어 있음
	- 타겟 그룹 설정에서 비활성화 가능
	- 데이터를 다른 AZ로 옮기는 데 비용이 들지 않음
- Network Load Balancer & Gateway Load Balancer
	- 교차 영역 로드 밸런싱이 기본으로 비활성화되어 있음
	- 활성화하여 데이터를 다른 AZ로 옮기려면 비용을 지불해야 함
- Classic Load Balancer
	- 교차 영역 로드 밸런싱이 기본으로 비활성화되어 있음
	- 활성화시켜도 AZ 간 데이터 이동에 비용이 들지 않음

# SSL / TLS

<aside>
* Listener: 구성한 프로토콜 (HTTP(S) 혹은 TCP, UDP) 및 포트로 서버 연결 요청을 확인하는 프로세스, 로드 밸런서를 통한 서버 접근 경로에 따라 어떤 서버로 요청을 보낼지를 설정, 리스너에 정의한 규칙에 따라 로드 밸런서에 등록된 Target Group으로 요청을 라우팅하는 방법이 결정됨
</aside>

- SSL 인증서는 클라이언트와 로드 밸런서 사이에서 트래픽이 이동하는 동안 암호화를 해줌
	- In-flight encryption
- SSL은 Secure Sockets Layer를 의미하고 연결을 암호화하는 데 사용
- TLS은 Transport Layer Security로 새로운 버전의 SSL임
- 최근에는 TLS 인증서가 주로 사용되는데 많은 사람들이 여전히 TLS를 SSL이라 부름
- Public SSL은 인증 기관(CA)에서 발급
	- Comodo, Symantec, GoDaddy, GlobalSign, Digicert, Letsencrypt 등과 같은 기관
- Public SSL 인증서를 로드 밸런서에 추가하면 클라이언트와 로드 밸런서 사이의 연결을 암호화할 수 있음
- SSL 인증서에는 만료 날짜가 있어서 주기적으로 갱신해 인증 상태를 유지해야 함

## Load Balancer - SSL Certificates

![LB13](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/LB13.png)

- 로드 밸런서는 X.509 인증서를 사용하는데, 이걸 SSL 또는 TLS 서버 인증서라고 부름
- AWS에는 이 인증서들을 관리할 수 있는 ACM이라는 게 있음 (AWS Certificate Manager)
- 원한다면 가지고 있는 인증서를 ACM에 업로드 가능
- HTTP 리스너를 구성할 때는 반드시 **HTTPS** 리스너로 해야 함
	- 기본 인증서를 지정해야 함
	- 다중 도메인을 지원하기 위해 다른 인증서를 추가할 수도 있음
	- **클라이언트는 SNI(Server Name Indication)라는 걸 써서 접속할 호스트의 이름을 알릴 수 있음**
	- 자기가 원하는 대로 보안 정책을 지정할 수 있음, 구 버전의 SSL과 TLS 즉 레거시 클라이언트를 지원할 수도 있음

## SNI (Server Name Indication)

![LB14](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/LB14.png)

- SNI는 **여러 개의 SSL 인증서를 하나의 웹 서버에 로드**해 하나의 웹 서버가 여러 개의 웹 사이트를 제공할 수 있게 함
- SNI는 확장된 (Newer) 프로토콜로, 최초 SSL 핸드셰이크 단계에 클라이언트가 대상 서버의 호스트 이름을 지정하도록 함
	- 이를 통해 클라이언트가 접속할 웹 사이트를 요청했을 때 서버는 어떤 인증서를 로드해야 하는지 알 수 있음
	- 확장된 (새로운) 프로토콜이기 때문에 모든 클라이언트가 지원하지는 않음
- 이 프로토콜은 ALB와 NLB, CloudFront에서만 동작함
	- CLB는 옛날 거라 동작 안 함

## Elastic Load Balancers - SSL Certificates

- CLB
	- 지원하긴 하지만 SSL 인증서 하나만 둘 수 있음
	- 여러 개의 인증서로 여러 호스트 이름을 지원하려면 CLB를 여러 개 둬야 함
- ALB
	- 여러 개의 SSL 인증서를 두고 리스너를 여러 개 지원할 수 있음
		- SNI를 이용
- NLB
	- 여러 개의 SSL 인증서로 다중 리스너 지원
		- SNI 이용

# Connection Draining

![LB15](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/LB15.png)

<aside>
* Draining: EC2 인스턴스 또는 컨테이너 서비스와 같은 리소스의 상태를 변경하거나 중지하기 전에 해당 리소스에서 실행 중인 작업을 안전하게 완료하도록 하는 프로세스
</aside>

- 다른 이름
	- Connection Draining (연결 드레이닝) -> for CLB
	- De-registration Delay (등록 취소 지연) -> for ALB, NLB
- 인스턴스가 드레이닝, 등록 취소(De-registration),  혹은 비정상(Unhealthy)인 상태에 있을 때 인스턴스에 어느 정도 시간을 줘 In-flight 요청, 즉 활성 요청을 완료할 수 있도록 하는 기능
- 연결이 드레이닝 되면, 즉 인스턴스가 드레이닝되면 ELB는 등록 취소 중인 EC2 인스턴스로 새로운 요청을 보내지 않음
- 연결 드레이닝 파라미터는 1 ~ 3,600초 사이의  값으로 설정 가능
	- 기본적으로 300초, 즉 5분
	- 이 값을 0으로 설정하면 전부 다 비활성화
		- 값이 0이면 드레이닝도 일어나지 않는다는 뜻
- 짧은 요청인 경우에는 **낮은 값**으로 설정하면 좋음
	- 예를 들어 1초보다 적은 아주 짧은 요청인 경우에는 연결 드레이닝 파라미터를 30초 정도로 설정하면 됨
		- 그래야 EC2 인스턴스가 빠르게 드레이닝되고 후에 오프라인 상태가 되어 교체 등의 작업을 할 수 있음
- 요청 시간이 매우 긴, 즉 업로드 또는 오래 지속되는 요청 등의 경우에는 어느 정도 **높은 값**으로 설정
	- 그러면 EC2 인스턴스가 금방 사라지지 않음
	- 연결 드레이닝 과정이 완료되기를 기다려야 할 것임

## 로드 밸런서에서 Connection Draining을 활용하는 시나리오

1. **인스턴스 유지 보수 및 업데이트**
	- 인스턴스에 패치 또는 업데이트를 적용하거나 다른 유지 보수 작업을 수행할 때, Connection Draining을 활성화하여 현재 연결을 완료한 다음 인스턴스를 안전하게 종료하거나 변경할 수 있음
2. **인스턴스 확장 또는 축소**
	- Auto Scaling 그룹과 함께 사용할 때, 인스턴스의 수가 변경되는 경우에도 Connection Draining이 유용
	- 인스턴스가 추가되거나 제거될 때 로드 밸런서는 새로운 연결을 새로운 인스턴스로 보내거나 기존 연결을 종료
3. **자동 확장 및 수축**
	- 로드 밸런서를 사용하여 트래픽이 증가하면 자동으로 인스턴스를 추가하고, 트래픽이 감소하면 자동으로 인스턴스를 제거하는 상황에서도 Connection Draining은 부드러운 확장 및 수축을 지원

# Auto Scaling Group

- 웹 사이트나 애플리케이션을 배포할 때 웹 사이트 방문자가 갈수록 많아지면서 로드가 바뀔 수 있음
- 클라우드에서, 즉 AWS에서는 EC2 인스턴스 생성 API 호출을 통해 서버를 빠르게 생성하고 종료할 수 있음
	- 이를 자동화하고 싶다면 ASG를 생성할 수 있음
- **ASG의 목표는 Scale out, 즉 증가한 로드에 맞춰 EC2 인스턴스를 추가하거나 Scale in, 즉 감소한 로드에 맞춰 EC2 인스턴스를 제거하는 것**
- 따라서 ASG 크기는 시간이 지나면서 변할 것
- ASG에서 실행되는 EC2 인스턴스의 최소 및 최대 개수를 보장하기 위해 매개변수를 전반적으로 정의할 수도 있음
- 기능
	- 로드 밸런서와 페어링하는 경우 ASG가 속한 모든 EC2 인스턴스가 로드 밸런서에 연결됨
	- 한 인스턴스가  비정상이면 종료하고 이를 대체할 새 EC2 인스턴스를 생성
- ASG는 무료이며 EC2 인스턴스와 같은 하위 리소스에 대한 비용만 내면 됨

## ASG 작동 방식

![LB16](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/LB16.png)

## ASG와 로드 밸런서

![LB17](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/LB17.png)

- 로드 밸런서가 비정상이라 판단하는 EC2 인스턴스를 ASG가 종료할 수 있어 편리함

## Auto Scaling Group Attribute

![LB18](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/LB18.png)

- 인스턴스 속성을 기반으로 ASG를 생성하려면 시작 템플릿 (Launch Template)을 생성해야 함
	- AMI + 인스턴스 타입
	- EC2 User data
	- EBS Volume
	- Security Groups
	- SSH Key pair
	- IAM Roles for your EC2 Instances
	- Network + Subnets Information
	- Load Balancer Information
- ASG의 최소 / 최대 / 초기 용량을 정의해야 함
- 스케일링 정책을 정의해야 함

## CloudWatch Alarms가 Auto Scaling과 결합되는 방식

![LB19](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/LB19.png)

- CloudWatch 경보를 기반으로 ASG를 Scale in / out 할 수 있음
- 경보는 지표(Metric)을 모니터링함 (such as Average CPU, or a custom metric)
- 예를 들어 ASG 전체의 평균 CPU가 너무 높으면 지표에 따라 경보를 울리고 경보가 ASG의 스케일링 활동을 유발
- 경보를 기반으로 Scale in / out 정책을 만들어 인스턴스 수를 자동으로 조절할 수 있음

## Dynamic Scaling Policies

### 대상 추적 스케일링 (Target Tracking Scaling)

- CloudWatch 지표와 목표값을 기준으로 그룹의 용량 조절
- 예를 들면 모든 EC2 인스턴스에서 오토 스케일링 그룹의 평균 CPU 사용률을 추적해 이 수치가 40%대에 머무를 수 있도록 할 때 사용
- 이처럼 기본 기준선을 세우고 상시 가용이 가능하도록 함

### 단순 / 단계 스케일링 (Simple / Step Scaling)

- CloudWatch 경보 (지표 하한값, 지표 상한값) 설정 기반으로 그룹의 용량 조절
- CloudWatch Alarm을 설정하고 가령 다음과 같이 전체 ASG에 대한 CPU 사용률이 70%를 초과하는 경우 용량을 두 유닛 추가하도록 설정할 수 있음
	- CPU 사용률이 30% 이하로 떨어지면 유닛 하나를 제거한다는 설정도 추가할 수 있음
- 단 CloudWatch 경보를 설정할 때는 한 번에 추가할 유닛의 수와 한 번에 제거할 유닛의 수를 단계별로 설정할 필요가 있음

### 예약된 작업 (Scheduled Actions)

- 나와 있는 사용 패턴을 바탕으로 스케일링을 예상하는 것
- 예를 들어 금요일 오후 5시마다 큰 이벤트가 예정되어 있으니 여러 사람이 애플리케이션에 몰려올 것을 대비해 ASG 최소 용량을 매주 금요일 오후 5시마다 자동으로 늘리도록 하는 것
- 스케일링이 필요함을 미리 알 때 예정된 작업을 설정하면 됨

### 예측 스케일링 (Predictive Scaling)

![LB20](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/LB20.png)

- AWS 내 오토 스케일링 서비스를 활용해 지속적으로 예측을 생성 가능
- 로드를 보고 다음 스케일링을 예측
- 시간에 걸쳐 과거 로드를 분석하고 예측이 생성됨
- 해당 예측을 기반으로 사전에 스케일링 작업이 예약됨
- 머신 러닝 기반

## Good **metrics** to scale on

![LB21](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/LB21.png)

- CPUUtilization (CPU 사용률)
	- 인스턴스 전반의 평균 CPU 사용률
	- 모든 인스턴스의 평균 CPU 사용률을 봤을 때 이 수치가 올라가면 인스턴스가 잘 사용되고 있다는 의미이니 스케일링에 있어서 좋은 지표가 됨
- RequestCountPerTarget
	- 로드 밸런서를 통해 전달되는 요청의 수를 백엔드 타겟(인스턴스 또는 컨테이너)별로 추적하는 데 사용되는 메트릭
	- 분산 작업을 통해 각 백엔드 타겟에 도달한 요청의 수를 실시간으로 측정
		- 이를 통해 특정 백엔드 인스턴스나 컨테이너에 대한 부하를 파악하고 관리할 수 있음
	- 특정 타겟에 트래픽이 집중되거나 불균형한 분산이 발생하는 경우를 식별하고 대응할 수 있음
	- EC2 인스턴스는 한 번에 타겟별로 1,000개의 요청까지만 최적으로 작동하므로 이 타겟을 스케일링에 활용할 수 있음
- Average Network In / Out
	- 애플리케이션이 네트워크에 연결된 경우
	- 예를 들어 업로드와 다운로드가 많아 EC2 인스턴스에 대해 해당 네트워크에서 병목 현상이 발생할 것으로 판단되면 평균 네트워크 입출력량을 기반으로 스케일링을 수행해 특정 임계값에 도달할 때 스케일링을 수행하도록 설정할 수 있음
- Any custom metric
	- CloudWatch에서 애플리케이션 별로 지표를 설정하고 이를 기반으로 스케일링 정책을 바꿀 수도 있음

## 스케일링 휴지 (Scaling Cooldowns)

- 스케일링 작업이 끝날 때마다 인스턴스를 추가 또는 삭제할 시 기본적으로 5분 또는 300초의 휴지 기간을 갖는 것
- 휴지 기간에는 ASG가 추가 인스턴스를 실행 또는 종료할 수 없음
- 이는 지표를 이용해 새로운 인스턴스가 안정화될 수 있도록 하며 어떤 새로운 지표의 양상을 살펴보기 위함
- 따라서 스케일링 작업이 발생할 때에 기본으로 설정된 쿨다운이 있는지 확인해야 함
	- 있을 경우 해당 작업을 무시
	- 없을 경우 인스턴스를 실행 또는 종료하는 스케일링 작업 수행
- 즉시 사용이 가능한 AMI를 이용해 EC2 인스턴스 구성 시간을 단축하고 이를 통해 요청을 좀 더 신속히 처리하는 것이 좋음
	- EC2 인스턴스 구성에 할애되는 시간이 적으면 즉시 적용이 가능하기 때문
	- 이렇게 활성화 시간이 빨라지면 쿨다운 또한 단축되기 때문에 ASG 상에서 더 많은 동적 스케일링이 가능해짐
- 또한 ASG가 일 분마다 지표에 접근할 수 있도록 세부 모니터링 기능 등을 사용하도록 설정하고 이와 같은 지표를 신속히 업데이트할 필요가 있음