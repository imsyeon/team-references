# 대표포털 개발용 VM 생성 가이드

## 1. mariadb

### 1.1. mariadb 설치
```s
$ sudo apt update
$ sudo apt install mariadb-server
$ mysql -V
mysql  Ver 15.1 Distrib 10.1.48-MariaDB, for debian-linux-gnu (x86_64) using readline 5.2
```

- mariadb 접속
```s
sudo mysql -u root
```

### 1.2. 계정 정보 생성
```s
## create user
CREATE USER '{ID}'@'%' IDENTIFIED BY '{PASSWORD}';


## grant all privileges
grant all privileges on *.* to {ID}@localhost identified by '{PASSWORD}';

FLUSH PRIVILEGES;

## create database
CREATE DATABASE /*!32312 IF NOT EXISTS*/ `{DB}` /*!40100 DEFAULT CHARACTER SET utf8 */;
```

### 1.3. DB 외부 접속 설정
- VM 내부가 아닌 외부에서 DB에 접근하기 위한 설정 필요

#### 1.3.1. bind-address 주석 처리

```s
$ sudo vi /etc/mysql/mariadb.conf.d/50-server.cnf
#bind-address            = 127.0.0.1
```

#### 1.3.2. mariaDB 재시작

```s
$ sudo service mysql restart
```

#### 1.3.3. 권한 설정

- host에 %로 설정되어야 외부에서 접속이 가능함

```s
# mysql 접속
MariaDB [(none)]> use mysql;

# 계정 정보 조회
MariaDB [mysql]> select host, user, password, plugin, authentication_string from user;

# root 계정 및 위에서 생성한 계정 모두 외부에서 접속 가능하게 설정
MariaDB [mysql]> grant all privileges on *.* to 'root'@'%' identified by '비밀번호';

# 권한 설정 저장
flush privileges;
```

## 2. Redis

- Redis 설치
```s
$ sudo apt-get update
$ sudo apt-get install redis-server
$ redis-server --version
Redis server v=4.0.9 sha=00000000:0 malloc=jemalloc-3.6.0 bits=64 build=9435c3c2879311f3
```

- Redis 설정 변경
```s
$ sudo vi /etc/redis/redis.conf

maxmemory 1g
maxmemory-policy allkeys-lru
```

- Redis 재시작 및 부팅 시 자동 시작 적용
```s
$ sudo systemctl restart redis-server.service
$ sudo systemctl enable redis-server.service
```

- 대표포털에서 사용하는 설정 적용
```s
$ redis-cli

127.0.0.1:6379> config get requirepass
127.0.0.1:6379> config set requirepass {PASSWORD}
127.0.0.1:6379> auth {PASSWORD}
127.0.0.1:6379> config get requirepass
1) "requirepass"
2) "{PASSWORD}"
```

- redis 외부 접속 설정
    + 외부에서 접근하기 위한 외부 접속 설정

```s
# bind 0.0.0.0 으로 설정
$ sudo vi /etc/redis/redis.conf
# bind 127.0.0.1
bind 0.0.0.0

# redis 재시작
$ sudo systemctl restart redis

# 설정 확인
$ sudo netstat -ntlp | grep redis
tcp        0      0 0.0.0.0:6379            0.0.0.0:*               LISTEN      43276/redis-server  
```

## 3. vsftpd

- 파일 저장용 ftp 파일 서버
- vsftpd 설치
```s
$ sudo apt-get update
$ sudo apt-get install vsftpd
```

- 서비스 재시작 및 부팅 시 자동 시작
```s
$ sudo systemctl restart vsftpd.service
$ sudo systemctl enable vsftpd.service
```

- ftp용 유저 생성 및 디렉터리 생성
```s
$ sudo adduser vsftp
Enter new UNIX password:

$ sudo mkdir /home/vsftp/ftp
$ sudo chown vsftp:vsftp /home/vsftp/ftp
$ sudo chmod 755 /home/vsftp/ftp
```

- vsftpd 설정 파일 수정
    + 기존 설정 파일 백업 후 삭제
