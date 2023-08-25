# EBS Volume

![Unité Choose the Right Storage Service | Salesforce Trailhead](https://res.cloudinary.com/hy4kyit2a/f_auto,fl_lossy,q_70/learn/modules/aws-storage/choose-the-right-storage-service/images/75c6bec122ddc0a1a76b0bf99a89cae0_2-c-235-e-2-f-2448-40-c-3-8-c-7-b-e-9753-d-6-b-0-df-5.png)

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

