# 필수 개념 및 명령어

## 종료

```bash
$ shutdown -p now
$ halt -p now
$ init 0
```

## 시스템 재부팅

```bash
$ shutdown -r now
$ reboot
$ init 6
```

## 로그아웃

```bash
$ logout
$ exit
```

## 가상 콘솔

- 가상의 모니터라고 생각하면 됨
- 각각의 가상 콘솔로 이동하는 단축키는 Ctrl + Alt + F3 ~ F6

## Runlevel

| Runlevel |  Eng-mod   |              Desc              |       Note       |
| :------: | :--------: | :----------------------------: | :--------------: |
|    0     | Power Off  |           종료 모드            |                  |
|    1     |   Rescue   |        시스템 복구 모드        | 단일 사용자 모드 |
|    2     | Multi-User |                                |  사용하지 않음   |
|    3     | Multi-User | 텍스트 모드의 다중 사용자 모드 |                  |
|    4     | Multi-User |                                |  사용하지 않음   |
|    5     | Graphical  | 그래픽 모드의 다중 사용자 모드 |                  |
|    6     |   Reboot   |                                |                  |

### 예시

```bash
$ ln –sf /lib/system/system/multi-user.target /etc/systemd/system/default.target
```

- Runlevel n으로 부팅할 수 있도록 해주는 설정 파일

  - default.target이 multi-user.target을 참조해 부팅하도록 함

## 자동 완성과 히스토리

- 자동 완성: Tab 키 활용
- 히스토리: history

## vi 기능 요약

### 명령 모드 -> 입력 모드

- i: 현재 커서 위치부터 입력
- a: 현재 커서의 위치 다음 칸부터 입력
- I: 현재 커서 줄의 맨 앞에서부터 입력
- A: 현재 커서 줄의 맨 뒤부터 입력

### 명령 모드에서 커서 이동

- h: 왼쪽
- k: 위
- j: 아래
- l: 오른쪽

### 명령 모드에서 삭제, 복사 붙여넣기

- x: 현재 커서가 위치한 글자 삭제
- X: 현재 커서가 위치한 앞 글자 삭제
- dd: 현재 커서의 행 삭제
- number + dd: 현재 커서부터 숫자만큼의 행 삭제
- yy: 현재 커서가 있는 행을 복사
- number + yy: 현재 커서부터 숫자만큼의 행을 복사
- p: 복사한 내용을 현재 행 이후에 붙여넣기
- P: 복사한 내용을 현재 행 이전에 붙여넣기

## Mount

- 보조기억장치나 파일 시스템이 다른 디스크를 /의 하위 디렉터리로 연결해서 사용해주는 것
- 요즘은 Plug and Play 기능이 있어 자동으로 등록하지만, 서버 환경에서는 없는 경우도 많음

```bash
$ mount [option] [device] [directory]
```

## 기본 명령어

- ls: 디렉터리에 있는 파일 목록을 나열
- cd: 디렉터리 이동
- pwd: 현재 디렉터리 전체 경로
- rm: 삭제
- cp: 복사
- touch: 크기가 0인 파일 생성
- mv: 파일의 이름을 변경하거나 위치 이동
- mkdir: 새로운 디렉터리 생성
- rmdir: 디렉터리 삭제
- cat: 텍스트로 작성된 파일 출력
- head, tail: 앞 10행 또는 뒤 10행 출력
- more: 텍스트 파일을 페이지 단위로 출력
- less: more와 용도가 비슷하지만 확장된 기능
- file: 어떤 종류의 파일인지 출력
- clear: 명령창 클리어

## 사용자와 그룹

- 리눅스는 Multi-User system
- root라는 이름을 가진 Superuser가 있으며 모든 작업을 할 수 있는 권한이 있음
- 모든 사용자는 하나 이상의 그룹에 소속되어 있음
- 사용자는 /etc/passwd 파일에 정의되어 있음
- 사용자의 비밀번호는 /etc/shadow 파일에 정의되어 있음
- 그룹은 /etc/group 파일에 정의되어 있음
- /etc/skel 디렉터리는 사용자가 처음 생성되면 기본으로 포함해야 할 정보를 가짐

### 사용자와 그룹 관련 명령어

- useradd: 새로운 사용자 추가
- passwd: 사용자의 비밀번호 지정 또는 변경
- usermod: 사용자의 속성 변경
- userdel: 사용자 삭제
- change: 사용자의 암호를 주기적으로 변경하도록 설정
- groups: 사용자가 속한 그룹을 보여줌
- groupadd: 새로운 그룹 생성
- groupmod: 그룹 속성 변경
- groupdel: 그룹 삭제
- gpasswd: 그룹 암호 설정 또는 그룹 관리 수행

