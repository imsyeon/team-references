# 파스-타 대표포털 로컬 세팅 가이드

## 1. 파스-타 대표포털 repository clone
- PaaS-TA Official Repository
  + https://github.com/paas-ta/

- PaaS-TA 대표포털 Repository 모두 로컬에 clone

- 프로젝트 관리자에게 PaaS-TA organization member로 초대를 받아야 소스 수정 후 반영 가능

## 2. Java 8 다운로드

- https://www.oracle.com/java/technologies/javase-downloads.html
- 윈도우용으로 다운로드
- Java 환경 변수 설정
  + 윈도우 검색창에 `시스템 환경 변수 편집` 검색 > `환경 변수` 클릭 > `시스템 변수`에 새로 만들기 선택
  + 변수 이름
    - JAVA_HOME
  + 변수 값
    - JDK 설치 경로를 복사
    - ex) C:\Program Files\Java\jdk1.8.0_333
  + 시스템 변수 목록에서 `Path` 선택 > `새로 만들기` > %JAVA_HOME%\bin 추가 > `확인`
  + CMD 창을 열어서 환경 변수 설정이 잘 되었는지 확인

```shell
> java -version
> javac -version
```

## 3. tomacat 다운로드
- tomcat (9.0) 설치
  + https://tomcat.apache.org/download-90.cgi
  + 32-bit/64-bit Windows Service Installer 선택

## 4. eclipse setting

- eclipse 설치
  + https://nextcloud.paas-ta.org/index.php/s/NZWjASLTBizLK7J/download

- eclipce.exe 실행

- 프로젝트 import
  + `File` > `open project from file system` 선택 >   `Directory` 선택 후 clone 받은 Repository 선택

- 프로젝트 import 후 `run as` > `maven build`

> ### ※ 참고사항
> - 아래와 같은 오류 발생 시 해결 방안
> - jdk가 설정되어 있지 않아서 발생한 오류
>```
>[ERROR] Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:3.1:compile (default-compile) on project psta-api-apply: Compilation failure
>[ERROR] No compiler is provided in this environment. Perhaps you are running on a JRE rather than a JDK?
>```
>
>- 해결 방법
>   + 상단 메뉴 `window` > `preferences` > `java` > `installed JREs` > `Add`
>   + `standard VM` > next > `jdk`선택 > 완료
>   + installed JREs 화면에서 jdk 선택 후 `Apply and Close` 클릭


- tomcat 서버 생성
  + tomcat 서버는 프로젝트 당 하나씩 생성해 서버를 구동함
  + **psta-zuul-gateway**는 제외
  + `servers` 창에서 우클릭 > `New`에서 `server` 선택 > Tomcat v9.0 server 선택 > Server name을 각 프로젝트 이름에 맞게 변경 > `Next` > 프로젝트를 선택하여 `Add` 클릭 > `Finish` 선택
  
- tomcat 서버 port 변경

  + 각 프로젝트 마다 port 변경 필요
  + 생성한 서버 더블 클릭 시 `Overview` 창 확인 가능
  + HTTP port를 지정된 port로 변경
  + Tomcat admin port는 각 프로젝트끼리 겹치지 않게만 port 변경 후 저장

```
psta-web-user: 8080    
psta-web-admin: 8090   
psta-api-common: 9030    
psta-api-board: 9040 
psta-api-apply: 9050
psta-api-release: 9060
psta-web-user-en: 8070 
```

- 접속 경로 변경
  + 프로젝트 접속 경로 변경 필요
  + `Modules` 탭 선택 > `Path`에 지정된 경로로 변경 후 저장
  
```
psta-web-user: /         
psta-web-admin: /pstaadmin
psta-api-common: /psta-api-common
psta-api-board: /psta-api-board
psta-api-apply: /psta-api-apply
psta-api-release: /psta-api-relaese
psta-web-user-en: /en
```

- 각 서버에 암호화 설정값 추가
  + `Overview` > `Open launch configuration` > `Arguments` > `VM Arguments`에 `-Djasypt.encryptor.password=paas-ta-msa` 추가

![](https://velog.velcdn.com/images/imsooyeon/post/5a2c757a-2d5b-4936-87c9-8b415fb91ea4/image.png)

- psta-zuul-gateway 암호화 설정값 추가 방법
  + `RoutingAndFilteringGatewayApplication.java` 선택 후 우클릭 > `Run AS` >  `Run configurations` > `Arguments` > `VM Arguments`에 `-Djasypt.encryptor.password=paas-ta-msa` 추가
  
![](https://velog.velcdn.com/images/imsooyeon/post/0f05cfc1-02b7-45c3-8489-5485ef315752/image.png)

## 5. 프로젝트 실행 방법
- `Servers`에 있는 서버들을 하나씩 실행
- psta-zuul-gateway
  + `RoutingAndFilteringGatewayApplication.java` 선택 후 우클릭 > `Run AS` > `Java appication` 클릭