# ARP (Address Resolution Protocol, 주소 결정 프로토콜)

- 네트워크 상에서 IP 주소를 물리적 네트워크 주소(MAC)로 대응시키기 위해 사용하는 프로토콜
	- 상대방의 MAC 주소를 알아내기 위해 사용되는 프로토콜
- 데이터 통신을 하기 위해 2계층 물리적 주소인 MAC 주소와 3계층 논리적 IP 주소 두 개를 사용
	- IP 주소와 MAC 주소를 연결해 주는 것이 필요
		- ARP 담당

## ARP Header

![arp](https://github.com/seungwonbased/TIL/blob/main/Network/assets/arp1.png)

3) MAC 주소 길이 ⇒ 6 bytes
4) IP 주소 길이 ⇒ 4 bytes
5) ARP의 구체적인 동작
	- ARP Request
	- ARP Reply
	- RARP Request
	- RARP Reply
6) 7\) 송신자의 MAC 주소와 IP 주소
8) 9\) 수신자의 MAC 주소와 IP 주소

## ARP 동작

![arp](https://github.com/seungwonbased/TIL/blob/main/Network/assets/arp2.png)

- 목적지의 MAC 주소를 모르기 때문에 정상적으로 패킷을 만들 수 없음

![arp](https://github.com/seungwonbased/TIL/blob/main/Network/assets/arp3.png)

- ARP Request를 네트워크에 브로드캐스팅

![arp](https://github.com/seungwonbased/TIL/blob/main/Network/assets/arp4.png)

- ARP Request에 있는 대상자 IP 주소가 자신의 IP와 동일한 경우 ARP Request를 한 출발지로 ARP Reply

![arp](https://github.com/seungwonbased/TIL/blob/main/Network/assets/arp5.png)

- ARP Cache 테이블을 갱신 후 패킷을 생성해 전송

## ARP Cache Table

- 패킷 네트워크에서는 큰 데이터를 잘라 전송하므로 여러 개의 패킷을 전송해야 함
- 패킷을 보낼 때마다 ARP 브로드캐스트를 수행하면 네트워크 통신의 효율성이 크게 저하되므로 메모리에 정보를 저장해 두고 재사용

## ARP 영역

- ARP 요청과 응답이 일어나는 영역
- LAN 영역: 동일한 네트워크 ID를 공유하는 호스트가 MAC 주소를 기반으로 스위칭 방식으로 내부 통신을 수행하면서, 단일 ARP 영역을 생성하는 공간

## 네트워크에서 출발지부터 목적지로 데이터를 전송할 때 사용하는 통신 방식

|타입|통신 대상|범위|IPv4|IPv6|예제|
|:---:|:---:|:---:|:---:|:---:|:---:|
|Unicast|1 : 1|전체 네트워크|O|O|HTTP|
|Broadcast|1 : All|서브넷 (로컬 네트워크)|O|X|ARP|
|Multicast|1 : Group|정의된 구간|O|O|IPTV|
|Anycast|1 : 1|전체 네트워크|△|O|가까운 DNS 서버 찾기|
