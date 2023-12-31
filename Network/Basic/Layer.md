# Layer

- 비음성 통신에서 데이터를 전송하기 위한 일련의 단계, 과정, 절차
- 송신자 운영체제가 응용 계층에서 시작해 전송, 네트워크, 데이터 링크 계층 순으로 데이터를 전송
- 수신자 운영체제가 역으로 물리 계층에서 시작해 데이터 링크, 네트워크, 응용 계층 순으로 데이터를 수신

## OSI 7 Layer

![lay](https://github.com/seungwonbased/TIL/blob/main/Network/assets/lay1.png)

### L1, 물리 계층

- 들어온 전기 신호를 그대로 전달
	- 들어온 전기 신호를 재생성해 내보냄
	- 주소 개념이 없으므로 전기 신호가 들어온 포트를 제외하고 모든 포트에 같은 전기 신호를 전송
- 허브, 리피터, 케이블, 커넥터, 트랜시버, 탭
	- 허브, 리피터: 네트워크 통신을 중재하는 네트워크 장비
	- 케이블, 커넥터: 케이블
	- 트랜시버: 컴퓨터의 랜 카드와 케이블을 연결하는 장치
	- 탭: 네트워크 모니터링과 패킷 분석을 위해 전기 신호를 다른 장비로 복제

### L2, 데이터 링크 계층

- 전기 신호를 모아 우리가 알아볼 수 있는 데이터 형태로 처리 
- 주소 정보를 정의하고 정확한 주소로 통신이 되도록 하는 데 초점
- 흐름 제어 (Flow Control): 수신자 상태에 따라 데이터를 전송    
- MAC 주소
- NIC, 스위치 (Switch)

### L3, 네트워크 계층

- IP 주소와 같은 논리적인 주소(사용자 환경에 맞게 변경해 사용 가능)가 정의됨    
- 라우터 (Router): IP 주소를 사용해 최적의 경로를 찾아주고 해당 경로로 패킷을 전송하는 역할

### L4, 전송 계층

- 패킷이 유실되거나 순서가 바뀌었을 때 바로잡아 주는 역할
- 시퀀스 번호: 패킷에 보내는 순서를 명시한 것
- ACK 번호: 받는 순서를 나타내는 것
- 포트 번호를 사용해 상위 애플리케이션을 구분
- 로드밸런서, 방화벽 

### L5, 세션 계층

- 양 끝단의 응용 프로세스가 연결을 성립하도록 도와주고 연결이 안정적으로 유지되도록 관리하고 작업 완료 후 연결을 끊는 역할
- TCP/IP 세션을 만들고 없앨 책임이 있음
- 에러로 중단된 통신에 대한 에러 복구와 재전송도 수행

### L6, 표현 계층

- 표현 방식이 다른 애플리케이션이나 시스템 간의 통신을 돕기 위해 하나의 통일된 구문 형식으로 변환시키는 기능
- MIME 인코딩, 암호화, 압축, 코드 변환 등

### L7, 응용 계층

- 애플리케이션 프로세스를 정의하고 애플리케이션 서비스를 수행

## TCP / IP Layer

![lay](https://github.com/seungwonbased/TIL/blob/main/Network/assets/lay2.png)

## 전송 데이터 구성

|---헤더 3---|---헤더 2---|---헤더 1---|---페이로드---|
|---|---|---|---|

- 페이로드 (Payload): 사용자가 상대방에게 전송하고자 하는 실제 정보가 담긴 공간
- 헤더 (Header): 보내는 사람의 주소(출발지 주소)와 받는 사람의 주소(목적지 주소)가 담긴 공간
- 메시지 (Message): 페이로드만으로 이루어진 데이터 전송 단위

### Datagram or Segment


|데이터그램 / 세그먼트 헤더|UDP / TCP 페이로드|
|---|---|


- Datagram: 데이터그램 헤더 + UDP 페이로드 
- Segment: 세그먼트 헤더 + TCP 페이로드
- Datagram or Segment Header
	- 페이로드 앞에 붙는 첫번째 헤더 
		- UDP 페이로드 → 데이터그램 헤더
		- TCP 페이로드 → 세그먼트 헤더
	- 출발지/목적지 포트 번호를 포함
- 수신측 운영체제에서 데이터그램 또는 세그먼트 헤더에 담긴 포트 번호를 통해 페이로드의 내용을 어떤 어플리케이션 처리할지를 판단
	- 목적지 포트: 일반적으로 서비스 포트 번호를 사용
	- 출발지 포트: 1024번 이후의 포트 번호를 사용

### Packet

|패킷 헤더|데이터그램 / 세그먼트 헤더|UDP / TCP 페이로드|
|---|---|---|

- 패킷 헤더
	- 데이터그램 / 세그먼트 앞에 붙는 두번째 헤더
	- 출발지 / 목적지 **IP 주소**를 포함
	- 라우터 장비가 라우팅 기능을 수행할 때 참조하는 부분

### Frame

|프레임 헤더|패킷 헤더|데이터그램 / 세그먼트 헤더|UDP / TCP 페이로드|프레임 푸터|
|---|---|---|---|---|

- 프레임 헤더 
	- 패킷 앞에 붙는 세번째 헤더
	- MAC 주소를 포함
	- 스위치 장비가 스위칭 기능을 수행할 때 참조하는 부분
