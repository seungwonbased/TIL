# StatefulSet

> https://kubernetes.io/ko/docs/concepts/workloads/controllers/statefulset/

- 애플리케이션의 Stateful을 관리하는 데 사용
- Pod 집합의 Deployment와 스케일링을 관리하고 Pod들의 순서 및 고유성을 보장
- PV와 Pod를 함께 조합해서 제어
- Deployment와 다르게, StatefulSet은 각 Pod의 독자성을 유지
	- Pod들은 동일한 스펙으로 생성되지만, 서로 교체는 불가능

## Deployment vs StatefulSet

![sfs](https://github.com/seungwonbased/TIL/blob/main/K8s/assets/sfs1.png)
- Headless 서비스
	- 다른 서비스 유형과 달리 Cluster IP를 생성하지 않고, 각 Pod에 고유한 DNS 이름을 부여해 네트워크 통신을 지원
	- 주로 내부 네트워크 통신 및 서비스 디스커버리를 위한 목적으로 사용되며, 외부로 노출되지 않는 경우가 많음

![sfs](https://github.com/seungwonbased/TIL/blob/main/K8s/assets/sfs2.png)

![sfs](https://github.com/seungwonbased/TIL/blob/main/K8s/assets/sfs3.png)
- Deployment
	- 노드가 응답을 하지 않으면 다른 노드에 Pod를 대체 가동
- StatefulSet
	- 노드가 응답을 하지 않아도 노드가 확실히 죽거나 클러스터에서 삭제된 게 아니라면 대체 Pod를 가동하지 않음

