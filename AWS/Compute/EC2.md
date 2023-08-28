# EC2 (Elastic Computing Cloud)

- Amazon EC2는 클라우드에서 컴퓨팅 파워의 규모를 자유자재로 변경할 수 있는 웹 서비스
- **IaaS (Infra as a Service)**
- 64000 EBS IOPS를 달성하려면 **Nitro-based EC2**를 프로비저닝해야 함
	- Non-nitro EC2 사용 시 최대 32000 EBS IOPS
- EC2 인스턴스를 다시 시작하면 Public IP가 변경될 수 있음
	- **Elastic IP (탄력적 IP)** 를 사용해 EC2 인스턴스에 고정 Public IPv4를 할당
	- 기본적으로 모든 AWS 계정은 지역당 탄력적 IP 주소를 5개로 제한
- EC2 인스턴스를 동일한 AZ에 배치해 데이터 전송 비용을 줄임
- **EC2 Hibernate**는 인스턴스 메모리(RAM)의 내용을 Amazon EBS 루트 볼륨에 저장
- **VM Import / Export**를 사용해 가상 머신 이미지를 가져오고 Amazon EC2 AMI로 변환하여 EC2 인스턴스를 시작

## EC2 Sizing & Configuration Options
### Options

- **Operating system**: Linux, Windows, MacOs, …
- How much compute power & cores (**CPU**)
- How much random-access memory (**RAM**)
- How much **storage** space
	- Network-attached (EBS & EFS)
	- Hardware (EC2 Instance Store)
- **Network card**: speed of the card, Public IP address
- **Firewall** rules: security group
- **Bootstrap script** (configure at first launch): EC2 User data
	**→ 원하는 대로 가상 머신을 선택하여 AWS에서 빌릴 수 있음!**

### EC2 User Data

- 인스턴스를 Bootstrapping 할 수 있음
	- Bootstrapping: 머신이 **작동될 때** 명령을 시작하는 것
	- 스크립트는 처음 시작할 때 한 번만 실행되고 다시는 실행되지 않음
	- 부팅 작업을 자동화하기 때문에 Bootstrapping이라는 이름을 갖게 됨
	- 자동화하는 것
		- 업데이트 설치
		- 소프트웨어 설치
		- 일반적인 파일을 인터넷에서 다운로드
		- 그 외 원하는 모든 것
- EC2 User data script는 루트 계정에서 실행됨
	- 따라서 모든 명령문은 sudo로 실행

## EC2 Instance types

| Instance class |    사용 목적    |                                                     사용 예시                                                     |
| :------------: | :-------------: | :---------------------------------------------------------------------------------------------------------------: |
|      T, M      |    일반 목적    |                        웹 서버, 마이크로 서비스, 소규모 DB, 가상 데스크탑, 개발 환경 구축                         |
|       C        |  컴퓨팅 최적화  |                      고성능 컴퓨팅, 일괄 처리, 게임 서버, 과학적 모델링, CPU 기반 기계 학습                       |
|    R, X, Z     |  메모리 최적화  |                                  인메모리 캐시, 고성능 DB, 실시간 빅데이터 분석                                   |
|    F, G, P     |   가속 컴퓨팅   |                           고성능 GPU, 그래픽 집약적 애플리케이션, 기계 학습, 음석 인식                            |
|    D, H, I     | 스토리지 최적화 | EC2 인스턴스 스토리지, 높은 I/O 성능, HDFS, MapReduce file system, Spark, Hadoop, Redshift, Kafka, Elastic Search |

#### General Purpose Instances

- 웹 서버나 코드 리포지토리와 같은 다앙한 작업에 적합
- 컴퓨팅, 메모리, 네트워킹 간의 균형이 잘 맞음

#### Compute Optimized Instances

- 컴퓨팅 집약적인 작업에 최적화됨
	- 일부 데이터의 일괄 처리
	- 미디어 트랜스코딩 작업
	- 고성능 웹 서버
	- 고성능 컴퓨팅 (HPC)
	- 머신러닝
	- 전용 게임 서버
	- 등등
