# Storage

- Stateless Applications
	- Deployment의 각 Pod는 별도의 데이터를 가지고 있지 않으며, 단순히 요청에 대한 응답만 반환
	- 요청이 다음 요청에 영향을 미치지 않음
	- e.g., API Server
- Stateful Applications
	- Pod 내부에 특정 데이터를 보유해야 하는 애플리케이션
	- e.g., Database

## K8s Volumes
### 볼륨의 종류와 액세스 가능 범위

![store](https://github.com/seungwonbased/TIL/blob/main/K8s/assets/store1.png)

## Local Volumes
### emptyDir

- 같은 Pod 내의 컨테이너 간 데이터 공유

### hostPath

- 워커 노드 내의 디렉터리를 볼륨으로 사용해 Pod 간 데이터 공유
- Pod가 다른 노드에 생성되면 이전에 생성한 데이터를 참조할 수 없음
- Pod가 삭제되어도 노드의 디렉터리에는 데이터가 유지됨
- 노드가 꺼지면 마운트된 데이터도 사라짐
- hostPath 볼륨에는 많은 **보안 위험**이 있으며, 가능하면 HostPath를 사용하지 않는 것이 좋음
	- HostPath 볼륨을 사용해야 하는 경우, 필요한 파일 또는 디렉터리로만 범위를 지정하고 ReadOnly로 마운트해야 함

## Persistent Volumes
### NFS

- 네트워크 파일 공유 프로토콜로, 여러 컴퓨터 간에 파일 및 디렉터리를 공유하고 액세스하는데 사용
- nfs-common
	- NFS 클라이언트를 지원한 리눅스 패키지 
	- NFS 서버로부터 파일 및 디렉터리를 마운트하고 읽고 쓸 수 있도록 설정

### PV & PVC (Persistent Volume & Persistent Volume Claim)

![store](https://github.com/seungwonbased/TIL/blob/main/K8s/assets/store2.png)

- 볼륨과 애플리케이션의 정의가 서로 밀접하게 연결되어 있어 분리의 어려움이 있음
- PV, PVC를 통해 Pod가 볼륨의 세부적인 사항을 몰라도 볼륨을 사용할 수 있도록 추상화
- PV
	- 볼륨 그 자체
	- 클러스터 안에서 자원으로 다룸
	- Pod와는 별개로 관리되며 별도의 생명 주기가 있음
- PVC
	- 사용자가 PV에 하는 요청
		- 사용하고 싶은 용량은 얼마인지, 읽기/쓰기는 어떤 모드로 설정하고 싶은지 등
- 쿠버네티스 볼륨을 Pod에 직접 할당하는 방식이 아니라 중간에 PVC를 둬 Pod가 사용할 스토리지를 분리
	- Pod는 어떤 스토리지를 사용하는 지 신경 쓰지 않아도 됨

#### PV & PVC 생명주기

![store](https://github.com/seungwonbased/TIL/blob/main/K8s/assets/store3.png)

1. Provisioning
	- PV를 만드는 단계
	- Static: PV를 미리 만들어두고 사용하는 방법
		- 사용할 수 있는 스토리지 용량이 제한적일 때 유용
	- Dynamic: 요청이 있을 때마다 PV를 만드는 방법
		- 사용자가 원하는 용량만큼 생성해서 사용 가능
2. Binding
	- 프로비저닝으로 만든 PV와 PVC를 연결하는 단계
	- PV와 PVC의 매핑은 1:1 관계로 PVC 하나가 여러 개 PV에 할당될 수 없음
3. Using
	- PVC가 Pod에 설정되고 Pod는 PVC를 볼륨으로 인식해서 사용
	- 할당된 PVC는 Pod를 유지하는 동안 계속 사용하면 시스템에서 임의로 삭제 불가
4. Reclaiming
	- 사용이 끝난 PVC는 삭제되고 PVC를 사용하던 PV를 초기화(Reclaiming)하는 과정
	- Reclaim Policy
		- Retain
			- PVC가 삭제되어도 저장소에 있던 파일을 삭제하지 않는 정책
				- PV는 수동으로 삭제해야 함
			- 볼륨이 Released되어서 다시 바인딩할 수 없어도 새로 볼륨을 만들고 경로를 똑같게 하면 그대로 데이터를 다시 쓸 수 있음
		- Delete
			- PVC가 삭제되면 PV와 연결된 저장소 자체를 삭제
			- NFS와 같은 스토리지 타입에서는 Delete가 제대로 적용되지 않음
				- 직접 삭제해야 함
			- CSP가 제공하는 서비스에서는 동적으로 스토리지를 프로비저닝하기 때문에 Delete 정책을 사용할 수 있음
		- Recycle
			- PVC가 삭제되면 PV와 연결된 저장소 데이터는 삭제하지만 저장소 볼륨 자체는 삭제하지 않고 유지

#### PV Access Modes
- RWO (= ReadWriteOnce)
	- 하나의 노드에서 해당 볼륨이 읽기-쓰기로 마운트됨
- ROX (= ReadOnlyMany)
	- 볼륨이 많은 노드에서 읽기 전용으로 마운트됨
- RWX (= ReadWriteMany)
	- 볼륨이 많은 노드에서 읽기-쓰기로 마운트됨
- RWOP (= ReadWriteOncePod)
	- 볼륨이 단일 파드에서 읽기-쓰기로 마운트됨
	- 전체 클러스터에서 단 하나의 파드만 해당 PVC를 읽거나 쓸 수 있어야 하는 경우에 사용

#### PV Status
- Available (= 사용 가능)
	- 아직 클레임에 바인딩되지 않은 사용할 수 있는 리소스
- Bound (= 바인딩됨)
	- 볼륨이 클레임에 바인딩됨
- Released (= 릴리스)
	- 클레임이 삭제되었지만 클러스터에서 아직 리소스를 반환하지 않음
	- 이 경우, PV는 더 이상 어떤 PVC에도 바운드되지 않고, 더 이상 사용되지 않는 상태
- Failed (= 실패)
	-  볼륨이 자동 반환에 실패