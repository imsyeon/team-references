# MariaDB install guide

##  1. System upgrade

- 최신 패키지 확인 및 업그레이드

```shell
$ sudo apt update
$ sudo apt upgrade -y
$ sudo reboot
```

## 2. Package 설치

```shell
$ sudo apt install software-properties-common -y
```

## 3. MariaDB GPG 키 및 MariaDB APT 리포지토리 추가

```shell
$ curl -LsS -O https://downloads.mariadb.com/MariaDB/mariadb_repo_setup
$ sudo bash mariadb_repo_setup --mariadb-server-version=10.6
[info] Checking for script prerequisites.
[info] Repository file successfully written to /etc/apt/sources.list.d/mariadb.list
[info] Adding trusted package signing keys...
[info] Running apt-get update...
[info] Done adding trusted package signing keys
```
## 4. Ubuntu 18.04에 MariaDB 10. 6 설치

```shell
$ sudo apt update

# 설치 가능한 버전 확인
$ sudo apt-cache search mariadb-server

# 설치
$ sudo apt install mariadb-server mariadb-client
```

## 5. 보안 설정

```shell
$ sudo mariadb-secure-installation

# 엔터 입력
Enter current password for root (enter for none):

# unix_socket 인증 플러그인
Switch to unix_socket authentication [Y/n] Y

# 비밀번호 설정
Change the root password? [Y/n] Y
New password: 
Re-enter new password: 

# 불필요한 사용자 제거
Remove anonymous users? [Y/n] Y

# 외부에서 root 계정으로 로그인 사용
Disallow root login remotely? [Y/n] Y

# 테스트 데이터베이스 삭제
Remove test database and access to it? [Y/n] Y

# 바로 적용할 건지 여부
Reload privilege tables now? [Y/n] Y
 
```

## 6. MariaDB 상태 확인

```shell
$ systemctl status mariadb
mariadb.service - MariaDB 10.6.4 database server
   Loaded: loaded (/lib/systemd/system/mariadb.service; enabled; vendor preset: enabled)
  Drop-In: /etc/systemd/system/mariadb.service.d
           └─migrated-from-my.cnf-settings.conf
   Active: active (running) since Tue 2021-10-19 11:37:25 UTC; 9s ago
     Docs: man:mariadbd(8)
     ...
```

## 7. 서버 재부팅 시 MariaDB 자동으로 시작

```shell
$ sudo systemctl enable mariadb
```

## 8. 버전 확인

```shell
$ mariadb -V
```

## 9. 접속

```shell
$ mysql -u root -p
```
## 10. 계정 생성

```shell
MariaDB [mysql]> create user sue@'%' identified by '비밀번호';
```

## 11. 권한 설정

```shell
# ccc_homepage_dev에만 접속이 가능하도록 권한 부여
MariaDB [mysql]> grant all privileges on ccc_homepage_dev.* to 'sue'@'%' identified by 'PaaS-TA@2021';
```

<br /><br />

---

# 외부 접속 허용 방법

## 1. bind-address 주석 처리

```shell
$ sudo vi /etc/mysql/mariadb.conf.d/50-server.cnf
#bind-address            = 127.0.0.1
```

## 2. mariaDB 재시작

```shell
$ sudo service mysql restart
```

## 3. 권한 설정

- host에 %로 설정되어야 외부에서 접속이 가능함

```shell
$ mysql -u root -p

# mysql 접속
MariaDB [(none)]> use mysql;

# 계정 정보 조회
MariaDB [mysql]> select host, user, password, plugin, authentication_string from user;

# root 계정을 외부에서 접속 가능하게 설정
MariaDB [mysql]> grant all privileges on *.* to 'root'@'%' identified by '비밀번호';

MariaDB [mysql]> select host, user from user;
+-----------+-------------+
| Host      | User        |
+-----------+-------------+
| %         | joy         |
| %         | kevin       |
| %         | rex         |
| %         | root        |
| %         | sue         |
| localhost | mariadb.sys |
| localhost | mysql       |
| localhost | root        |
+-----------+-------------+

# 권한 설정 저장
flush privileges;
```

<br /><br />

---

# MariaDB 삭제

```shell
# 패키지와 패키지의 환경설정 모두 삭제
$ sudo apt purge mariadb-*

# 의존성 때문에 설치되었지만 사용되지 않는 패키지 삭제
$ sudo apt autoremove

$ dpkg -l | grep mysql
$ sudo apt purge mysql-common
$ sudo reboot
```