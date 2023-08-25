# AMI
- Amazon Machine Image
	- EC2 인스턴스를 통해 만든 이미지를 통칭
	- Image: 설정과 실행에 필요한 모든 파일과 설정값들을 담고 있는 것
- AMI are a customization of an EC2 instance
	- 원하는 소프트웨어 또는 설정 파일을 추가하거나 별도의 OS, 모니터링 툴 등을 추가할 수 있음
- AMI를 따로 구성하면 부팅 및 설정에 드는 시간을 줄일 수 있음
	- EC2 인스턴스에 설치하고자 하는 모든 소프트웨어를 AMI가 미리 패키징
- AMI를 특정 리전에 구축한 다음 다른 리전으로 복사해서  AWS의 글로벌 인프라를 활용할 수도 있음
- EC2 인스턴스를 다음과 같은 AMI에서 실행할 수 있음
	- Public AMI: AWS provided
	- My own AMI: 직접 구성하고 유지, 관리해야 함
	- AWS Marketplace AMI: 다른 유저가 만든 AMI

## AMI Process (from an EC2 instance)
1. EC2 인스턴스를 원하는 대로 설정
2. 인스턴스를 중지해 데이터 무결성 확보
3. 해당 인스턴스를 바탕으로 AMI 구축, 이 과정에서 EBS 스냅샷 생성됨
4. AMI로 새 인스턴스 실행

# EBS Volume

