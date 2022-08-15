## PostgreSQL install GUIDE
1. 사전 작업
    ```shell
    # OS 확인
    $ cat /etc/os-release

    # PostgreSQL 현재 패키지 버전 확인
    $ sudo apt install wget ca-certificates
    ```

<br/>

2. PostgreSQL 저장소 추가
    ```shell
    $ sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
    $ sudo apt install wget ca-certificates
    $ wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
    ```

<br/>

3. PostgreSQL 설치
    ```shell
    $ sudo apt-get update  # 오류날 시 다음 줄 그냥 진행
    $ sudo apt show postgresql  # 버전 정보 확인
    ```
   - 버전 지정하여 설치
       ```shell
       $ sudo apt-get install postgresql-13  # PostgreSQL 13 버전
       ```
   - 최신 버전 설치
       ```shell
       $ sudo apt-get install postgresql postgresql-contrib
       ```

<br/>

4. 설치 확인
    ```shell
    $ service postgresql status
   
    ● postgresql.service - PostgreSQL RDBMS
      Loaded: loaded (/lib/systemd/system/postgresql.service; enabled; vendor preset: enabled)
      Active: active (exited) since Wed 2021-09-29 04:12:11 UTC; 1min 1s ago
    Main PID: 24254 (code=exited, status=0/SUCCESS)
       Tasks: 0 (limit: 4915)
      CGroup: /system.slice/postgresql.service
    ```

<br/>

5. 접속 확인
    ```shell
    $ sudo su - postgres
    $ psql
   
    psql (13.4 (Ubuntu 13.4-1.pgdg18.04+1))
    Type "help" for help.
    
    postgres=# \conninfo
    You are connected to database "postgres" as user "postgres" via socket in "/var/run/postgresql" at port "5432".
    postgres=# \q
    ```

<br/>

6. 사용자 생성
    ```shell
    # 사용자 생성
    postgres=# create user 사용자명;

    # 사용자 Role 설정
    postgres=# alter role 사용자명 with createdb;

    # 사용자 비밀번호 설정
    postgres=# alter user 사용자명 with encrypted password '비밀번호';
    ```
   - 생성한 사용자 정보 조회
       ```shell
       postgres=# \du
       ```

<br/>

7. Database 생성
    ```
    # 데이터베이스 생성
    postgres=# create database 데이터베이스명 owner 사용자명;

    # Privileges 설정
    postgres=# grant all privileges on database 데이터베이스명 to 사용자명;
    ```
   - 생성한 Database 조회
       ```shell
       postgres=# \l
       ```