# 메일 서버

## Sendmail 설치 (Server)

```bash
$ vi dnf -y install sendmail
```

- SMTP 프로토콜을 이용해 다른 메일 서버로 메일을 전달하는 프로그램

## hostname 파일 수정 (Server)

```bash
$ vi /etc/hostname
```

```bash
# /etc/hostname

mail.naver.com
```

## hosts 파일 수정 (Server)

```bash
$ vi /etc/hosts
```

```bash
# /etc/hosts

...
192.168.111.100 mail.naver.com
```

## local-host-names 파일 수정 (Server)

```bash
$ vi /etc/mail/local-host-names
```

```bash
# /etc/mail/local-host-names

mail.naver.com
```

## network 파일 수정 (Server)

```bash
$ vi /etc/sysconfig/network
```

```bash
# /etc/sysconfig/network
# Created by anaconda

HOSTNAME=mail.naver.com
```

## Sendmail 설치 및 파일 수정 (Server(B))

- 위 과정을 Server(B)에서는 mail.daum.net으로 수정

## BIND 설치 (Server)

```bash
$ dnf -y install bind-*
```

- Server에 Name server 설치

## named.conf 파일 수정 (Server)

```bash
$ vi /etc/named.conf
```

```bash
# /etc/named.conf

options {
				listen-on-port 53 { any; };
				listen-on-v6 port 53 { none; };
				...
				allow-query { any; };
				...
				dnssec-validation no;
				...
}
...
zone "naver.com" IN {
				type master;
				file "naver.com.db";
				allow-update { none; };
};

zone "daum.net" IN {
				type master;
				file "daum.net.db";
				allow-update { none; };
};
```

- 포트 외부 접근을 허용하고, 쿼리를 허용할 대역을 설정
- 도메인을 관리하는 데이터베이스 파일인 zone 파일을 지정

## .com.db 파일 생성 (Server & Server(B))

```bash
$ cd /var/named
$ vi naver.com.db
$ vi daum.net.db
```

```bash
$TTL     3H
@        SOA      @       root.      (  2  1D  1H  1W  1H  )
         IN       NS      @
         IN       A       192.168.111.100
         IN       MX      10      mail.naver.com.

mail     IN       A       192.168.111.100
```

```bash
$TTL     3H
@        SOA      @       root.      (  2  1D  1H  1W  1H  )
         IN       NS      @
         IN       A       192.168.111.200
         IN       MX      10      mail.daum.net.

mail     IN       A       192.168.111.200
```

- $TTL: 도메인에 대한 정보를 다른 네임 서버에서 가져간 다음 가져간 네임 서버에 얼마나 보관할 것인지를 명시
- SOA (Start of Authority): 여기서 설정한 도메인에 대해 네임 서버가 모든 정보를 가지고 있음을 선언
- A: 해당 도메인의 실제 IP 주소를 설정하는 레코드
- MX: 메일 서버로 사용하게 될 도메인을 지정

## 메일 서버 구동 확인 (Server & Server(B))

![Screenshot 2023-08-17 at 23.31.56.png](https://github.com/seungwonbased/TIL/blob/main/Linux/assets/Screenshot_2023-08-17_at_23.31.56.png)

![Screenshot 2023-08-17 at 23.41.36.png](https://github.com/seungwonbased/TIL/blob/main/Linux/assets/Screenshot_2023-08-17_at_23.41.36.png)
