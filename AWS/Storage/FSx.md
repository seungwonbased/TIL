# Amazon FSx
## Overview

![fsx1](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/fsx2.png)

- Amazon FSx는 AWS에서 완전 관리형 서비스로 타사 고성능 파일 시스템을 실행시키ㅁ
- 가령 RDS에서 AWS에 MySQL나 Postgres를 실행하는 것과 같은 개념
	- RDS가 FSx로 바뀌었고 파일 시스템을 실행한다는 점이 다름
- 예시
	- FSx에 Lustre를 실행 가능
	- Windows File Server를 실행할 수도 있음
	- NetApp ONTAP나 OpenZFS가 될 수도 있음

## Amazon FSx for Windows (File Server)

- FSx for Windows File Server는 완전 관리형 Windows 파일 서버 공유 드라이브로 Windows를 사용하기 때문에 SMB 프로토콜과 Windows NTFS를 지원
- Microsoft Active Directory 통합을 지원하므로 사용자 보안을 추가할 수 있고 ACL로 사용자 할당량을 추가해 액세스를 제어할 수 있음
- 특징
	- 겉보기에는 Windows에서만 사용할 수 있는 것 같지만 Linux EC2 인스턴스에도 마운트할 수 있단 걸 기억
	- 기존에 온프레미스 등에 Windows 파일 서버가 있는 경우 Microsoft 분산 파일 시스템인 DFS 기능을 이용해서 파일 시스템을 그룹화 가능
		- 이렇게 하면 온프레미스의 Windows 파일 서버와 FSx for Windows File Server 결합 가능
- 성능
	- 초당 수십 GB에 수백만 IOPS 그리고 수백 PB의 데이터까지 확장될 수 있음
- 스토리지 옵션
	- SSD: 지연 시간이 짧아야 하는 워크로드를 저장 가능
		- 데이터베이스, 미디어 처리 데이터 분석 등
	- HDD: 더 비용이 쌈, 넓은 스펙트럼의 워크로드를 저장 가능
		- 홈 디렉터리나 CMS 등
- 프라이빗 연결로 온프레미스 인프라에서 액세스할 수 있음
- 고가용성 다중 AZ에 대해 FSx for Windows File Server를 구성 가능
- 모든 데이터는 재해 복구 목적으로 Amazon S3에 매일 백업됨

## Amazon FSx for Lustre

- Lustre는 원래 분산 파일 시스템으로 대형 연산에 쓰였음
- Lustre의 이름
	- Lustre는 Linux와 클러스터(Cluster)를 합친 단어로 머신 러닝과 HPC, 즉 고성능 연산에 쓰임
- 사용 사례
	- 동영상 처리나 금융 모델링 전자 설계 자동화 등의 애플리케이션에서 쓰임
- 특징
	- 확장성이 상당히 높음
- 성능
	- 초당 수백 GB의 데이터에 수백만 IOPS로 확장되고 밀리초보다 짧은 지연 시간을 자랑
- 스토리지 옵션
	- SSD: 낮은 지연 시간, 워크로드가 많거나, 크기가 작은 무작위 파일 작업이 많으면 IOPS도 사용 가능
	- HDD: 처리량이 많은 워크로드나 크기가 큰 시퀀스 파일 작업
- Amazon S3와 무결절성 통합이 가능한데, 다시 말하면 FSx로 S3를 파일 시스템처럼 읽어들일 수 있음
	- 또한 FSx의 연산 출력값을 다시 Amazon S3에 쓸 수 있음
- VPN과 직접 연결을 통해 온프레미스 서버에서 사용 가능

### FSx Lustre - File System Deployment Options

![fsx2](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/fsx1.png)

- FSx의 파일 시스템 배포 옵션
- 스크래치 파일 시스템
	- 임시 스토리지로 데이터가 복제되지 않음
		- 즉 기저 서버가 오작동하면 파일이 모두 유실됨
	- 최적화로 초과 버스트를 사용할 수 있음
		- 영구 파일 시스템보다 성능을 여섯 배 높일 수 있음
		- TiB 처리량당 초당 200MB의 속도
	- 스크래치 파일 시스템은 단기 처리 데이터에 쓰이며 데이터 복제가 없어 비용을 최적화할 수 있음
	- FSx가 있으면 컴퓨팅(Compute) 인스턴스가 AZ 1과 AZ 2에 연결하는데, 이때 FSx for Lustre 스크래치 파일 시스템을 사용하면 위 사진처럼 데이터의 사본이 하나만 존재함
	- 또한 데이터 저장소에 추가로 S3 버킷을 둘 수도 있음
	- 
- 영구 파일 시스템
	- 장기 스토리지로 동일한 가용 영역에 데이터가 복제됨
		- AZ 간은 아니라 동일한 AZ 내에서만 복제됨
	- 다시 말하면 기저 서버가 오작동했을 때 단 몇분 내에 해당 파일이 대체된다는 것
	- 영구 파일 시스템의 예시는 민감한 데이터의 장기 처리 및 스토리지
	 - FSx for Lustre는 단일 AZ에만 있고, FSx for Lustre 파일 시스템의 영구 모드에는 데이터 사본이 두 개 생기는데 첫 번째 데이터 볼륨에 복제본이 하나, 두 번째 데이터 볼륨에 하나가 있음

## Amazon FSx for NetApp ONTAP

![fsx3](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/fsx3.png)

- AWS의 관리형 NetApp ONTAP 파일 시스템으로 NFS, SMB, iSCSI 프로토콜과 호환 가능
- FSx for NetApp ONTAP 파일 시스템을 사용하여 온프레미스 시스템의 ONTAP이나 NAS에서 실행 중인 워크로드를 AWS로 옮길 수 있음
- 다양한 운영 체제에서 사용 가능
	- Linux, Windows, MacOS AWS의 VMware Cloud, Workspaces, Appstream, EC2, ECS, 그리고 EKS
	- 즉, 호환 가능한 폭이 아주 넓음
- 스토리지는 자동으로 확장 및 축소됨
	- 오토스케일링
- 복제와 스냅샷 기능도 지원
- 비용도 적게 들고 데이터 압축이나 데이터 중복제거도 가능
	- NetApp ONTAP에서 중복 파일을 찾을 수 있음
- 지정 시간 복제 기능이 있는데, 새 워크로드 등을 테스트할 때 상당히 유용
	- 파일 시스템에서 신속히 복제가 가능하고 스테이징 파일 시스템을 둘 수 있음

## Amazon FSx for OpenZFS

![fsx4](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/fsx4.png)

- Amazon FSx for OpenZFS는 AWS의 관리형 OpenZFS 파일 시스템으로 여러 버전에서의 NFS 프로토콜과 호환이 가능
- 주로 ZFS에서 실행되는 워크로드를 내부적으로 AWS로 옮길 때 사용됨
- Linux, Mac, Windows에서 사용 가능
- 성능이 상당히 좋아서 백만 IOPS까지 확장 가능하고 지연 시간은 0.5 밀리초 이하
- 스냅샷, 압축을 지원하고 비용이 적지만 데이터 중복제거 기능은 없음
- NetApp ONTAP처럼 지정 시간 동시 복제 기능이 있어 새 워크로드 테스트 시에 유용