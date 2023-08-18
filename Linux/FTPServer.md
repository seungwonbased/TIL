# FTP 서버

## FTP 설치 (Server)

```bash
$ dnf -y install vsftpd
```

## vsftpd.conf 파일 수정 (Server)

```bash
$ vi /etc/vsftpd/vsftpd.conf
```

```bash
# /etc/vsftpd/vsftpd.conf

...
anonymous_enable=YES
...
write_enable=YES
...
anon_upload_enable=YES
...
anon_mkdir_write_enable=YES
...
```

- anonymous_enable: 익명 FTP 서비스 활성화 여부 설정
- write_enable: 쓰기 가능 여부 설정
- anon_upload_enable: 익명 FTP 접속자의 파일 업로드 권한 설정
- anon_mkdir_write_enable: 익명 FTP 접속자의 디렉토리 생성 권한 설정

## Root에서 FTP로 파일 소유권 이전 (Server)

```bash
$ cd /var/ftp
$ chown ftp.ftp /var/ftp/pub
```

## 외부에서 FTP 연결 (WinClient)

![Screenshot 2023-08-17 at 23.51.43.png](https://github.com/seungwonbased/TIL/blob/main/Linux/assets/Screenshot_2023-08-17_at_23.51.43.png)

## 사이트 등록 및 연결 (WinClient)

![Screenshot 2023-08-17 at 23.53.06.png](https://github.com/seungwonbased/TIL/blob/main/Linux/assets/Screenshot_2023-08-17_at_23.53.06.png)

## 외부에서 파일 전송

![Screenshot 2023-08-17 at 23.53.37.png](https://github.com/seungwonbased/TIL/blob/main/Linux/assets/Screenshot_2023-08-17_at_23.53.37.png)

- 파일 전송 성공