- C로 시작하는 이름 (Compute)

#### Memory Optimized Instances

- 메모리에서 대규모 데이터셋을 처리하는 작업에서 빠른 퍼포먼스를 보임
	- 인 메모리 데이터베이스
	- 분산 웹 스케일 캐시 스토어 (예: ElastiCache)
	- Business Intelligence (BI)
	- 대규모 비정형 데이터의 실시간 처리를 실행하는 애플리케이션
	- 등등
- R로 시작하는 이름 (X1, Z1 도 있긴 함) (RAM)

#### Storage Optimized Instances

- 로컬 스토리지에서 대규모의 데이터셋에 읽기와 쓰기 엑세스를 요구하는 Storage 작업에 적합
	- High frequency Online Transaction Processing (OLTP) systems
	- Relational & NoSQL databases
	- 인 메모리 데이터베이스를 위한 캐시 (예: Radis)
	- Data Warehousing 애플리케이션
	- 분산 파일 시스템
- I, G로 시작하는 이름 (H1도 있긴 함)

# Security Groups
## Introduction

- Security Group은 AWS에서 네트워크 보안을 실행하는 데 핵심
- EC2 인스턴스에 들어오고 나가는 트래픽을 허용하는 것을 제어
- Security Group은 출입이 허용된 것이 무엇인지 확인 가능
- Security Group은 IP나 다른 Security Group을 참조하여 Rule을 만들 수 있음

## Deeper Dive

- Security Group은 EC2 인스턴스의 방화벽 역할
- 통제하는 것
	- Port로의 액세스
	- Authorized IP ranges → IPv4 and IPv6
	- 외부에서 인스턴스로 들어오는 인바운드 네트워크
	- 인스턴스에서 외부로 나가는 아웃바운드 네트워크

## Diagram

![SG1](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/SG1.png)

## 알아야 할 것

- 여러 인스턴스와 연결 가능
	- 보안 그룹과 인스턴스와의 일대일 관계는 없음
- 보안 그룹은 **지역**과 **VPC**의 결합으로 통제되어 있음
	- 지역을 전환하면 새 보안 그룹을 생성하거나 다른 VPC를 생성해야 함
- 보안 그룹은 EC2 외부에 있음
	- 그래서 트래픽이 차단되면 EC2 인스턴스는 보안 그룹을 볼 수 없음
- SSH 액세스를 위해 하나의 별도 보안 그룹을 유지하는 것이 좋음
- 타임아웃으로 애플리케이션에 접근할 수 없으면 그것은 보안 그룹 이슈임
	- 연결 거부 이슈는 보안 그룹이 작동하는 것이고 애플리케이션에 문제가 생긴 등의 이슈임
- 디폴트로 인바운드 트래픽은 블락되어 있음
- 디폴트로 아웃바운드 트래픽은 Authorized

## 보안 그룹의 다른 보안 그룹 참조 방법

![SG2](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/SG2.png)

- IP를 신경쓰지 않아도 되는 점이 좋음
- 인스턴스에 연결된 보안 그룹 3은 보안 그룹 1에서 Authorized가 아니라 연결 거부
- 고급 기능이지만 일반적인 패턴이라 알아두면 좋음

## EC2 시작 유형 (EC2 Launch Type)
### **On-demand**

- 사용한 만큼 지불
- 실행하는 인스턴스에 따라 시간 또는 초당 컴퓨팅 파워로 측정된 가격을 지불
- 약정이 필요 없음
- 장기적인 수요 예측이 힘들거나 유연하게 EC2를 사용하고 싶을 때 사용

#### On-demand 권장 소비자

- 선불 결제 또는 장기 약정 없이 Amazon EC2의 저렴한 비용과 유연성을 선호하는 사용자
- 중단할 수 없는 단기, 예측할 수 없는 워크로드가 있는 애플리케이션
- Amazon EC2에서 처음으로 개발 또는 테스트 중인 애플리케이션

### **예약 인스턴스 (Reserved Instance - RI Pricing)**

