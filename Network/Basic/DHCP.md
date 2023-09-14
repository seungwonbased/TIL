# DHCP (Dynamic Host Configuration Protocol)

- 호스트의 IP 주소와 각종 TCP/IP 프로토콜의 기본 설정을 클라이언트에게 자동적으로 제공해주는 프로토콜
	- Subnet mask, Gateway IP 주소, DNS 서버 IP 주소 등
- 네트워크 안에 컴퓨터에 자동으로 네임 서버 주소, IP 주소, 게이트웨이 주소를 할당해주는 것을 의미하고, 해당 클라이언트에게 일정 기간 임대를 하는 동적 주소 할당 프로토콜
- 사용자 이동이 많고 한정된 IP 주소를 가진 경우 유용
- 서비스 포트
	- 서버: 67(bootps)
	- 클라이언트: 68(bootpc)

## DHCP Server

- 네트워크 인터페이스를 위해 IP 주소를 가지고 있는 서버에서 실행되는 프로그램
- 일정한 범위의 IP주소를 다른 클라이언트에게 할당하여 자동으로 설정하게 해주는 역할
- DHCP 서버는 클라이언트에게 할당된 IP 주소를 변경없이 유지해 줄 수 있음
- 클라이언트에게 IP 할당 요청이 들어오면 IP를 부여해주고 할당 가능한 IP들을 관리

## DHCP Client

- 클라이언트들은 시스템이 시작하면 DHCP 서버에 자신의 시스템을 위한 IP 주소를 요청
- DHCP 서버로부터 IP 주소를 부여받으면 TCP/IP 설정은 초기화되고, 다른 호스트와 TCP/IP를 사용해서 통신을 할 수 있게 됨
- 즉, 서버에게 IP를 할당받으면 TCP/IP 통신을 할 수 있음

## DHCP 동작

![dhcp](https://github.com/seungwonbased/TIL/blob/main/Network/assets/dhcp1.png)

1. DHCP Discover
	- 단말이 DHCP 서버를 찾기 위한 메시지
		- 브로드캐스트 메시지 (Destination MAC = FF:FF:FF:FF:FF:FF)
		- 단말이 LAN 상에 브로드캐스팅해 DHCP 서버를 찾음
	- 메시지 방향: 단말 -> DHCP 서버
2. DHCP Offer
	- DHCP가 응답하는 메시지
	- 단순히 DHCP 서버의 존재만을 알리지 않고 단말에 할당할 IP 정보를 포함한 세부 정보를 실어 단말에 전송
		- 브로드캐스트 메시지 (Destination MAC = FF:FF:FF:FF:FF:FF)이거나 유니캐스트일 수 있음
		- 단말이 보낸 DHCP Discover 메시지 내의 Broadcast Flag의 값에 따라 달라지는데, Flag=1이면 DHCP 서버는 DHCP Offer 메시지를 Broadcast로, Flag=0이면 Unicast로 보냄
	- 메시지 방향: DHCP 서버 -> 단말
3. DHCP Request
	- 단말이 **하나의 DHCP 서버를 선택**하고 해당 서버에게 **단말이 사용할** 네트워크 세부 정보를 요청
		- 브로드캐스트 메시지 (Destination MAC = FF:FF:FF:FF:FF:FF)
	- 메시지 방향: 단말 -> DHCP 서버
4. DHCP Ack
	- DHCP 절차의 마지막 메시지로, DHCP 서버가 단말에게 **네트워크 세부 정보**를 전달해 주는 메시지
		- 브로드캐스트 메시지 (Destination MAC = FF:FF:FF:FF:FF:FF) 혹은 유니캐스트일수 있음
	- 메시지 방향: DHCP 서버 -> 단말

### DHCP 갱신

![dhcp](https://github.com/seungwonbased/TIL/blob/main/Network/assets/dhcp2.png)