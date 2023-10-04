# Vagrant

- 가상화 환경을 구축하고 관리하기 위한 오픈 소스 도구
- 가상 머신의 생성, 시작, 정지, 삭제 등의 작업을 CLI을 통해 간단하게 수행
- 다양한 가상화 플랫폼을 지원 (VirtualBox, VMware, Hyper-V, Docker 등)

## 명령어

- vagrant init
	- Vagrantfile을 생성
- vagrant up
	- Vagrantfile을 기반으로 가상 환경 프로비저닝 진행
- vagrant halt
	- 가상 머신을 종료
- vagrant destroy
	- 가상 머신을 삭제
- vagrant ssh
	- 가상 머신에 원격 쉘 접속
- vagrant provision
	- 가상 머신의 설정을 변경하고 적용

## Troubleshooting

### 1. 노드 상태 확인

```bash
vagrant@master: sudo su
```

```bash
root@master: kubectl get node
```

- 만약, NotReady인 노드가 있다면 2.로

### 2. SSH 상태 확인

```bash
root@master: exit
```

```bash
vagrant@master: ssh 192.168.10.110
```

### 3.1. SSH 접속 성공했을 경우: kubelet 서비스 재시작

```bash
root@worker-1: sudo systemctl restart kubelet.service
```

### 3.2. SSH 접속 실패했을 경우: 노드 재시작

```cmd
c:\k8s: vagrant halt worker-1
c:\k8s: vagrant up worker-1
```