- 미리 일정 기간(1 ~ 3년)을 약정해서 쓰는 방식 (선납금 납부)
- 최대 75% 저렴
- 수요 예측이 확실할 때 사용

#### Reserved Standard

- 표준 RI는 장기 계약 시 특정 인스턴스 유형에 대해 큰 폭의 할인 제공

#### Reserved Convertible

- 컨버터블 RI는 할인 폭은 좀 더 작지만 계약 도중 다른 인스턴스 유형으로 변경 가능
- 계약 중 다른 속성도 변경할 수 있음

#### Reserved Instance 권장 소비자

- 수요가 꾸준한 애플리케이션
- 예약 용량이 필요한 애플리케이션
- 총 컴퓨팅 비용 절감을 위해 1년 또는 3년 동안 EC2를 사용하기로 약정할 수 있는 사용자

### **Savings Plans**

- 컴퓨팅 리소스 사용량을 미리 지불해 할인받을 수 있는 요금 체계
- 기존 RI와 비슷하지만 다른 요금 체계
	- RI는 리전, 인스턴스 사이즈, 인스턴스 타입 등을 명시적으로 선택해 약정을 구매해야 했으며 한번 구매한 RI는 변경이 불가능하거나 재판매가 제한적이었음
- Savings Plans는 시간당 사용 금액만 미리 산정해 약정
	- 즉 인스턴스 사이즈, 인스턴스 타입과는 상관 없이 사용하고자 하는 금액을 설정
- AWS는 Savings Plans가 RI와 비슷한 수준의 할인을 제공하면서 더 유연한 정책을 갖고 있으므로 RI 만료 이후에 Savings Plans를 사용할 것을 권장

#### Savings Plans 종류
##### Computing Savings Plans

- 최대 66% 절감
- EC2 인스턴스 패밀리, 인스턴스 사이즈, AZ, Region, OS, Tenancy 상관 없이 적용
- Fargate 적용 가능
- 기존 컨버터블 RI와 할인율 동일

##### EC2 Instance Savings Plans

- 최대 72% 절감
- AZ, 인스턴스 사이즈, OS, Tenancy 관계 없이 적용
- Region, 인스턴스 패밀리는 지정이 필요함
- 기존 표준 RI와 할인율 동일

#### Savings Plans 특징

- 인스턴스 타입 변경시 별도 작업 없이 RI보다 유연하게 할인을 적용받을 수 있음

### **Spot Instnace**

- 경매 형식으로 시장에 남는 인스턴스를 구매해서 쓰는 방식
- On-demand 요금에 비해 최대 90% 저렴
- AWS가 임의로 인스턴스를 회수해갈 수 있음
	- 반환 시간 예측 불가능
	- 인스턴스 수요가 적어지면 다시 인스턴스를 할당받아 애플리케이션을 다시 구동 가능

#### Spot Instance 원리

![SpotInstance](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/SpotInstance.png)

- 원하는 지정 가격을 정하고, 이 지정 가격보다 비싸지면 반납해서 인스턴스를 중지시키고, 가격이 싸지면 다시 인스턴스를 사용하는 원리
- 인스턴스를 종료하기 위해서는 스팟 요청(Spot Request)을 먼저 취소해 AWS가 더 이상 새로운 인스턴스를 실행하지 않게끔 한 뒤에 연결된 스팟 인스턴스를 종료해야 함 (순서가 중요함)

#### Spot capacity pool

- 미사용 EC2 인스턴스 집합

#### Spot price

- 스팟 인스턴스의 현재 시간당 가격

#### Spot Instance request

- 스팟 인스턴스를 사용하기 위한 최고 가격을 제시하면 그에 맞는 인스턴스가 있을 때 사용할 수 있게 해줌
- 일회성이나 영구적으로 사용하는 방법이 있는데 영구적 사용 시 인스턴스가 종료되고 다시 인스턴스 풀에 사용할 수 있는 인스턴스가 발견되면 사용하게 해줌

#### Spot Fleet

