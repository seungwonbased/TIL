# Telnet

# Telnet 서버 설치 (Server)

```bash
$ dnf install telnet -server
```

# 설치 확인 및 유저 추가 (Server)

![Screenshot 2023-08-17 at 19.04.48.png](Telnet%20080a7615a6e14cc7b6859b9473e19ad2/Screenshot_2023-08-17_at_19.04.48.png)

# teluser로 로그인 (Server)

![Screenshot 2023-08-17 at 19.05.56.png](Telnet%20080a7615a6e14cc7b6859b9473e19ad2/Screenshot_2023-08-17_at_19.05.56.png)

# Ping test (WinClient)

![Screenshot 2023-08-17 at 19.06.48.png](Telnet%20080a7615a6e14cc7b6859b9473e19ad2/Screenshot_2023-08-17_at_19.06.48.png)

- WinClient에서 Ping 시도
- Telnet 192.168.111.100 연결 실패
    - 서버에서 설치할 때 서비스 대부분을 허용하지 않은 상태로 설치했기 때문에 외부에서 연결 불가능

# Firewall 설정 (Server)

```bash
$ firewall-config
```

![Screenshot 2023-08-17 at 19.13.53.png](Telnet%20080a7615a6e14cc7b6859b9473e19ad2/Screenshot_2023-08-17_at_19.13.53.png)

# 외부에서 Telnet 원격 접속 (WinClient)

```bash
$ telnet 192.168.111.100
```

![Screenshot 2023-08-17 at 19.15.05.png](Telnet%20080a7615a6e14cc7b6859b9473e19ad2/Screenshot_2023-08-17_at_19.15.05.png)

- 원격 접속 성공

---

[◀ 이전 페이지 돌아가기](../Linux,%20VMware%E1%84%85%E1%85%B3%E1%86%AF%20%E1%84%8B%E1%85%B5%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AB%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5%20%E1%84%80%E1%85%AE%E1%84%8E%E1%85%AE%E1%86%A8%202d50778ce91c4b59839555f0accc1aeb.md)