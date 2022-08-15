# SCM-Manager install GUIDE

## 1. 종류
> - docker
> - apt 패키지 관리자
> - 일반 유닉스 설치

이 중 일반 유닉스 설치 방법으로 진행

<br />

## 2. 필요 환경
- [OpenJDK 11](kevin-open-jdk-install-guide.md) (JAVA_HOME 세팅 필요)
- Ubuntu 18.04

<br />

## 3. 설치
1. SCM-Manager tar 다운로드
    ```shell
    $ wget -P ${workspace} https://packages.scm-manager.org/repository/releases/sonia/scm/packaging/unix/2.23.0/unix-2.23.0.tar.gz
    ```
2. tar.gz 압축해제
    ```shell
    $ tar xvfz ${workspace}/unix-2.23.0.tar.gz
    ```
3. port번호 설정
    ```shell
    $ vi ${workspace}/scm-server/conf/server-config.xml
    
    # 8080 기본 port를 8081로 변경
    =====================================================
    <SystemProperty name="jetty.port" default="8081" />
    =====================================================
    ```
4. scm-server 실행
    ```shell
    # 기본 실행
    $ ${workspace}/scm-server/bin/scm-server
    
    # 백그라운드 실행
    $ ${workspace}/scm-server/bin/scm-server start
    ```

<br />

### 4. 접속 정보(초기)
- url : http://10.200.33.124:8081
- ID : scmadmin
- PW : scmadmin