- 사용자가 지정한 기준에 따라 시작되는 스팟 인스턴스의 집합
- 목표 용량을 정해두면 그 용량에 맞춰 인스턴스를 유지하려고 함
- 이 요청에 온디맨드 요청을 포함시킬 수 있음
- 여러 가지 런치풀과 인스턴스 유형을 다양하게 정의할 수 있음

##### Spot Fleet 내에 Spot Instance를 할당해줄 다음과 같은 전략을 정의해야 함

1. lowestPrice: 스팟 플릿이 **가장 적은 비용**을 가진 풀에서부터 인스턴스를 실행 (비용 최적화, 아주 짧은 워크로드에 적합)
2. diversified: 스팟 인스턴스가 기존에 정의한 모든 풀에 걸쳐 분산됨, 한 풀이 중단되더라도 다른 풀이 활성화 되어있음 (가용성 뛰어남, 긴 워크로드에 적합)
3. capacityOptimized: 인스턴스의 개수에 따라 최적 용량으로 실행되고 적절한 풀을 찾아줌

#### EC2 Instance Rebalance Recommendation

- 스팟이 종료되려고 할 때 리밸런싱 권고 신호를 2분 전에 생성

#### Spot Instance Interruption

- 최고 가격에 맞는 인스턴스가 더 이상 없을 경우 스팟 인스턴스를 중지하거나 절전 모드로 전환

#### Spot Instance 권장 소비자

- 시작 및 종료 시간이 자유로운 응용 애플리케이션
- 빅데이터 처리, ML 등 많은 인스턴스가 필요한 작업에 사용
- 컴퓨팅 가격이 매우 저렴해야만 수익이 나는 애플리케이션
- 대량의 서버 용량 추가로 긴급히 컴퓨팅 파워가 필요한 사용자

#### Spot Instance vs Spot Fleet

- AWS에서 **스팟 인스턴스는 미사용 EC2 인스턴스에 입찰할 수 있는 EC2 인스턴스 유형**으로, 온디맨드 인스턴스에 비해 훨씬 저렴한 비용으로 시작할 수 있습니다. 이러한 인스턴스는 EC2 Spot Market을 통해 사용할 수 있으며 **가격은 수요와 공급에 따라 결정**됩니다.

- 반면 **스팟 플릿은 단일 단위로 관리할 수 있는 스팟 인스턴스 모음**입니다. **스팟 플릿을 사용하여 지정된 대상 용량, 인스턴스 유형 및 가격 전략으로 스팟 인스턴스 그룹을 시작하고 관리**할 수 있습니다. 스팟 플릿은 스팟 시장에서 사용 가능한 가장 저렴한 인스턴스를 선택하여 비용 절감을 최적화하는 데 사용할 수 있습니다.

- 스팟 인스턴스와 스팟 집합의 주요 차이점은 스팟 인스턴스는 입찰하는 개별 인스턴스인 반면 스팟 집합은 단일 단위로 시작하고 관리하는 인스턴스 그룹이라는 것입니다. 스팟 플릿은 스팟 인스턴스보다 더 유연하고 확장 가능한 옵션입니다. 목표 용량을 지정하고 여러 인스턴스 유형과 가용 영역을 사용하여 워크로드가 항상 실행되도록 할 수 있기 때문입니다.

- 또한 스팟 집합은 지정된 용량 및 입찰 전략에 따라 인스턴스를 자동으로 시작 및 종료할 수 있으므로 개별 스팟 인스턴스보다 관리가 더 쉽습니다. 그러나 Spot Fleet은 AWS Management Console에서 몇 번의 클릭만으로 시작할 수 있는 Spot 인스턴스에 비해 추가 구성 및 설정이 필요할 수 있습니다.

### **전용 호스트 (Dedicated Host)**

- EC2 인스턴스 용량을 갖춘 물리적 서버를 대여
- 보안적인 이유, 라이센스에 따른 이유, 퍼포먼스를 위한 이유 등으로 사용됨
- 사용자 전용의 EC2 인스턴스 용량을 갖춘 물리적 서버
- 호스트 단위 결제

#### Dedicated Host 권장 소비자

