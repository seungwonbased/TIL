# Virtual Machines and Networks in the Cloud

<aside>
💡 사설 네트워크: 인터넷과는 분리되어 있고 일반적으로 한 조직의 내부 시스템과 장치들을 연결하는 컴퓨터 네트워크

</aside>

# Virtual Private Cloud 네트워킹

- 많은 사용자들은 첫 번째 Google Cloud 프로젝트 내에서 자체 Virtual Private Cloud를 정의하거나 기본 Virtual Private Cloud를 시작함

## `VPC` (Virtual Private Cloud)

- Google Cloud와 같은 퍼블릭 클라우드 내에서 호스팅되는 안전한 개별 프라이빗 클라우드 컴퓨팅 모델
- VPC 내의 가상 머신 인스턴스에는 외부에서 접근 불가능
- 데이터 및 인프라 보안, 네트워크 트래픽 관리 등 보안 및 관리 요구사항을 충족시킬 수 있음
- 일반 프라이빗 클라우드에서 할 수 있는 모든 작업을 수행할 수 있으나, 이 프라이빗 클라우드는 퍼블릭 클라우드 제공업체가 원격으로 호스팅
- 퍼블릭 클라우드 컴퓨팅의 확장성과 편의성을 프라이빗 클라우드 컴퓨팅의 데이터 격리와 결합
- VPC 네트워크는 Google Cloud 리소스를 서로 연결하고 인터넷에 연결
    - 네트워크 분할 방화벽 규칙을 사용해 인스턴스 액세스 제한 정적 경로를 만들어 특정 대상으로 트래픽을 전달하는 것이 포함
- VPC 네트워크는 **전역(Global)**
- 전세계 모든 Google Cloud 리전에 서브넷을 가질 수 있음
    - 서브넷은 리전을 구성하는 여러 영역에 걸쳐 있을 수 있음
    - 이 아키텍처를 통해 전역 범위의 네트워크 레이아웃을 쉽게 정의할 수 있음
    - 리소스는 동일한 서브넷의 서로 다른 영역에 존재할 수 있음
    - 서브넷의 크기는 서브넷에 할당된 IP 주소 범위를 확장해 늘릴 수 있고 이렇게 해도 이미 구성된 가상 머신에는 영향을 미치지 않음

## Project, networks, subnetworks

[Project, networks, subnetworks](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/Core/Project.md)

## IP address

[IP address](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/Core/IP.md)

## Route and Firewall rules

[Route and Firewall rules](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/Core/Route.md)

## 일반적인 네트워크 설계

[일반적인 네트워크 설계](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/Core/NetworkArchi.md)

# Virtual Machine, `Compute Engine`

- Google Cloud의 IaaS 솔루션
- Compute Engine을 사용하면 사용자는 Google 인프라에서 가상 머신을 만들고 실행할 수 있음
- 빠르고 일관된 성능을 제공하도록 설계된 시스템에서 수천 개의 가상 CPU를 실행할 수 있음
- 각 VM에는 운영체제의 성능과 기능이 포함되어 있음
- 필요한 CPU 성능 및 메모리 용량, 스토리지 용량 및 운영체제의 유형을 지정하여 가상 머신이 물리적 서버와 매우 유사하게 구성될 수 있음

## Compute Engine

[Compute Engine](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/Core/Compute.md)

## VM access and Life cycle

[VM access and Life cycle](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/Core/VMAccess.md)

## Computing Options

[Computing Options](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/Core/ComputingOptions.md)

## 특수한 컴퓨팅 구성

[특수한 컴퓨팅 구성](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/Core/SpecialComputing.md)

## Image

[Image and Disk options](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/Core/ImageDisk.md)

## 일반적인 Compute Engine 작업

