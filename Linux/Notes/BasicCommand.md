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

![파일 허가권](https://github.com/seungwonbased/TIL/blob/main/Linix/asset/Screenshot_2023-08-20_at_20.41.08.png)

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
-