- EC2에서 Microsoft 및 Oracle 같은 공급업체의 적격 소프트웨어 라이센스를 사용할 경우 (BYOL 환경이 필요한 경우)
- 기존의 물리적 서버에서 EC2를 사용할 경우

### **전용 인스턴스 (Dedicated Instance)**

- 인스턴스를 구동하면 그 인스턴스가 할당된 물리적 서버는 같은 AWS 계정의 인스턴스만 할당되어 사용하는 옵션
- 인스턴스를 어떤 물리적 서버에 배치하고 구동할지 고객은 선택할 수 없으며 이는 AWS가 수행
- 단일 고객 전용 VPC에서 실행되는 EC2 인스턴스
- 인스턴스 단위 결제

```
전용 호스트, 전용 인스턴스 공통점
- 사용자 전용 물리적 서버
- 성능이나 보안상 물리적 차이는 없음
```

## EC2 요금 정책

### 가격 순서

- Spot < Reserved < On-demand < Dedicated
- EC2의 가격 모델은 EBS와는 별도로 청구된다 (EBS는 사용한 만큼 지불)
- 기타 데이터 통신 등의 비용은 별도로 청구
	- 이때, AWS는 AWS 바깥으로 나가는 트래픽에만 요금 부과
		- 수십 TB 파일을 업로드할 땐 무료
		- 해당 파일을 다운로드 할 땐 요금 청구
- Reserved Instance는 20개, vCPU On-demand 표준 인스턴스 1,152개, vCPU Spot Instance 1,440개로 제한

## Elastic IP

- EC2 인스턴스를 시작하고 중지할 때 공용 IP를 바꿀 수 있음
- 고정 Public IP를 사용하려면 Elastic IP가 필요
- Public IPv4이며 삭제하기 전까진 유지됨
- 한 번에 한 인스턴스에만 attach 가능
- 한 인스턴스에서 다른 인스턴스로 빠르게 이동함으로써 인스턴스 또는 소프트웨어의 오류를 마스킹할 때 사용할 수 있음
	- 계정당 탄력적 IP를 5개만 쓸 수 있기 때문에 이런 일은 드물긴 함
- 결과적으로 탄력적 IP는 사용하지 않는 것이 좋음
	- 매우 좋지 않은 아키텍처상 결정
	- 대신 임의의 Public IP를 써서 DNS를 할당하는 것이 좋음

## Placement Groups (배치 그룹)

- EC2 인스턴스가 AWS 인프라에 배치되는 방식을 제어하고자 할 때 사용
- AWS의 하드웨어와 직접적인 상호작용을 하진 않지만 EC2 인스턴스가 각각 어떻게 배치되기를 원하는지 AWS에 알려주는 것

### 배치 그룹을 만들 때 다음과 같은 전략 정의 가능
#### **Cluster**

![PlacementGroupsCluster](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/PlacementGroups.jpg)

- 단일 가용 영역 내에서 지연 시간이 짧은 하드웨어 설정으로 인스턴스를 그룹화
- 높은 성능, 높은 위험

##### 장점

- 훌륭한 네트워크

##### 단점

- 하나의 랙이 실패하면 모든 인스턴스가 실패

##### Use Cases

- 빅 데이터와 같이 매우 빨라야 하는 작업을 수행하거나, 극히 짧은 지연 시간과 높은 네트워크 처리량을 필요로 하는 작업에 사용

#### **Spread**

![PlacementGroupsSpread](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/spread.jpg)

- 인스턴스가 다른 하드웨어에 분산된다는 의미, 가용 영역별로 분산된 배치 그룹당 7개의 EC2 인스턴스만 가질 수 있다는 제약 조건이 있음
- Critical application이 있는 경우 분산 배치 그룹 사용

##### 장점

- 여러 가용 영역에 걸쳐있을 수 있음
- 동시 실패의 위험 감소
- EC2 인스턴스들이 다른 물리적 하드웨어에 있음

##### 단점

- 배치 그룹의 가용 영역당 **7개의 인스턴스**로 제한됨
	- 배치 그룹의 규모가 제한됨

##### Use Cases

