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

![Screenshot 2023-08-17 at 20.34.20.png](https://github.com/seungwonbased/TIL/blob/main/Linux/assets/Screenshot_2023-08-17_at_20.34.20.png)

# Firewall 설정 (Server)

```bash
$ firewall-config
```

![Screenshot 2023-08-17 at 20.35.18.png](https://github.com/seungwonbased/TIL/blob/main/Linux/assets/Screenshot_2023-08-17_at_20.35.18.png)

# 외부에서 XRDP 원격 접속 (WinClient)

![Screenshot 2023-08-17 at 20.35.59.png](https://github.com/seungwonbased/TIL/blob/main/Linux/assets/Screenshot_2023-08-17_at_20.35.59.png)

![Screenshot 2023-08-17 at 20.36.17.png](https://github.com/seungwonbased/TIL/blob/main/Linux/assets/Screenshot_2023-08-17_at_20.36.17.png)

- 원격 접속 성공
