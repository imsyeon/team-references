# SonarQube install GUIDE

## 1. 기능
|항목|설명|
|:---:|:---|
|**Rules**|설치된 규칙들과 상세 정보 확인 가능|
|**Quality Profile**|규칙들을 특정 상황에 맞도록 그룹으로 묶은 것|
|**Quality Gate**|• 통과 여부(Pass or Fail)에 따라 프로젝트의 상태 쉽게 확인 가능<br>• 결과 → 현재 버전 프로젝트의 배포 가능 여부의 기준|
|**Issues**|현재 프로젝트 내의 모든 이슈를 한 눈에 파악 가능|
|**Measures**|프로젝트의 분석 결과를 다양한 관점으로 파악 가능|
|**Code**|프로젝트의 디렉터리 구조와 실제 코드 파악|
|**Activity**|프로젝트의 분석 수행 히스토리 파악|
|**Administration**|프로젝트 분석을 위한 설정 기능|

<br/>

## 2. 종류
> - zip 파일로 설치
> - Docker 이미지로 설치

이 중 zip 파일을 이용하여 시스템에 직접 설치하는 방법으로 진행

<br/>

## 3. 필요 환경
- OpenJDK 11
- PostgreSQL 13
- SonarQube 8.9.2
- Ubuntu 18.04

<br/>

## 4. 설치
### 4.1 OpenJDK 11
- [OpenJDK install guide](https://github.com/rexx4314/paas-ta-ccc-team/blob/main/references/oss/install-guide/kevin-open-jdk-install-guide.md)

<br/>

### 4.2 PostgreSQL
- 아래 링크 참고하여 설치 후 다음 단계 진행
    - [PostgreSQL install guide](https://github.com/rexx4314/paas-ta-ccc-team/blob/main/references/oss/install-guide/joy-postgresql-install-guide.md)
        > 주의) 9/30 부로 14가 최신버전. 현재 SonarQube에서는 13버전까지만 지원하므로 버전 지정하여 설치

1. 외부 접속 설정
    ```
    $ sudo vi /etc/postgresql/13/main/postgresql.conf
   
    ---------------------------------------------------------------------------------
   
    # 주석 해제 후 127.0.0.1을 '*'로 수정
    listen_addresses = '*'
   
    ---------------------------------------------------------------------------------
    ```

2. 모든 사용자 접속 가능하게 설정 - 비밀번호 사용
    ```
    $ sudo vi /etc/postgresql/13/main/pg_hba.conf
    ```
    
    - 변경 전
        ```
        ---------------------------------------------------------------------------------
        
        local   all             postgres                                peer
        ...
        
        # IPv4 local connections:
        host    all             all             127.0.0.1/32        scram-sha-256
        
        ---------------------------------------------------------------------------------
        ```
        
    - 변경 후
        ```
        ---------------------------------------------------------------------------------
        
        local   all             postgres                                peer
        ...
        
        # IPv4 local connections:
        host    all             all             0.0.0.0/0               password
        
        ---------------------------------------------------------------------------------
        ```
        
    > 주의) peer를 md5로 지정하지 말 것. 데이터베이스 connection 오류 발생함

3. service 설정 반영 및 postgresql 재시작
    ```shell
    $ sudo systemctl daemon-reload
    $ sudo systemctl restart postgresql.service
    ```

<br/>

### 4.3 SonarQube
1. 홈페이지에서 다운로드 링크 복사
   > https://www.sonarqube.org/downloads/

   ![소나큐브_다운로드 링크](https://user-images.githubusercontent.com/74666378/135384951-8d2439a1-3c50-466e-9627-d350f64114d2.PNG)

2. 설치
    ```shell
   # 다운로드, wget [복사한 링크]
   $ wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-8.9.2.46101.zip
   
   # zip파일 압축 해제를 위한 unzip 설치
   $ sudo apt-get install unzip
   
   # zip파일 압축 해제
   $ unzip sonarqube-8.9.2.46101.zip
   ```

3. 설정
   - SonarQube 관련 설정
       ```shell
       # SonarQube 설정 파일 수정
       $ vim ~/workspace/user/{작업디렉터리명}/sonarqube-8.9.2.46101/conf/sonar.properties
     
       ---------------------------------------------------------------------------------
     
       # 주석 해제 후 DB셋팅 시 만든 계정 정보 입력
       sonar.jdbc.username=아이디
       sonar.jdbc.password=비밀번호
  
       # 주석 해제 후 url 지정
       sonar.jdbc.url=jdbc:postgresql://{VM IP}/DB명
  
       # 주석 해제 후 port 지정
       sonar.web.port=8083
     
       ---------------------------------------------------------------------------------
       ```
     > 주의) `sonar.jdbc.url` 설정 시 IP 뒤에 port 지정하면 웹페이지 접속 안 됨
     <br/> ex) `sonar.jdbc.url=jdbc:postgresql://10.10.0.0/sonar` (o)
     <br/> ex) `sonar.jdbc.url=jdbc:postgresql://10.10.0.0:5432/sonar` (x)

   - OpenJDK 경로 설정
       ```shell
       # 설치 경로 조회
       $ which java
     
       # 설정 파일 수정
       $ vim ~/workspace/user/{작업디렉터리명}/sonarqube-8.9.2.46101/conf/wrapper.conf

       ---------------------------------------------------------------------------------
    
       wrapper.java.command=/usr/bin/java # 조회한 설치경로로 수정
     
       ---------------------------------------------------------------------------------
       ```

   - Max map count 설정
     > 주의) profile에 설정하지 않으면 해당 세션에서만 적용됨
       ```shell
       # profile 수정
       $ sudo vim /etc/profile
     
       ---------------------------------------------------------------------------------
     
       # 맨 밑에 추가
       sudo sysctl -w vm.max_map_count=262144
     
       ---------------------------------------------------------------------------------
     
       # 수정 내역 적용
       $ source /etc/profile
       ```

4. 시작
    ```shell
    # shell 파일 존재하는 디렉토리로 이동
    $ cd ~/workspace/user/{작업디렉터리명}/sonarqube-8.9.2.46101/bin/linux-x86-64
   
    # SonarQube 서비스 상태 확인
    $ ./sonar.sh status
   
    # SonarQube 서비스 시작
    $ ./sonar.sh start
    ```
    
    - 설정 정보 변경 후 서비스 재시작(필수)
        ```shell
        $ ./sonar.sh restart
        ```

5. 웹 브라우저로 접속
   - 기본 포트 : http://[IP]:9000
   - 지정 포트 : http://[IP]:8083