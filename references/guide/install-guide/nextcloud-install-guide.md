# Nextcloud install GUIDE

## 1. 기능 

- 클라우드 스토리지 프로그램
- 유사한 프로그램
    + 구글드라이브
    + 드롭박스

## 2. 필요 환경

- mariadb
- Apache

## 3. 설치

### 3.1 Apache 서버 설치

```shell
$ sudo apt update
$ sudo apt install apache2

$ sudo systemctl start apache2.service
$ sudo systemctl enable apache2.service
```
- 설치 확인 :: 80번 포트 수정 방안 필요
  ```shell
  ## curl or IP:80 브라우저 접속
  $ curl localhost:80
  ```
- 포트 번호 변경 :: 테스트 X
  ```shell
  ## Listen 80이라고 설정된 포트를 원하는 포트로 변경
  $ sudo vi /etc/apache2/ports.conf
  
  $ sudo systemctl restart apache2.service
  ```

### 3.2 mariadb 설치

> 자세한 mairadb 설치 가이드 링크
> - https://github.com/rexx4314/paas-ta-c3-team/blob/main/references/guide/install-guide/sue-mariadb-install-guide.md
> - 10.2/10.3/10.4/10.5 버전 설치 권장
> - 10.6 설치 시 `Error while trying to initialise the database: An exception occurred while executing a query: SQLSTATE[HY000]: General error: 4047 InnoDB refuses to write tables with ROW_FORMAT=COMPRESSED or KEY_BLOCK_SIZE.` 이슈 발생
>   + 이슈 관련 참고 사이트 
>     - https://github.com/nextcloud/docker/issues/1536 

```shell
$ sudo apt-get install mariadb-server mariadb-client
$ sudo systemctl start mariadb.service
$ sudo systemctl enable mariadb.service
```
- mysql 접속 방법
  ```shell
  $ sudo mysql -u root -p
  ```

### 3.3 PHP 관련 모듈 설치

- php 모듈 버전 호환성 확인

    + https://docs.nextcloud.com/server/20/admin_manual/installation/system_requirements.html#server

  ```shell
  $ sudo apt-get install software-properties-common
  $ sudo add-apt-repository ppa:ondrej/php :: enter 입력
  $ sudo apt update

  # PHP 7.4 이상 설치 필요
  $ sudo apt install php7.2 libapache2-mod-php7.2 php7.2-common php7.2-gmp php7.2-curl php7.2-intl php7.2-mbstring php7.2-xmlrpc php7.2-mysql php7.2-gd php7.2-xml php7.2-cli php7.2-zip
  ```
  ---
  > ## 참고
  > ### PHP 7.2에서 7.4 버전 업그레이드
  > 
  > 1. ondrej/phpPPA 추가
  > 
  > ```
  > $ sudo add-apt-repository ppa:ondrej/php sudo apt-get update
  > ```
  > 
  > 2. 사용중인 PHP 모듈 목록 확인
  > 
  > ```
  > $ dpkg -l | grep php | tee packages.txt
  > ```
  >   - 후에 버전 변경을 위해 앞에 7.2가 붙어 있는 확장 패키지 목록 메모
  > 
  > 3. 기본 패키지 설치
  > 
  > ```
  > $ sudo apt install php7.4 php7.4-common php7.4-cli
  > ```
  > 4. 확장 패키지 설치
  > 
  > - 메모해 둔 확장 패키지 목록 7.4 버전으로 설치
  > 
  > ```
  > $ sudo apt install php7.4-bcmath php7.4-bz2 php7.4-curl php7.4-intl php7.4-mbstring php7.4-mysql php7.4-readline php7.4-xml php7.4-zip php7.4-gd php7.4-memcached php7.4-redis
  > ```
  > 
  > 5. 버전 확인
  > 
  > ```
  > $ php -v
  > ```
  > ```
  > # PHP 7.2 비활성화
  > $ sudo a2dismod php7.2
  > # PHP 7.4 활성화
  > $ sudo a2enmod php7.4
  > ```
  > 
  > 6. 기본 패키지 변경
  > - 버전 선택
  > ```
  > $ sudo update-alternatives --config php
  > 
  > #PHP 7.2 제거
  > $ sudo apt purge php7.2*
  > 
  > # Apache 재시작
  > $ sudo systemctl restart apache2 
  > ```
