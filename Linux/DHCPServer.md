# DHCP 서버

# DHCP 설치 확인 (Server)

```bash
$ rpm -qa dhcp-client
```

# VMware DHCP 끄기 (VMware)

![Screenshot 2023-08-18 at 15.21.19.png](DHCP%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5%20b4f6e1f39520484db19617e830fb08a9/Screenshot_2023-08-18_at_15.21.19.png)

- VMware의 Virtual Network Manager에서 DHCP 서비스 사용 해제

### 결과

- `**Server(B)**`

![Screenshot 2023-08-18 at 15.23.00.png](DHCP%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5%20b4f6e1f39520484db19617e830fb08a9/Screenshot_2023-08-18_at_15.23.00.png)

- **`LinClient`**

![Screenshot 2023-08-18 at 15.23.43.png](DHCP%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5%20b4f6e1f39520484db19617e830fb08a9/Screenshot_2023-08-18_at_15.23.43.png)

- IP 할당 중지, 네트워크 연결 끊어짐

# DHCP server 설치 (Server)

```bash
$ dnf -y install dhcp-server
```

# dhcpd.conf 파일 설치 (Server)

```bash
$ vi /etc/dhcp/dhcpd.conf
```

```bash
# /etc/dhcp/dhcp.conf
#
# DHCP Server Configuration file.
#   see /usr/share/doc/dhcp-server/dhcpd.conf.example
#   see dhcpd.conf(5) man page
#

ddns-update-style interim;
subnet 192.168.111.0 netmask 255.255.255.0 {
        option    routers 192.168.111.2;
        option    subnet-mask 255.255.255.0;
        range     dynamic-bootp 192.168.111.55  192.168.111.99;
        option    domain-name-servers 8.8.8.8;
        default-lease-time  10000;
        max-lease-time      50000;
}
```

- subnet `NETWORKID` netmask `NETMASK` { }
- `option routers`: 게이트웨이 지정
- `option subnet-mask`: 서브넷 마스크 지정
- `range dynamic-bootp`: IP 할당 범위
- `option domain-name-server`: 도메인 네임 서버 지정
- `default-lease-time`: 임대 요청 만료 시간 (sec)
- `max-lease-time`: 클라이언트가 IP를 가지고 있는 최대 시간 (sec)

# IP 할당 확인 (Server(B) & LinClient)

- **`Server(B)`**

![Screenshot 2023-08-18 at 15.25.34.png](DHCP%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5%20b4f6e1f39520484db19617e830fb08a9/Screenshot_2023-08-18_at_15.25.34.png)

- **`LinClient`**

![Screenshot 2023-08-18 at 15.26.09.png](DHCP%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5%20b4f6e1f39520484db19617e830fb08a9/Screenshot_2023-08-18_at_15.26.09.png)

- IP 할당 성공

# 할당 데이터베이스 확인 (Server)

```bash
$ cat /var/lib/dhcpd/dhtpd.leases
```

![Screenshot 2023-08-18 at 15.27.22.png](DHCP%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5%20b4f6e1f39520484db19617e830fb08a9/Screenshot_2023-08-18_at_15.27.22.png)

- Server(B), LinClient에 IP를 할당한 내역

---

[◀ 이전 페이지 돌아가기](../Linux,%20VMware%E1%84%85%E1%85%B3%E1%86%AF%20%E1%84%8B%E1%85%B5%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AB%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5%20%E1%84%80%E1%85%AE%E1%84%8E%E1%85%AE%E1%86%A8%202d50778ce91c4b59839555f0accc1aeb.md)