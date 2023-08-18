# 마스터 네임 서버

## HTTP 설치 (Server)

```bash
$ vi dnf -y install httpd
```

## 테스트 HTML 파일 생성 (Server)

```bash
$ vi /var/www/html/index.html
```

```html
<h1>web page by seungwon</h1>
```

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
...bash
zone "seungwon.com" IN {
	type master;
	file "seungwon.com.db";
	allow-update { none; };
};
```

- 포트 외부 접근을 허용하고, 쿼리를 허용할 대역을 설정
- 도메인을 관리하는 데이터베이스 파일인 zone 파일을 지정

## .com.db 파일 생성 (Server)

```bash
$ vi /var/named/seungwon.com.db
```

```bash
$TTL    3H
@       SOA     @       root.   (2      1D      1H      1W      1H)
        IN      NS      @
        IN      A       192.168.111.100

www     IN      A       192.168.111.100
ftp     IN      A       192.168.111.200
```

- $TTL: 도메인에 대한 정보를 다른 네임 서버에서 가져간 다음 가져간 네임 서버에 얼마나 보관할 것인지를 명시
- SOA (Start of Authority): 여기서 설정한 도메인에 대해 네임 서버가 모든 정보를 가지고 있음을 선언
- A: 해당 도메인의 실제 IP 주소를 설정하는 레코드

## FTP 설치 (Server(B))

```bash
$ dnf -y install vsftpd
```

## FTP 설치 (LinClient)

```bash
$ su -c 'dnf -y install ftp'
```

## nslookup 테스트 (LinClient)

```bash
$ nslookup
```

![4.PNG](https://github.com/seungwonbased/TIL/blob/main/Linux/assets/4.png)

- 네임 서버 정상 작동

## 도메인 이용하여 외부 FTP 접속 (LinClient)

```bash
$ ftp ftp.seungwon.com
```

![5.PNG](https://github.com/seungwonbased/TIL/blob/main/Linux/assets/5.png)

## 도메인 이용하여 외부 웹 접속 (LinClient)

![6.PNG](https://github.com/seungwonbased/TIL/blob/main/Linux/assets/6.png)