- 가용성을 극대화하고 위험을 줄여야하는 애플리케이션
- 인스턴스 오류를 서로 격리해야 하는 크리티컬 애플리케이션

#### **Partition**

![PlacementGroupsPartition](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/partition.jpg)

- 분산 배치 그룹과 비슷하게 인스턴스를 분산하려는 것, 다만 분할 배치 그룹은 여러 파티션에 인스턴스가 분할되어 있고 이 파티션은 가용 영역 내의 다양한 하드웨어 랙 세트에 의존, 그룹당 수백 개의 EC2 인스턴스를 통해 확장할 수 있고 이를 통해 Hadoop, Cassandra, Kafka 같은 애플리케이션 실행 가능
- 각 파티션은 AWS의 랙을 나타냄
- 7 partitions per AZ
- 각 파티션은 동일한 리전의 여러 가용 영역에 걸쳐 있을 수 있음
- 설정으로 최대 수백 개 EC2 인스턴스를 얻을 수 있음
- 각 파티션은 다른 파티션의 실패로부터 격리됨
- EC2 인스턴스가 어떤 파티션에 있는지 알기 위해 메타데이터 서비스를 사용해 이 정보에 액세스하는 옵션이 있음

##### Use Cases

- 파티션 전반에 걸쳐 데이터와 서버를 퍼뜨려 두도록 파티션 인식 가능한 애플리케이션의 경우
- HDFS, Hadoop, Cassandra, Apache Kafka를 사용해 파티션을 인식하는 빅 데이터 애플리케이션

## Elastic Network Interfaces (ENI)

- VPC의 논리적 구성 요소이며 **가상 네트워크 카드**를 나타냄
	- EC2 인스턴스를 생성하면 인스턴스당 하나 이상의 네트워크 인터페이스가 생성됨
	- 직접 ENI를 만들고 인스턴스에 붙일 수 있음
- ENI는 EC2 인스턴스가 네트워크에 액세스할 수 있게 해줌
- ENI는 EC2 인스턴스 외부에서도 사용됨
- 만들어도 비용 발생 안함
- ENI는 다음과 같은 속성을 가질 수 있음
	- 주요 사설 IPv4와 하나 이상의 보조 IPv4를 가질 수 있음
	- 각 ENI는 사설 IPv4당 탄력적 IPv4를 갖거나 혹은 하나의 Public IPv4를 가질 수 있음
		- 즉 사설 및 공용 IP가 하나씩 제공됨
	- ENI에 하나 이상의 보안 그룹을 연결할 수 있음
- EC2 인스턴스와 독립적으로 ENI를 생성하고 즉시 연결하거나 장애 조치를 위해 EC2 인스턴스에서 이동시킬 수 있음
- ENI는 특정 가용 영역(AZ)에 바인딩됨
	- 특정 AZ에서 ENI를 생성하면 해당 AZ에만 바인딩할 수 있음
- 하나의 인스턴스에 장애가 나면 다른 인스턴스로 ENI를 옮겨 붙여서 사설 IP를 이동시킬 수 있음
	-> 장애 조치에 유용

## EC2 Hibernate

- In-memory (RAM) 상태가 보존됨
- 인스턴스 부팅이 빨라짐
	- OS가 중지되거나 다시 시작되는 게 아니기 때문
- RAM 상태가 루트 경로의 EBS 볼륨에 기록되기 때문에 루트 EBS 볼륨을 암호화해야 하고 용량도 충분해야 함

### Use cases

- Long-running processing
- Saving the RAM state
- Service that take time to initialize

### Good to Know

- Supported Instance Families: C3, C4, C5, I3, M3, M4, R3, R4, T2, T3, ...
- Instance RAM Size: must be less than 150 GB
- Instance Size: not supported for bare metal instances
- AMI: Amazon Linux 2, Linux AMI, Ubuntu, RHEL, CentOS & Windows, ...
- Root Volume: must be EBS, encrypted, not instance store, and large
- Available for **On-demand, Reserved and Spot Instances**
- An instance cannot be hibernated more than 60 days
