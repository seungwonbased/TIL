# XRDP

# XRDP 설치 (Server)

```bash
$ dnf -y install epel-release
```

```bash
$ dnf -y install xrdp
```

- EPEL (Extra Packages for Enterprise Linux): CentOS에서 제공하는 패키지 외 추가 패키지를 제공하는 저장소
- CentOS에서 기본적으로 XRDP를 제공하지 않으므로 EPEL을 통해 설치

# XRDP 시작 및 상시 가동 설정 (Server)

![Screenshot 2023-08-17 at 20.34.20.png](XRDP%2085fa5255e1714491b2c1a718ffa16b74/Screenshot_2023-08-17_at_20.34.20.png)

# Firewall 설정 (Server)

```bash
$ firewall-config
```

![Screenshot 2023-08-17 at 20.35.18.png](XRDP%2085fa5255e1714491b2c1a718ffa16b74/Screenshot_2023-08-17_at_20.35.18.png)

# 외부에서 XRDP 원격 접속 (WinClient)

![Screenshot 2023-08-17 at 20.35.59.png](XRDP%2085fa5255e1714491b2c1a718ffa16b74/Screenshot_2023-08-17_at_20.35.59.png)

![Screenshot 2023-08-17 at 20.36.17.png](XRDP%2085fa5255e1714491b2c1a718ffa16b74/Screenshot_2023-08-17_at_20.36.17.png)

- 원격 접속 성공

---

[◀ 이전 페이지 돌아가기](../Linux,%20VMware%E1%84%85%E1%85%B3%E1%86%AF%20%E1%84%8B%E1%85%B5%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AB%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5%20%E1%84%80%E1%85%AE%E1%84%8E%E1%85%AE%E1%86%A8%202d50778ce91c4b59839555f0accc1aeb.md)