```s
## 기존의 설정 파일 백업
$ sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.back

## 기존 설정 파일 삭제 후 아래 설정만 작성 후 저장
$ sudo vim /etc/vsftpd.conf
listen=NO # ip4 보안그룹 설정에 따라 설정 변경 필요 / ip4만 네트워크 개방 시 YES로 설정
listen_ipv6=YES #ip6 / ip4만 네트워크 개방 시 NO로 설정
local_umask=022
dirmessage_enable=YES
use_localtime=YES
xferlog_enable=YES
# connect_from_port_20=YES

anonymous_enable=NO
local_enable=YES
write_enable=YES
chroot_local_user=YES
allow_writeable_chroot=YES
anon_mkdir_write_enable=YES

user_sub_token=$USER
local_root=/home/$USER/ftp
secure_chroot_dir=/var/run/vsftpd/empty

pam_service_name=vsftpd

pasv_enable=YES
pasv_min_port=50030
pasv_max_port=50040

listen_port=50021
ftp_data_port=50020

# userlist
userlist_enable=YES
userlist_file=/etc/vsftpd.userlist
userlist_deny=NO
```

- 접근 유저 설정
```s
$ sudo vim /etc/vsftpd.userlist
vsftp
```

- 서비스 재시작
```
$ sudo systemctl restart vsftpd
$ sudo systemctl status vsftpd
```

## 4. Apache
- 웹에서 이미지를 보기 위한 용도

### 4.1. apache 설치

```s
$ sudo apt update
$ sudo apt install apache2
$ sudo service apache2 start
$ ps aux | grep apache2
```
- 설치 완료 후 vm ip 주소로 브라우저 접속
- 아래와 같은 화면 확인 시 정상 설치 완료

![](https://velog.velcdn.com/images/imsooyeon/post/8a03e649-eea9-4689-9347-850ecce03586/image.png)


### 4.2. https 설정
- 대표포털 code에 https로 설정되어 있어 기본 설정인 http는 브라우저에서 이미지 확인이 불가능
- openssl 설치 여부 확인
```s
$ openssl version
OpenSSL 1.1.1  11 Sep 2018
```
- 미설치되어 있을 시 설치 진행
```s
$ sudo apt install openssl
```
- apache 모듈 사용 :: Mod_ssl
- Mod_rewrite 아파치 모듈 활성화
```s
$ a2enmod ssl
$ a2enmod rewrite
```

- apache2.conf 파일 마지막 줄에 설정 추가
```s
$ sudo vi /etc/apache2/apache2.conf

# 아래 설정 추가
<Directory /var/www/html>
AllowOverride All
</Directory>
```

- 키와 인증서 발급
```s
$ mkdir /etc/apache2/certificate
$ cd /etc/apache2/certificate
$ openssl req -new -newkey rsa:4096 -x509 -sha256 -days 365 -nodes -out apache-certificate.crt -keyout apache.key

Generating a RSA private key
............++++
.......................................................++++
writing new private key to 'apache.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]: KR 
State or Province Name (full name) [Some-State]:SEOUL
Locality Name (eg, city) []:SEOUL
Organization Name (eg, company) [Internet Widgits Pty Ltd]: #enter
Organizational Unit Name (eg, section) []: #enter
Common Name (e.g. server FQDN or YOUR name) []: #VM IP 입력
Email Address []: #enter
```

- 000-default.conf 설정 변경
```s
$ sudo vi /etc/apache2/sites-enabled/000-default.conf
```

- 아래와 같이 설정 파일 변경
- http로 들어왔을 때 https로 redirect 시켜주는 설정 포함
```s
<VirtualHost *:80>
        RewriteEngine On
        RewriteCond %{HTTPS} !=on
        RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R=301,L]
</virtualhost>
<VirtualHost *:443>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
        SSLEngine on
        SSLCertificateFile /etc/apache2/certificate/apache-certificate.crt
        SSLCertificateKeyFile /etc/apache2/certificate/apache.key
</VirtualHost>
```

- 링크 설정
```s
$ cd "/var/www/html"
$ ln -s /home/vsftp/ftp/file file
```

- apache 다시 시작
```s
$ sudo service apache2 restart
```

- VM IP로 브라우저 접속 시 아래와 같이 뜨면 정상 적용 확인

![](https://velog.velcdn.com/images/imsooyeon/post/9b013495-3179-4823-a4ba-6f49d8aba34f/image.png)

## 5. 대표포털 프로젝트 적용

- 각 프로젝트에 application.property 수정

- redis

```s
# redis
spring.redis.host={VM IP}
spring.redis.password={REDIS PW}
spring.redis.port={redis PORT}
spring.redis.timeout
```

- vsftpd

```s
# file server
fs.type=ftp
fs.ftp.url={VM IP}
fs.ftp.port={vsftpd PORT}
fs.ftp.id={vsftpd ID}
fs.ftp.pw={vsftpd PW}
```
- mariadb
- 프로젝트 마다 사용하는 username, password, database 정보 확인

```s
# datasource
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.username={mysql USERNAME}
spring.datasource.password={myql PW}
spring.datasource.url={mysql URL}
```