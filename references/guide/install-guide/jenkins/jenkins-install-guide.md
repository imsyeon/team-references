# Jenkins install GUIDE

## 1. 종류
> - docker
> - apt 패키지 관리자
> - war file 설치

이 중 war 파일 및 apt 패키지 관리자를 이용하여 설치하는 방법으로 진행

<br />

## 2. 필요 환경
- [OpenJDK 11](kevin-open-jdk-install-guide.md) (JAVA_HOME 세팅 필요)
- Ubuntu 18.04

<br />

## 3. 설치
### 3.1 war file
1. Jenkins 실행파일 war 다운로드
   ```shell
   $ wget -P ${workspace} http://mirrors.jenkins.io/war-stable/latest/jenkins.war
   ```

2. Jenkins 실행
   ```shell
   $ nohup java -jar ${workspace}/jenkins.war --httpPort=${HTTP_PORT} &
   ```

<br />

### 3.2 apt 패키지 관리자
1. Jenkins 데비안 저장소 추가
   ```shell
   # Jenkins 저장소의 GPG 키 가져오기
   $ wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
   
   # 시스템에 Jenkins 리포지토리 추가
   $ sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
   ```

2. Jenkins 설치
   ```shell
   # 패키지 목록 업데이트
   $ sudo apt update
   
   # jenkins 최신 버전 설치
   $ sudo apt install jenkins
   ```

3. 실행 확인
    - Jenkins 서비스는 설치 프로세스 완료 후 자동 실행
       ```shell
      $ systemctl status jenkins
      ```

4. 포트 변경
    - jenkins 파일 수정
       ```shell
      $ sudo vi /etc/default/jenkins
      ```
        - 변경 전
           ```shell
          ...
          # port for HTTP connector (default 8080; disable with -1)
          HTTP_PORT=8080
          ```
        - 변경 후
           ```shell
          ...
          # port for HTTP connector (default 8080; disable with -1)
          HTTP_PORT=9090
          ```
    - jenkins 재실행
      ```shell
      $ sudo systemctl restart jenkins
      ```

5. Jenkins 설정
    1. Jenkins 브라우저 접속
       > http://(hostIp or hostName):9090
    2. Administrator password 입력
        - password 확인
           ```shell
          $ sudo cat /var/lib/jenkins/secrets/initialAdminPassword
          ```
    3. plugin 설치
        - `Install suggested plugins` 선택
    4. 계정 설정
       > + 계정명 : admin
       > + 암호 : PaaS-TA@2021
        - 브라우저를 통해 Jenkins 접속시 필요한 정보
        - 모든 항목 기입 필요
    5. Instance Configuration
        - Jenkins URL 확인 후 `Save and Finish`
          > http://{hostIP}:9090/
    6. `Start using Jenkins` 클릭
   
<br />

## 4. Jobs
### 4.1 Jenkins와 Github 연동
1. Github
   1. `Settings` > `Developer settings` > `Personal access tokens` 이동
   2. `Generate new token` 선택 
      1. New personal access token
         - Note :: token명 
         - Select scopes 
           + 전체 항목 체크
             - repo 
             - admin:org 
             - admin:repo_hook
      2. `Generate token` 버튼 클릭
   3. 화면에서 access tokens 발급 내역 확인 가능
      > 주의) 화면에서 벗어나는 경우 재확인 불가능하므로 **저장 필수**
2. Jenkins
   1. `Jenkins 관리` > `Manage Credential`
      1. `Stores scoped to Jenkins` 하위 항목 클릭하여 `System`으로 이동
      2. Credential 추가 창으로 이동(택 1)
         1. `Global credential ▼` 버튼 클릭
         2. `Global credential` 버튼 클릭 > 좌측의 `Add Credentials` 클릭
      3. Credential 추가
         1. Kind :: Username with password
         2. Username :: Github 아이디
         3. Password :: Github Token 값
         4. ID
            - Git이 Jenkins에게 Credential을 줄 때 인식하게끔 하는 값
            - 원하는 값으로 설정
   2. `Jenkins 관리` > `System configuration` > `시스템 설정` 클릭
      1. 하단의 `Github` 항목 > `Github Server`
         - Name :: 원하는 이름으로 설정
         - API URL :: https://api.github.com
         - Credentials :: 추가해놓은 credential 선택
      2. `Test connection` 클릭하여 정상 연동 확인
<br />
      
### 4.2 build & deploy
1. `Jenkins 관리`
   1. `Global Tool Configuration` > `Gradle` > `Add Gradle` 클릭
        + name :: 원하는 이름으로 설정
        + `Install automatically` 선택
        + Install from Gradle.org
          + Version :: 프로젝트에서 사용되는 Gradle 버전
      - `Save` 버튼 클릭하여 저장
   2. `플러그인 관리` > 'Post build task' 검색하여 설치

2. 신규 Item 생성
   1. `새로운 Item` 클릭 > 원하는 project명 입력 > `Freestyle project` 클릭
   2. `General`
      1. `GitHub project` 체크 > `Project url`에 Github project URL 입력<br />
      ex) https://github.com/rexx4314/paas-ta-ccc-team-dev/
      2. `소스 코드 관리` > `Git` 체크
         1. Repositories
            - Repository URL :: Github repository URL<br />
            ex) https://github.com/rexx4314/paas-ta-ccc-team-dev.git
            - Credentials :: 추가해놓은 credential 선택
         2. Branches to build
            - Branch Specifier :: 특정 브랜치만 빌드하고 싶을 경우 지정<br />
            ex) */lv3-joy-spring-boot-gradle-jsp-restful
      3. Build > `Add build step` 클릭 > `Invoke Gradle script` 선택
         > 빌드 시 수행할 작업 설정
            - `Invoke Gradle` 선택
            - Gradle Version :: 앞에서 지정한 Gradle 버전 선택
            - Tasks
              ```
              clean
              bootWar
              ```
            - 하단의 `고급...` 버튼 클릭
              - Root Build script :: Build File이 존재하는 경로
                - 최상위 디렉토리에 있을 경우 :: `/var/lib/jenkins/workspace/{item명}`
                - 그 외 :: `/var/lib/jenkins/workspace/{item명}/{하위1}/{하위2}/...`
              - Build File :: build.gradle
      4. 빌드 후 조치 > `빌드 후 조치 추가` 클릭 > `Post build task` 선택
         > 빌드 후 deploy하기 위한 작업 설정
         - Tasks :: 빌드 성공시 출력할 내용 지정
           - Log text :: 출력하기 원하는 텍스트 내용 지정 가능
             - 입력값 :: BUILD SUCCESS
           - Operation :: `--AND--`
         - Script
           ```
           nohup java -jar /var/lib/jenkins/workspace/{item명}/build/libs/{배포할 파일명} &
           ```
      5. `저장` 버튼 클릭
3. 좌측의 `Build Now` 클릭 > `Build History`의 최근 Build 내역 클릭 > `Console Output` 확인
<br />

### 4.3 배포 확인
1. 터미널에서 확인
   ```shell
   $ sudo netstat -anlp | grep :포트번호
   
   # 예시
   $ sudo netstat -anlp | grep :8085
   tcp6       0      0 :::8085                 :::*                    LISTEN      21100/java
   ```
2. 웹 브라우저에서 확인
   - url :: http://(hostIp or hostName):포트번호