![Unité Choose the Right Storage Service | Salesforce Trailhead](https://res.cloudinary.com/hy4kyit2a/f_auto,fl_lossy,q_70/learn/modules/aws-storage/choose-the-right-storage-service/images/75c6bec122ddc0a1a76b0bf99a89cae0_2-c-235-e-2-f-2448-40-c-3-8-c-7-b-e-9753-d-6-b-0-df-5.png)

![EBSVolume](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/EBSVolume.png)

- Elastic Block Store
- 인스턴스가 실행 중인 동안 연결 가능한 **네트워크 드라이브**
	- 물리적 드라이브가 아님
	- 통신을 위해서는 네트워크가 필요 (레이턴시 발생 가능)
	- 한 EC2 인스턴스에서 다른 인스턴스로 쉽게 Attach/detach 가능
- EBS 볼륨을 사용하면 인스턴스가 종료된 후에도 데이터를 지속할 수 있음
- 인스턴스를 재생성하고 이전 EBS 볼륨을 마운트하면 데이터를 다시 받을 수 있음
- CCP Level: **하나의 EBS를 하나의 EC2 인스턴스에 마운트**
- Associate Level: 일부 EBS 다중 연결
- EBS 볼륨을 생성할 때는 특정 AZ에서만 가능
	- 다른 AZ로 옮기려면 스냅샷 먼저 찍어야 함
- Provisioned capacity를 가짐 (Size in GB and IOPS)
	- Provision된 용량에 따라 요금이 청구됨
	- 용량을 이후에 늘릴 수도 있음
- EBS 볼륨을 생성하고 EC2에 연결해놓지 않아도 됨

## EBS Delete on Termination Attribute
- EC2 인스턴스를 생성할 때 콘솔에서 EBS 볼륨을 생성하면 종료 시 삭제 옵션이 있음
- 기본적으로 루트 볼륨에는 체크되어 있는데 새로운 EBS 볼륨에는 체크되어 있지 않음
- 이 옵션을 통해 인스턴스 종료 시 EBS 행동을 제어할 수 있음
- AWS 콘솔 또는 AWS CLI를 통해 조작 가능

## EBS Snapshot
- EBS 볼륨의 특정 시점에 대한 백업
- EC2 인스턴스에서 EBS 볼륨을 분리할 필요는 없지만 권장 사항임
- 스냅샷은 다른 AZ나 리전에도 복사 가능

### EBS Snapshot Features

#### EBS Snapshot Archive
- 최대 75%까지 저렴한 아카이브 티어
- 스냅샷을 아카이브 티어로 옮기면 아카이브를 복원하는 데 24 ~ 72시간 소요
	- 즉시 복원 X

#### Recycle Bin for EBS Snapshots
- EBS 스냅샷을 삭제하는 경우 영구 삭제하는 대신 휴지통에 넣을 수 있음
- 실수로 삭제한 경우 복구 가능
- 휴지통에 보관되는 기간은 1일에서 1년 사이로 설정 가능

#### Fast Snapshot Restore (FSR)
- 스냅샷을 완전 초기화해 첫 사용에서의 지연 시간을 없애는 기능 (비쌈)

## EBS Volume Types
- 다음 여섯 개의 타입
	1. gp2 / gp3 (SSD): 범용 SSD 볼륨, 다양한 워크로드에 대해 가격과 성능의 절충안
	2. io1 / io2 (SSD): 최고 성능인 SSD 볼륨, 미션 크리티컬이자 레이턴시가 낮고 대용량의 워크로드에 쓰임
	3. st1 (HDD): 저비용의 HDD 볼륨, 잦은 접근과 처리량이 많은 워크로드에 사용
	4. sc1 (HDD): 가장 비용이 적게 드는 HDD 볼륨, 접근 빈도가 낮은 워크로드에 사용
- 나누는 기준은 Size, throughput, IOPS (I/O Ops Per Sec)
- **gp2 / gp3와 io1 / io2 만이 부팅 볼륨으로 사용될 수 있음**
	- Root OS가 실행될 위치에 해당

### General Purpose SSD
- 짧은 지연 시간, **효율적인 비용**의 스토리지
- 시스템 부팅 볼륨에서 가상 데스크탑, 개발, 테스트 환경에서 사용 가능
- 사이즈는 1GB ~ 16TB까지 다양
- gp3
	- 최신 버전
	- 기본 성능으로 3,000 IOPS
	- 125mb/s의 처리량 (Throughput) 제공
	- IOPS는 최대 16,000, 처리량은 1,000mb/s까지 **따로** 증가시킬 수 있음
- gp2
	- 구 버전
	- 볼륨이 더 작음
	- 최대 3,000 IOPS
	- **볼륨과 IOPS가 연결되어 있어 최대 16,000 IOPS까지 증가**
	- **3 IOPS per GB**

### Provisioned IOPS (PIOPS) SSD
- IOPS 성능을 유지할 필요가 있는 주요 비즈니스 애플리케이션에 적합
- 16000 IOPS 이상을 요하는 애플리케이션에 적합
- 일반적으로 데이터베이스 워크로드에 적합 (스토리지 퍼포먼스와 일관성에 민감)
- io1 / io2 (4GB ~ 16TB)
	- Max PIOPS: 64,000 IOPS for Nitro EC2 instances & 32,000 IOPS for other
	- gp3 볼륨처럼 프로비저닝된 IOPS를 스토리지 크기와 독자적으로 증가시킬 수 있음
	- io2는 io1과 동일한 비용으로 내구성과 기기당 IOPS의 수가 더 높음
		- io2를 사용하는 것이 더 합리적
- io2 Block Express (4GB ~ 64TB)
	- 좀 더 고성능 유형의 볼륨
	- 지연시간이 밀리초 미만
	- IOPS 대 GB 비율이 1,000:1일 때 최대 256,000 IOPS가 됨
- EBS Multi-attach 지원

### Hard Disk Drives (HDD)
- 부트 볼륨이 될 수 없음
- 125mb ~ 16TB까지 확장
- st1
	- Throughput optimized HDD
	- 빅 데이터, 데이터 웨어하우스, 로그 처리에 적합
	- 최대 처리량은 초당 500mb, 최대 IOPS는 500
- sc1
	- Cold HDD
	- 아카이브 데이터용
		- 접근 빈도가 낮음
	- 최저 비용으로 데이터를 저장할 때 사용
	- 최대 처리량은 초당 250mb, 최대 IOPS는 250

## EBS Multi-Attach (io1 / io2 family)
- 하나의 EBS 볼륨을 같은 AZ에 있는 여러 EC2 인스턴스에 연결
- 각 인스턴스는 고성능 볼륨에 대한 읽기 및 쓰기 권한을 모두 가짐
	- 동시에 읽고 쓸 수 있음
- 사용 사례
	- 애플리케이션 가용성을 높이기 위해 Teradata처럼 클러스터링된 리눅스 애플리케이션
	- 동시 쓰기 작업을 관리해야하는 애플리케이션
- **같은 AZ에 있는 인스턴스만 가능**
- **한 번에 16개의 EC2 인스턴스만 같은 볼륨에 연결할 수 있음**
- 다중 연결을 실행하려면 **Cluster-aware 파일 시스템**을 사용해야 함 (not XFS, EX4, etc, ...)

## EBS Encryption
- Encrypted EBS 볼륨을 생성하면
	- 저장 데이터가 볼륨 내부에 암호화됨
	- 인스턴스와 볼륨 간의 전송 데이터가 암호화됨
	- 모든 스냅샷이 암호화됨
	- 스냅샷으로 생성한 볼륨이 암호화됨
		- 스냅샷을 복사해 암호화를 푼 것을 다시 암호화 활성화함
- 암호화 및 복호화 매커니즘은 보이지 않게 처리됨 (백그라운드에서 EC2와 EBS가 모두 처리)
- **암호화는 레이턴시에 영향이 거의 없음**
- **KMS에서 암호화 키를 생성해 AES 암호화 표준을 가짐**

### Encryption: Encrypt an unencrypted EBS volume
1. 볼륨의 EBS 스냅샷 생성
2. 복사 기능을 이용해 EBS 스냅샷을 암호화
3. 스냅샷을 이용해 새 EBS 볼륨을 생성하면 볼륨이 암호화됨
4. 암호화된 볼륨을 인스턴스 원본에 연결

# EC2 Instance Store
- EBS 볼륨은 좋은 퍼포먼스를 내지만 한정된 퍼포먼스를 내는 네트워크 드라이브
- 하드웨어 디스크의 높은 퍼포먼스가 필요하면 로컬 EC2 Instance Store 사용
- EC2 인스턴스는 가상 머신이지만 실제로는 물리적 하드웨어에 연결되어 있는데, 이와 같은 서버는 해당 서버에 물리적으로 연결된 디스크 공간을 가짐
	- 해당하는 물리적 서버에 연결된 하드웨어 드라이브 -> EC2 Instance Store
- 높은 I/O performance
- EC2 인스턴스, 즉 인스턴스 스토어를 중지 또는 종료하면 해당 스토리지 또한 손실됨 (Ephemeral, 임시적임)
- 장기적으로 데이터를 보관할 만한 장소가 될 수 없음
- Good for buffer / cache / scratch data / temporary content
- EC2 인스턴스의 서버에 장애가 발생할 경우 해당 EC2 인스턴스가 연결된 하드웨어에도 장애가 발생하므로 데이터 손실에 대한 위험 존재
	- 따라서 EC2 Instance Store를 사용할 때는 필요에 따라 백업이나 복제본을 만들어야 함

# Amazon EFS (Elastic File System)

![Unidade Store and Retrieve Data with AWS | Salesforce Trailhead](https://res.cloudinary.com/hy4kyit2a/f_auto,fl_lossy,q_70/learn/modules/core-aws-services/store-and-retrieve-data-with-aws/images/41882e622c8ba490b48ad64d30ac128c_b-934813-d-d-538-4167-9099-27569-a-28-b-024.png)

![EFS](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/EFS.png)

- EFS: Managed NFS (Network File System)
- 서버리스로 파일 시스템을 관리할 수 있는 AWS 서비스
- **여러 EC2 인스턴스에 마운트될 수 있음**
	- **EC2 인스턴스는 여러 AZ에 있을 수 있음**
- 가용성이 높고 확장성도 높음
- 비쌈 (gp2 EBS 볼륨의 3배 정도)
- Pay per use -> 프로비저닝하지 않아도 됨
- Use cases
	- 콘텐츠 매니지먼트
	- 웹 서버
	- 데이터 공유
	- WordPress
	- 등등
- 내부적으로 NFS 프로토콜 사용
- **EFS에 대한 액세스를 제어하기 위해서는 보안 그룹을 설정해야 함**
- **Linux 기반 AMI랑만 호환됨 (Not Windows)**
- KMS를 사용해 EFS 드라이브에 저장 데이터 암호화를 활성화할 수 있음
- Linux의 표준 파일 시스템(POSIX file system)을 사용하고 표준 파일 API를 가짐
- File system scales automatically -> 미리 프로비저닝하지 않아도 됨

## EFS Scale
- 수천 개의 NFS 클라이언트에서 EFS에 동시 액세스할 수 있게 확장됨
- 처리량은 10GB/s
- 용량을 미리 프로비저닝하지 않아도 네트워크 파일 시스템이 PB 규모로 자동 확장됨

## Performance mode (set at EFS creation time)
- 범용 모드 (General purpose) (default): 낮은 지연시간
	- 웹 서버, CMS, etc, ...
- 최대 I/O 모드 (Max I/O): 높은 지연시간, 처리량, 높은 병렬 처리
	- 빅 데이터, 미디어 처리 작업 등
## Throughput mode (set at EFS creation time)
	- Bursting Throughput mode (default): 1TB 파일 시스템의 데이터 전송 속도는 50mb/s + 100mb/s까지 burst up, 사용 공간이 많을수록 버스팅 용량과 처리량이 늘어남
	- Provisioned Throughput mode: 스토리지 크기에 상관 없이 처리량 설정 가능

## EFS Storage Class (set at EFS creation time)

### Storage Tier 
- Standard tier: 액세스가 빈번한 파일
- EFS-IA (Infrequent Access) tier: 파일을 검색할 경우 검색에 대한 비용 발생, 낮은 비용으로 파일 저장, 수명 주기 정책을 사용해야 함

### Availability and durability
- Standard option: EFS를 다중 AZ에 설정, 프로덕션에 적합 (한 가용 영역이 중단되더라도 EFS 파일 시스템에 영향 X)
- One Zone EFS option: EFS를 하나의 AZ에 설정, 개발에 적합, 기본적으로 백업 활성화됨, EFS-IA tier와 호환됨 (EFS One Zone-IA)

# EBS vs EFS 간략 정리

## EBS
- Elastic Block Store
- 한 번에 하나의 인스턴스에만 연결 가능
- 특정 가용 영역에 한정
- gp2: 디스크 사이즈가 늘어나면 IO도 함께 증가
- io1: IO를 볼륨 크기와 관계 없이 독립적으로 증가시킬 수 있음
	- 중요한 데이터베이스를 실행할 때 좋은 방법
- EBS를 다른 가용 영역으로 옮기고자 할 때
	1. 스냅샷 찍기
	2. 다른 AZ에서 그 스냅샷을 복원
	- EBS의 스냅샷이나 백업을 만들 때는 EBS 볼륨 내의 IO를 전부 사용하게 되므로 인스턴스가 EBS를 사용하지 않을 때만 실행해야 함 (성능 이슈가 생길 수 있음)
- EC2 인스턴스가 종료되면 인스턴스 내의 루트 EBS 볼륨도 기본적으로 종료됨
	- 원할 경우 이 동작은 비활성화 가능
- EBS 드라이브의 크기에 따라 청구됨 (Provisioned)

## EFS
- Elastic File System
- 여러 개의 가용 영역에 걸쳐 무수히 많은 인스턴스에 마운트될 수 있음
- EFS Mount Target을 사용해 특정 AZ에서 EC2 인스턴스들과 EFS 드라이브를 연결해줄 수도 있음
- WordPress와 같은 웹 사이트 파일을 공유할 때도 EFS를 사용
- Linux instance에서만 가능 (POSIX file system이라 Windows에서 구동 불가)
- EFS는 EBS보다 세 배정도 비쌈
	- 스토리지 티어로 EFS-IA를 사용하고 제품 수명 정책을 사용하면 비용 절감 가능
- 사용한 만큼만 청구됨
- EFS는 다수의 인스턴스에 걸쳐 연결해야 하는 네트워크 파일 시스템에 적합

