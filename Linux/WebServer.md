# Web 서버

<aside>
💡 LAMP (Linux, Apache, Mysql, Php) 서버 구축

</aside>

# AMP 설치 (Server)

```bash
$ dnf -y install httpd php php-mysqlnd mariadb-server
```

# AMP 테스트 (Server)

```bash
$ vi /var/www/html/index.html
```

```bash

# /var/www/html/index.html

[Test Web Page]
```

![Screenshot 2023-08-18 at 15.35.45.png](Web%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5%20cd30506489ec4cca9a1758d687287d66/Screenshot_2023-08-18_at_15.35.45.png)

```bash
$ vi /var/www/html/phpinfo.php
```

```bash
# /var/www/html/phpinfo.php

<? php phpinfo(); ?>
```

![Screenshot 2023-08-18 at 15.36.14.png](Web%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5%20cd30506489ec4cca9a1758d687287d66/Screenshot_2023-08-18_at_15.36.14.png)

# Web 서버용 데이터베이스 생성 (Server)

```bash
MariaDB > create database wpDB;
Query OK, 1 row affected (0.001 sec)

MariaDB > grant all on wpDB.* to wpUser@localhost identified by 'seungwon';
Query OK, 0 rows affected (0.005 sec)
```

# Wordpress 설치 (Server)

![캡처.PNG](Web%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5%20cd30506489ec4cca9a1758d687287d66/%25EC%25BA%25A1%25EC%25B2%2598.png)

# Wordpress 활용 웹 서버 구축 (Server)

```bash
$ vi /var/www/html/wordpress/wp-config.php
```

![2.PNG](Web%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5%20cd30506489ec4cca9a1758d687287d66/2.png)

- 23, 26, 29 line 수정
- Wordpress가 DB를 수정할 수 있는 권한 부여

# Web 서버 구동 확인 (WinClient)

![Screenshot 2023-08-18 at 15.40.19.png](Web%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5%20cd30506489ec4cca9a1758d687287d66/Screenshot_2023-08-18_at_15.40.19.png)

![Screenshot 2023-08-18 at 15.40.42.png](Web%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5%20cd30506489ec4cca9a1758d687287d66/Screenshot_2023-08-18_at_15.40.42.png)

- 정상 접속 성공

---

[◀ 이전 페이지 돌아가기](../Linux,%20VMware%E1%84%85%E1%85%B3%E1%86%AF%20%E1%84%8B%E1%85%B5%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%92%E1%85%A1%E1%86%AB%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%A5%20%E1%84%80%E1%85%AE%E1%84%8E%E1%85%AE%E1%86%A8%202d50778ce91c4b59839555f0accc1aeb.md)