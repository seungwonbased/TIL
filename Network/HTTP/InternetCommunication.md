# 인터넷 통신

# IP (인터넷 프로토콜)

- 역할
	- 지정한 IP address에 데이터 전달
	- Packet이라는 통신 단위로 데이터 전달
- 한계
	- 비연결성
		- 패킷을 받을 대상이 없거나 서비스 불능 상태여도 패킷 전송
	- 비신뢰성
		- 중간에 패킷이 사라진다면?
		- 패킷이 순서대로 안 온다면?
	- 프로그램 구분
		- 같은 IP를 사용하는 서버에서 통신하는 애플리케이션이 둘 이상이라면?

> Protocol: 통신 규약

# TCP, UDP

![ic](https://github.com/seungwonbased/TIL/blob/main/Network/assets/ic1.png)

- 프로토콜 계층
- 애플리케이션
	- 웹 브라우저, 게임, 채팅 프로그램, 소켓 라이브러리, …
- OS
	- TCP, UDP, IP, …
- 네트워크 인터페이스
	- LAN 드라이버, LAN 장비, …

## TCP/IP 패킷 정보

![ic2](https://github.com/seungwonbased/TIL/blob/main/Network/assets/ic2.png)

- TCP (Transmission Control Protocol, 전송 제어 프로토콜)
	- 연결 지향
		- TCP 3 way handshake (가상 연결)
	- 데이터 전달 보증
	- 순서 보장
	- 신뢰할 수 있는 프로토콜
	- 현재는 대부분 TCP 사용
- 3 way handshake

![ic3](https://github.com/seungwonbased/TIL/blob/main/Network/assets/ic3.png)

- UDP (User Datagram Protocol, 사용자 데이터그램 프로토콜)
	- 기능이 거의 없음, 하얀 도화지에 비유
	- 연결지향 X
	- 데이터 전달 보증 X
	- 순서 보장 X
	- 데이터 전달 및 순서가 보장되지 않지만 단순하고 빠름
	- IP와 거의 같으면서 Port, Checksum 정도만 추가
	- 애플리케이션에서 추가 작업 필요

# Port

- TCP나 UDP에서 어플리케이션의 상호 구분을 위해서 사용하는 논리적인 접속 장소 (번호)
- IP 내에서 프로세스 구분을 하기 위해서 사용
- 0~65535 사용 가능
	- 그러나 0~1023은 잘 알려진 포트로, 사용하지 않는 것이 좋음
		- FTP: 20, 21
		- TELNET: 23
		- HTTP: 80
		- HTTPS: 443

# DNS (Domain Name System)

- IP address는 기억하기 어렵고, 변경될 수도 있음
- DNS는 도메인 명을 IP 주소로 변환