---

### 3.4 NextCloud DB 세팅
- 설치한 mysql에 접속
  ```shell
  $ sudo mysql -u root -p
  ```
- nextcloud에 사용될 DB구성 설정
  ```shell
  CREATE DATABASE nextcloud;
  CREATE USER 'nextclouduser'@'localhost' IDENTIFIED BY 'PaaS-TA@2021';
  
  ## nextclouduser 계정에 권한 설정
  GRANT ALL ON nextcloud.* TO 'nextclouduser'@'localhost' IDENTIFIED BY 'PaaS-TA@2021' WITH GRANT OPTION;
  FLUSH PRIVILEGES;
  ```

### 3.5 최신 nextcloud 다운로드
- composer 다운로드
    + PHP 소프트웨어와 필요 라이브러리의 의존성을 관리하기 위한 표준 포맷을 제공하는 PHP 프로그래밍 언어의 패키지 관리자
  ```shell
  $ curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
  ```
- nextcloud 다운로드
    + https://github.com/nextcloud/server.git 브랜치 확인 후 clone
  ```shell
  $ cd /var/www/html
  $ sudo git clone --branch stable21 https://github.com/nextcloud/server.git nextcloud
  $ cd /var/www/html/nextcloud
  $ sudo composer install
  $ sudo git submodule update --init
  ```
- 권한 관련 오류를 방지하기 위해 폴더의 권한 수정
  ```shell
  $ sudo chown -R www-data:www-data /var/www/html/nextcloud/
  $ sudo chmod -R 755 /var/www/html/nextcloud/
  ```

### 3.6 Apache 설정
- /etc/apache2/sites-available
  + 서버에서 운영할 사이트의 설정파일을 저장하는 폴더
  ```shell
  $ sudo vi /etc/apache2/sites-available/nextcloud.conf
  <VirtualHost *:80> ## 3.1에서 변경한 port로 수정 필요
       ServerAdmin 이메일주소 ## 수정
       DocumentRoot /var/www/html/nextcloud/
       ServerName 서버 아이피 또는 URL ## 수정
    
       Alias /nextcloud "/var/www/html/nextcloud/"
  
       <Directory /var/www/html/nextcloud/>
          Options +FollowSymlinks
          AllowOverride All
          Require all granted
            <IfModule mod_dav.c>
              Dav off
            </IfModule>
          SetEnv HOME /var/www/html/nextcloud
          SetEnv HTTP_HOME /var/www/html/nextcloud
       </Directory>
  
       ErrorLog ${APACHE_LOG_DIR}/error.log
       CustomLog ${APACHE_LOG_DIR}/access.log combined
  
  </VirtualHost>
  ```
- 모듈 활성화
  + a2ensite *.conf 로 설정된 값을 Apache2가 인식할 수 있도록 sites-enable 디렉토리에 심링크 추가
  ```shell
  $ sudo a2ensite nextcloud.conf
  $ sudo a2enmod rewrite
  $ sudo a2enmod headers
  $ sudo a2enmod env
  $ sudo a2enmod dir
  $ sudo a2enmod mime
  
  $ sudo systemctl restart apache2.service
  ```

## 4. 접속 확인

- http://[Public IP]/nextcloud
- 정보 입력 및 정상 접속 확인  

![스크린샷 2022-02-10 오전 9 40 59](https://user-images.githubusercontent.com/86212069/153317529-603ad565-d391-471b-a39a-e25d10e8cfd7.png)