[일반적인 Compute Engine 작업](https://github.com/seungwonbased/TIL/blob/main/GoogleCloud/Core/GeneralCompute.md)

## 만드는 방법

- Google Cloud Console, Google Cloud CLI, Compute Engine API를 통해 가상 머신 인스턴스를 만들 수 있음

## 운영체제

- Google에서 제공하는 Linux 및 Windows Server 이미지
    - 혹은 이러한 이미지의 사용자 정의 버전
- 다른 운영 체제의 이미지
- 가상 머신에 이러한 이미지들을 빌드 및 실행하고 유연하게 재구성할 수 있음

## Cloud Marketplace를 이용한 구성

- Cloud Marketplace에는 타사 공급업체의 솔루션이 있어 이를 사용하면 소프트웨어, 가상 머신을 수동으로 구성할 필요가 없음
- 인스턴스, 스토리지 또는 네트워크 설정이 있지만 필요한 경우 출시 전에 수정 가능
- Cloud Marketplace의 대부분의 소프트웨어 패키지는 리소스에 대한 요금 외의 추가 비용 없이 사용 가능
- 일부 Cloud Marketplace의 이미지, 특히 제3 라이센스가 부여된 소프트웨어가 포함된 경우 사용료를 부과하지만 출시되기 전에 예상 월별 요금을 보여줌

## 가격 책정 및 청구 구조

- Compute Engine은 최소 1분부터 청구되며 초 단위 요금을 청구
- 지속 사용 할인은 실행 시간이 길어질수록 가상 머신에 자동으로 적용
- 약정 사용 할인도 제공
    - 안정적이고 예측 가능한 워크로드의 경우 약정에 대한 대가로 할인
- Preemptible Instances (선점형 인스턴스)
    - 일반적인 인스턴스보다 저렴하게 이용할 수 있지만 구글 클라우드 내부에서 일어나는 예기치 않은 이벤트(예: 가용성 영역의 문제)로 언제든지 종료될 수 있는 인스턴스
    - 일시적으로 필요한 계산 작업에 적합
    - 경우에 따라 90프로까지 할인 가능
- Spot VM
    - 다른 사용자들이 사용하지 않는 비정상적으로 남는 공간이나 자원이 남는 가상 머신 인스턴스를 사용하는 것
    - 저렴하지만, 언제든지 종료될 수 있음
    - 일시적으로 필요한 계산 작업에 적합
    - 경우에 따라 90프로까지 할인 가능
- 저장소
    - Compute Engine은 처리 디스크와 영구 디스크 간에 높은 처리량을 얻기 위해 특정 옵션이나 머신 유형이 필요하지 않음
    - 이것이 기본값으로, 추가 비용 없이 제공
- 커스텀 머신
    - 필요한 만큼만 비용 지불

## 인스턴스 머신 속성

- 사전 정의된 머신 유형 집합을 사용하거나
- 자체 커스텀 머신 유형을 만들어 가상 CPU 수 및 메모리 양과 같은 인스턴스의 머신 속성을 선택 가능

# 가상 머신 확장

- Compute Engine을 쓰면 인스턴스에 가장 적합한 머신 속성을 선택할 수 있음
    - 사전 정의된 머신 유형 선택
    - 커스텀 머신 유형 선택: 가상 CPU 수, 메모리 용량 등 선택
- 로드 측정 항목을 기반으로 애플리케이션에 VM을 추가하거나 빼는 자동 확장이라는 기능이 있음
- 수직 확장 (Scale up/down): VM 하나의 스펙을 올리거나 줄임
- 수평 확장 (Scale in/out): VM의 숫자를 늘림

# Important VPC Compatibilities

## Routing table

- 물리적 네트워크와 마찬가지로 VPC에는 라우팅 테이블이 내장되어 있어 라우터를 프로비저닝하거나 관리할 필요가 없음
- 외부 IP 주소 없이 동일한 네트워크 내에서, 하위 네트워크 간에 또는 Google Cloud 영역 간에도 한 인스턴스에서 다른 인스턴스로 트래픽을 전달하는 데 사용

## Firewall

- 프로비저닝하거나 관리할 필요가 없는 또 다른 항목
- VPC는 들어오는 트래픽과 나가는 트래픽 모두를 통해 인스턴스에 대한 액세스를 제한하도록 제어할 수 있는 글로벌 분산 방화벽을 제공
- 방화벽 규칙은 Compute Engine 인스턴스의 네트워크 태그를 통해 정의할 수 있음

## VPC Peering

- `VPC 피어링`을 통해 두 VPC 간의 관계를 설정해 트래픽 교환 가능
- IAM의 모든 기능을 사용해 한 프로젝트에서 다른 프로젝트의 VPC와 상호 작용할 수 있는 사람과 항목을 제어하려면 `공유 VPC`를 구성할 수 있음

# `Cloud Load Balancing`

- Load Balancer의 역할은 애플리케이션의 여러 인스턴스에 걸쳐 사용자 트래픽을 분산하는 것
    - 부하를 분산시켜 애플리케이션의 성능 문제가 발생할 위험을 줄여줌
- 사용자 트래픽 전체에 적용되는 완전 분산형의 소프트웨어 정의 관리형 서비스
- Load Balancer는 VM에서 실행되지 않기 때문에 확장 또는 관리에 대해 걱정하지 않아도 됨
    - 가능한 시나리오
        - 모든 트래픽 앞에 Load Balancer를 배치
        - HTTP 또는 HTTPS, 기타 TCP 및 SSL 트래픽, UDP 트래픽 앞에도 배치 가능
- 백엔드 상태가 양호하지 않을 경우 트래픽을 부분으로 나눠 이동시키는 자동 멀티 리전 장애 조치 등 리전을 교차하는 부하 분산을 제공
- Cloud Load Balancing은 사용자, 트래픽, 네트워크, 백엔드 상태와 기타 관련 조건의 변화에 즉각적으로 반응
    - 사전 경고, 웜업이 필요하지 않음
- 특정 포트에 특화된 Load Balancer가 있어 프로토콜에 맞게 사용할 수 있음
- 리전별 내부 Load Balancer: Google Cloud 내부 IP 주소의 트래픽을 받아 Compute Engine VM 간의 부하를 분산
    - 프로젝트 내에서 트래픽 부하를 분산

# Cloud DNS and Cloud CDN

## `Cloud DNS`

- Google Cloud는 전세계에서 Cloud DNS를 찾을 수 있도록 제공
- Google과 동일한 인프라에서 실행되는 관리형 DNS 서비스
- 대기 시간이 짧고 가용성이 높으며 사용자가 애플리케이션과 서비스를 사용할 수 있도록 하는 비용 효율적 방법
- DNS 정보는 전세계 중복 위치에서 제공
- Programmable
- Cloud Console, CLI, API를 사용해 DNS 영역과 레코드를 게시하고 관리 가능

## `Cloud CDN`

- Google의 Edge cache의 글로벌 시스템
    - 캐싱 서버를 이용해 최종 사용자에게 더 가까운 콘텐츠를 저장
    - 고객은 네트워크 대기 시간이 단축되고 콘텐츠 원본의 로드가 줄어들며 비용 감소

# Google VPC로 네트워크 연결하기

- 많은 Google Cloud 고객은 On-premise 네트워크나 다른 클라우드의 네트워크 같은 시스템의 다른 네트워크와 Google VPC를 연결하려고 함
- 이를 수행하는 여러가지 효과적인 방법이 있음

## IPsec VPN protocol

- IPsec VPN 프로토콜을 사용해 터널 연결을 만드는 것
- 연결을 동적으로 만들기 위해 Cloud Router라는 Google Cloud의 기능을 사용할 수 있음
- Cloud Router를 사용하면 다른 네트워크 및 Google VPC가 Border Gateway Protocol을 사용해 VPN을 통해 경로 정보를 교환할 수 있음
- 이 방법을 사용해 Google VPC에 새로운 서브넷을 추가하면 On-premise 네트워크가 자동으로 이 서브넷에 대한 경로를 가져옴
- 이렇게 인터넷으로 네트워크를 연결하는 게 최선의 옵션은 아님
    - 보안 이슈, 대역폭 안정성 이슈

## Direct Peering

- Google과 Direct peering
    - Peering: 라우터를 Google 접속 지점과 동일한 공개 데이터 센터에 배치하고, 이를 사용해 네트워크 간에 트래픽을 교환하는 것
- Google은 전세계에 100개 이상의 접속 지점을 보유
    - 아직 접속 지점에 있지 않은 고객은 이동통신사 피어링 프로그램의 파트너와 계약하여 연결 가능
        - 단점은 Google 서비스 수준 계약이 적용되지 않는다는 것

## Dedicated Interconnect

<aside>
💡 Topology: 컴퓨터 네트워크의 요소들(링크, 노드 등)을 물리적으로 연결해 놓은 것, 혹은 그 연결 방식

</aside>

<aside>
💡 SLA: Service Level Agreement, 서비스 제공자와 사용자 간의 계약으로, 제공되는 품질 수준과 이에 따른 보상 방안을 명시하는 것

</aside>

- Google과 하나 이상의 직접, 비공개 연결을 허용해 Google 사양을 만족하는 토폴로지가 있는 경우 최대 99.99%의 SLA가 적용될 수 있음
- 상호 연결을 위해 최대 업타임을 확보하는 것이 중요할 때 좋은 솔루션
- 이러한 연결은 더 나은 안정성을 위해 VPN으로 백업 연결 가능

## Partner Interconnect

- 지원되는 서비스 제공 업체를 통해 On-premise 네트워크와 VPC 네트워크 간의 연결을 제공
- 데이터 센터가 Dedicated Interconnect co-location 시설에 연결할 수 없는 물리적 위치에 있거나 데이터 수요가 10Gbps 연결 전체를 사용할 만큼 크지 않은 경우에 유용
- 마찬가지로 Google 사양을 만족하는 토폴로지가 있는 경우 최대 99.99%의 SLA가 적용될 수 있음
- 그러나 Google은 타사 서비스 제공 업체가 제공하는 Partner Interconnect의 모든 측면 또는 Google 네트워크 외부의 문제에 대해 책임을 지지 않음