## 파일과 디렉터리의 소유와 허가권

![FilePemission](https://github.com/seungwonbased/TIL/blob/main/Linux/assets/Screenshot_2023-08-20_at_20.41.08.png)

- 파일 유형
  - 디렉터리일 경우에는 d, 일반적인 파일일 경우에는 -
- 파일 허가권
  - 3개씩 끊어서 읽음
    - 첫 번째는 User의 파일 접근 권한
    - 두 번째는 Group의 파일 접근 권한
    - 세 번째는 그 외 사용자의 파일 접근 권한
  - 8진수 숫자로도 표현 가능
  - chmod: 파일 허가권 변경 명령어
- 파일 소유권: 파일을 소유한 사용자와 그룹을 의미
  - chown/chgrp: 파일의 소유권을 변경하는 명령어

## INODE

- inode는 리눅스/유닉스 파일 시스템에서 사용하는 자료구조
- 파일이나 디렉터리의 여러 정보를 가지고 있음
- 모든 파일이나 디렉터리는 한 개의 inode를 가지며 inode에는 파일의 소유권, 허가권, 파일 종류 등의 정보와 해당 파일의 실제 위치가 있음
- inode들이 모여있는 공간을 inode 블록이라고 함

## 링크

- 파일의 링크에는 하드 링크와 심볼릭 링크 (또는 소프트 링크) 두 가지가 있음
- 하드 링크를 생성하면 하드 링크 파일만 하나 생성되며 같은 inode를 사용
- 소프트 링크를 생성하면 새로운 inode를 만들고 데이터는 원본 파일을 연결하는 효과

## RPM (Redhat Package Manager)

- Windows의 "setup.exe"와 비슷한 설치 파일
- 확장명은 .rpm이며 이를 패키지라고 부름

```bash
# U는 설치, v는 설치 과정, h는 설치 시 진행도를 #으로 표현하는 옵션

$ rpm -Uvh PACKAGENAME
```

- RPM은 포로그램을 설치할 때 의존성이 있는 선행 패키지가 설치되어 있지 않으면 설치가 불가능하다는 게 단점

## DNF (Danifired YUM)

- RPM의 패키지 의존성 문제를 해결
- 인터넷을 통해 필요한 파일을 Repository에서 자동으로 다운로드해서 설치

### 작동 방식 설정 파일

- /etc/yum.conf: 특별히 변경할 필요 없음
- /etc/yum.repos.d/ 디렉터리: dnf 명령을 입력했을 때 검색하게 되는 네트워크의 주소가 들어 있는 여러 개의 파일이 있음
- /etc/yum.repos.d/ 디렉터리의 .repo 파일: 패키지를 받아올 위치를 결정

## 파일 압축과 묶기

### 파일 압축

- 압축 파일 확장명은 xz, bz2, gz, zip, Z 등
  - xz나 bz2 압축률 좋음

#### 파일 압축 관련 명령

- xz: xz로 압축을 하거나 해제
- bzip2: bz2로 압축을 하거나 해제
- gzip: gz로 압축을 하거나 해제

### 파일 묶기

- 리눅스나 유닉스에서는 파일 압축과 파일 묶기는 별개의 프로그램으로 수행
- 파일 묶기의 명령어는 tar이며 묶인 파일의 확장명도 .tar

#### 파일 묶기 명령

- 확장명 tar로 묶음 파일을 만들어 주거나 묶음을 풀어줌
  - 동작: c(묶기), x(풀기), t(경로 확인)
  - 옵션: f(파일), v(과정 보이기), J(tar + xz), z(tar + gz), j(tar + bz2)

## 파일 위치 검색

```bash
# 기본 파일 찾기

$ find [경로] [옵션] [조건] [action]
```

## Cron과 At

### Cron

- 주기적으로 반복되는 일을 자동적으로 실행될 수 있도록 설정
- 관련된 데몬은 crond, 관련 파일은 /etc/crontab

### at

- cron은 주기적으로 반복되는 작업을 예약하는 것이지만 at은 일회성 작업을 예약

## 네트워크 관련 명령어

- nmtui: 네트워크와 관련된 대부분의 작업을 수행
  - 자동 IP 주소 또는 고정 IP 주소 사용 결정
  - IP 주소, 서브넷 마스크, 게이트웨이 정보 입력
  - DNS 정보 입력
  - 네트워크 카드 드라이버 설정
  - 네트워크 장치 (ens160) 설정
  - 텍스트 기반으로 작동
- nslookup: DNS 서버의 작동을 테스트하는 명령어

```bash
# 네트워크 설정을 변경한 후에 변경된 내용을 시스템에 적용시키는 명령어

$ systemctl [start/stop/restart/status] NetworkManager
```

```bash
# 네트워크 장치를 On 또는 Off 시키는 명령어

$ ifup [장치 이름]
$ ifdown [장치 이름]
```

```bash
# 장치의 IP 주소 설정 정보를 출력

$ ifconfig [장치 이름]
```

```bash
# 해당 컴퓨터가 네트워크 상에서 응답하는 지를 테스트하는 명령어

$ ping [IP 주소 또는 URL]
```

## 네트워크 설정과 관련된 주요 파일

- /etc/sysconfig/network: 네트워크의 기본적인 정보가 설정되어 있는 파일
- /etc/sysconfig/network-scripts/ifcfg-ens160: ens 장치에 설정된 네트워크 정보가 있는 파일
- /etc/resolv.conf: DNS 서버의 정보 및 호스트 이름이 있는 파일
- /etc/hosts: 현 컴퓨터의 호스트 이름 및 FQDN이 있는 파일
- 위 네 개 파일을 직접 편집하면 nmutui들 사용하지 않아도 됨

## Pipe, Filter, Redirection

### Pipe

- 두 개의 프로그램을 연결해주는 연결 통로
- "|" 문자 사용

```bash
$ ls -l /etc | more
```

### Filter

- 필요한 것만 걸러주는 명령어
- grep, tail, wc, sort, awk, sed 등
- 주로 파이프와 같이 사용

```bash
$ ps -ef | grep bash
```

### Redirection

- 표준 입출력의 방향을 바꿔줌
- ">" 문자 사용

```bash
$ ls - l > list.txt
```

## Process, Daemon

- 정의: 하드디스크에 저장된 실행코드가 메모리에 로딩되어 활성화된 것
- Foreground process
  - 실행하면 화면에 나타나 사용자와 상호작용하는 프로세스
  - 대부분의 응용 프로그램
- Background process
  - 실행은 되었지만 화면에는 나타나지 않고 실행되는 프로세스
  - 서버 데몬 등
- Process number: 각각의 프로세스에 할당된 고유 번호
- 작업 번호: 현재 실행되고 있는 백그라운드 프로세스의 순차 번호

### 부모 프로세스와 자식 프로세스

- 모든 프로세스는 부모 프로세스를 가지고 있음
- 부모 프로세스를 kill 하면 자식 프로세스도 자동으로 kill 됨

### 프로세스 관련 명령

- ps: 현재 프로세스의 상태를 확인하는 명령어
- kill: 프로세스를 강제로 종료하는 명령어
- pstree: 프로세스의 관계를 트리 형태로 보여줌

## Service, Socket

### Service

- 시스템과 독자적으로 구동되어 제공하는 프로세스
  - 웹 서버, DB 서버, FTP 서버 등
- 실행 및 종료는 대개 "systemctl start/stop/restart SERVICENAME"으로 사용됨
- 서비스의 실행 스크립트 파일은 /usr/lib/systemd/system/ 디렉터리에 SERVICENAME.service 라는 이름으로 확인할 수 있음

### Socket

- 서비스는 항상 가동되지만 소켓은 외부에서 특정 서비스를 요청할 경우에 systemd가 구동시킴
- 요청이 끝나면 소켓도 종료됨
- 소켓으로 설정된 서비스를 요청할 때는 처음 연결되는 시간이 서비스에 비교했을 때 약간 더 걸릴 수 있음
  - systemd가 서비스를 새로 구동하는 데 시간이 소요되기 대문
- Telnet 서버 등
- 소켓과 관련된 스크립트 파일은 /usr/lib/systemd/system/ 디렉터리에 SOCKETNAME.socket 이라는 이름으로 존재

## GRUB 부트로더

- 부트 정보를 사용자가 임의로 변경해 부팅할 수 있음
  - 즉, 부트 정보가 올바르지 않더라도 수정하여 부팅할 수 있음
- 다른 여러 가지 운영체제와 멀티부팅 가능
- 대화형 설정을 제공
- 비밀번호 분실 시 GRUB 부트로더로 해결 가능

## 모듈의 개념과 커널 컴파일의 필요성

![Module](https://github.com/seungwonbased/TIL/blob/main/Linux/assets/Module.png)

- 모듈: 필요할 때마다 호출하여 사용되는 코드

![KernelCompile](https://github.com/seungwonbased/TIL/blob/main/Linux/assets/KernelCompile.png)

## IDE, SATA, SCSI

- 리눅스에서 하드디스크가 물리적으로 나눠진 것을 /dev/sda, /dev/sdb, /dev/sdc, ... 형식으로 부름
- 디스크 파티션이 논리적으로 나눠진 것을 /dev/sda1, /dev/sda2, /dev/sda3, ... 형식으로 부름

### IDE (Intergrated Drive Electronics)

- 일반적인 메인보드에서 지원되는 하드디스크
- ODD 등의 장치들의 인터페이스를 총칭

### ATA (Adavanced Technology Attachment)

- CD-ROM 드라이브나 하드디스크 장치 등을 IDE 인터페이스에 접속하기 위한 규격, 연결 방식
- PATA: Parallel ATA
- SATA: Serial ATA

### SCSI (Small Computing System Interface)

- 서버 환경이나 워크스테이션 등에 주로 이용되는 장치
  - 빠른 RPM
  - 빠른 전송률
  - 긴 수명
  - 안정적 데이터 보존률

## 하드디스크 추가하기

- 파티션을 그냥 사용할 수 없으며 반드시 특정한 디렉터리에 마운트 시켜야 사용 가능
- fdisk: 파티션을 나눠주는 명령어
- Partition type
  - Primary Partition
    - 실제 데이터를 저장하는 파티션
    - 1 ~ 4개 까지 생성 가능
  - Extended Partition
    - 실제 데이터 저장이 불가능한 파티션
    - 파티션 테이블 영역을 확장하는 용도
    - 파일 시스템 및 마운트 지정 불가능
    - 단 한 개의 파티션만 생성 가능
  - Logical Partition
    - 데이터 저장은 가능하나 운영체제 설치 불가
    - 확장 영역의 크기 내에서 분할해서 사용 가능
- mkfs: 파티션을 원하는 파일 시스템으로 포맷하는 명령어
- fdisk를 사용한 파티션 분할, mkfs로 디스크 포맷까지 완료했다면 mount를 통해 사용 가능하도록 등록해야 함 (디렉터리 생성 후 mount)
  - mount 후 /etc/fstab 파일에 설정으로 입력해 재부팅 후에도 자동으로 마운트 되어있을 수 있도록 함

## RAID

- 여러 개의 디스크를 하나의 디스크처럼 사용하는 방식
- 하드웨어 RAID와 소프트웨어 RAID로 구분
- 비용 절감 + 신뢰성 향상 + 성능 향상의 효과
- mdadm: RAID 생성

### 각 RAID 방식의 비교

![RAID](https://github.com/seungwonbased/TIL/blob/main/Linux/assets/RAID.png)

#### Linear RAID

- 최소 두 개의 하드디스크가 필요
- 두 개 이상의 하드디스크를 한 개의 볼륨으로 사용
- 앞 디스크부터 차례로 저장
- 100%의 공간 효율성 (비용 저렴)

#### RAID 0

- 최소 두 개의 하드디스크가 필요
- 모든 디스크에 동시에 저장됨
- 100%의 공간 효율성 (비용 저렴)
- 신뢰성 낮음
- 빠른 성능을 요구하되 전부 잃어버려도 큰 문제가 되지 않는 자료가 적당

#### RAID 1

- Mirroring이라고 부름
- 데이터 저장에 두 배의 용량이 필요
- Fault-tolerance 제공 (신뢰성 높음)
- 공간 효율 나쁨
- 저장 속도, 성능은 변화 없음

#### RAID 5

![RAID5](https://github.com/seungwonbased/TIL/blob/main/Linux/assets/RAID5.png)

- RAID 1의 안정성, + RAID 0의 공간 효율성
- 최소 세 개 이상의 하드디스크
- 오류가 발생했을 때는 Parity를 이용해 데이터 복구
- 디스크 개수 - 1의 공간을 사용
- 어느 정도 결함을 허용해 주면서 공간 효율이 좋음

#### RAID 6

- RAID 6 방식은 RAID 5 방식이 개선된 것
- 공간 효율은 RAID 5보다 약간 떨어지지만 두 개의 디스크가 동시에 고장나도 데이터에는 이상이 없도록 함
- 최소 네 개의 하드디스크 필요
- 공간 효율, 성능은 RAID 5보다 약간 떨어지는 반면에 데이터에 대한 성뢰도는 좀 더 높아지는 효과

#### RAID 10

- 최소 네 개의 하드디스크 필요
- 두 개의 디스크를 RAID 0으로 묶어 용량을 늘리고, 각 pair를 또 묶어서 복제본을 만듦
- 신뢰성과 성능을 모두 잡을 수 있지만, 디스크의 낭비가 발생

## LVM (Logical Volumn Manager)

![LVM](https://github.com/seungwonbased/TIL/blob/main/Linux/assets/LVM.png)

- 하드디스크를 통합한 뒤 논리적인 크기로 분할하는 기법
- 여러 개의 하드디스크를 한 개의 파일 시스템으로 사용

### 구성

-
