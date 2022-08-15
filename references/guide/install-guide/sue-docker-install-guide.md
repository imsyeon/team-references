# Docker install guide
- 필요 시 이전 버전 제거 후 진행
  ```shell
  $ sudo apt-get remove docker docker-engine docker.io containerd runc
  ```


## 1. repository 설정
- 패키지 업데이트 및 패키지 설치
  ```shell
  $ sudo apt-get update
  $ sudo apt-get install \
       ca-certificates \
       curl \
       gnupg \
       lsb-release
  ```
- Docker의 공식 GPG 키 추가
  ```shell
  $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
  ```
- repository 안정화
  ```shell
  $ echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  ```

## 2. Docker engine 설치
- 패키지 업데이트 및 설치
  ```shell
  $ sudo apt-get update
  $ sudo apt-get install docker-ce docker-ce-cli containerd.io
  ```
- 설치 가능한 버전 확인
  ```shell
  $ apt-cache madison docker-ce
  ```
- 버전 지정하여 설치
  ```shell
  $ sudo apt-get install docker-ce=5:20.10.11~3-0~ubuntu-bionic docker-ce-cli=5:20.10.11~3-0~ubuntu-bionic containerd.io
  ```
- 설치를 확인하기 위한 hello-world image 실행
  ```shell
  $ sudo docker run hello-world
  ```
- 버전 확인
  ```shell
  $ docker version
  ```
- Docker 권한 설정
  - 사용자에게 root 권한 부여
  ```shell
  $ sudo usermod -aG docker $USER
  $ sudo service docker restart
  ```

---

# Docker 삭제

```shell
# Docker engine, cli 및 패키지 제거
$ sudo apt-get purge docker-ce docker-ce-cli containerd.io

# 이미지, 컨테이너 및 볼륨 삭제
$ sudo rm -rf /var/lib/docker
$ sudo rm -rf /var/lib/containerd
```

---

# Docker 샘플앱 배포

## 1. 샘플앱 다운로드

- docker github에서 다운로드
```shell
$ wget https://github.com/docker/getting-started/archive/refs/heads/master.zip
```

## 2. 컨테이너 이미지 빌드

- app 폴더 하위 package.json, src, pec 존재
- package.json과 동일한 폴더 하위에 Dockerfile 파일 생성
- cd getting-started-master/app/

```shell
$ vi Dockerfile

# syntax=docker/dockerfile:1
FROM node:12-alpine
RUN apk add --no-cache python3 g++ make
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
```
- 컨테이너 이미지 빌드
```shell
$ docker build -t getting-started .
```
- 이미지 확인
```shell
$ docker images
```

## 3. 컨테이너 앱 실행
- 실행 시 포트 및 이름 지정
```shell
$ docker run -dp 3000:3000 getting-started
```
- 프로세스 확인
```shell
$ docker ps
```
- http://{IP}:3000/ 로 접속 후 UI 확인
- docker 정보 확인
```shell
$ docker info
```

---

# Docker image build 및 deploy

## 1. spring boot war file 생성
- build.gradle에 아래 항목 추가
```groovy
plugins {
    id 'war' // war 지정
}

archivesBaseName = 'sue-docker-board' // war file 이름
group = 'com.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '1.8'
```

- 명령어 입력 후 build/lib에서 war file 확인 가능
```shell
$ ./gradlew build
```
- 생성된 war file을 ubuntu로 복사



## 2. Dockerfile 생성
- Docker 이미지를 만들기 위한 스크립트 파일
- ubuntu에서 생성
```shell
$ vi Dockerfile

FROM openjdk:8-jre-slim
  
WORKDIR /app

COPY ./sue-docker-board-0.0.1-SNAPSHOT.war .

ENTRYPOINT ["java","-jar","-Dserver.port=8084","sue-docker-board-0.0.1-SNAPSHOT.war"]
```

### 2.1 Dockerfile 포멧

- 명령어는 모두 대문자로 써 주는 것이 관례
  + 인자와 구분하기 쉽도록 하기 위함

```
# 주석(Comment)
명령어(INSTRUCTION) 인자(arguments)
```


### 2.2 FROM

- Docker 이미지는 base 이미지부터 시작해서 기존 이미지 위에 새로운 이미지를 중첩해서 여러 단계의 이미지 층을 쌓아가면서 만들어짐
- FROM 명령문은 base 이미지를 지정해 주는 역할
- Dockerfile 내 최상단에 위치
- base 이미지는 일반적으로 Docker Hub와 같은 repository에 공개되어 있음

```
FROM <이미지>
FROM <이미지>:<태그>
```
```
# ubuntu 최신 버전을 base 이미지로 사용 시
FROM ubuntu:latest

# NodeJS 12를 base 이미지로 사용 시
FROM node:12
```

### 2.3 WORKDIR 

- `cd` 명령문처럼 컨테이너 상에서 작업 디렉토리로 전환하기 위해 사용
- RUN, CMD, ENTRYPOINT의 명령이 실행될 디렉터리를 설정
- WORKDIR 명령문으로 작업 디렉터리를 전환 시 그 이후에 등장하는 모든 RUN, CMD, ENTRYPOINT, COPY, ADD 명령문은 해당 디렉터리를 기준으로 실행됨
- 중간에 다른 디렉터리를 설정하여 실행 디렉터리 변경 가능

```
WORKDIR <이동할 경로>
```
```
# /usr/app으로 작업 디렉터리 전환
WORKDIR /usr/app

# /var/www/hello.txt 파일 생성
WORKDIR var
WORKDIR www

RUN touch hello.txt
```
### 2.4 COPY 

- 호스트 컴퓨터에 있는 파일을 Docker 이미지의 파일 시스템으로 복사하기 위해서 사용
- 절대 경로, 상대 경로 모두 지원

```
COPY <src>... <dest>
COPY ["<src>",... "<dest>"]
```
```
# 현재 Dockerfile가 위치한 경로의 모든 파일을 /app에 복사
COPY . /app
```

### 2.5 ENTRYPOINT 

- 컨테이너가 시작되었을 때 스크립트 혹은 명령을 실행
  + docker run으로 컨테이너를 생성하거나, docker start로 정지된 컨테이너를 시작할 때 실행
-  Dockerfile에서 단 한번만 사용 가능

```
ENTRYPOINT ["<커맨드>", "<파라미터1>", "<파라미터2>"]
ENTRYPOINT <전체 커맨드>
# war 실행 명령어
ENTRYPOINT ["java","-jar","-Dserver.port=8084","sue-docker-board-0.0.1-SNAPSHOT.war"]
```

## 3. 컨테이너 이미지 빌드
- 컨테이너 이미지 빌드
  - DockerFile을 실행하여 이미지 빌드
```shell
$ docker build [OPTIONS] PATH | URL | -
$ docker build -t {dockerhub repository:tag} {Dockerfile path}
$ docker build -t paastaccc/sue-docker-board:0.1 .
```
- 이미지 확인
```shell
$ docker images
```


## 4. 컨테이너 앱 실행
- 실행 시 포트 및 이름 지정
```shell
$ docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
$ docker run --rm -d -p 8080:8084 --name sue-docker paastaccc/sue-docker-board:0.1
```
  - `-d`
    + 컨테이너를 백그라운드로 실행
  - `-p 8080:8084`
    + 호스트의 8080번 포트와 컨테이너의 8084번 포트를 연결하고 외부에 노출
  - `-rm`
    + 컨테이너를 단기적(일회성)으로 실행 시 주로 사용
    + 컨테이너 종료 시 컨테이너 관련 리소스(파일 시스템, 볼륨) 모두 제거
  - `http://<호스트 IP>:8080` 접속 시 컨테이너의 8084번 포트로 접속됨
  
- 프로세스 확인
```shell
$ docker ps
```
- 배포 확인
```shell
  $ curl {ip}:{port}
  $ curl 3.35.53.49:8080
  ```
- 3.35.53.49:84로 접속 후 UI 확인
- 로그 확인
```shell
$ docker logs -f [CONTAINER ID]
  ```

## 5. 삭제
- 컨테이너 삭제
```shell
$ docker stop [CONTAINER ID]
```
- 이미지 삭제
```shell
$ docker rmi [IMAGE ID]
```
---
# Spring music sample image build 및 deploy

>### 사전 작업
>
> - local에서 spring music을 jar로 build 후 리눅스 환경으로 복사

## 1. Dockerfile 생성

```
$ vi Dockerfile

---
FROM openjdk:11-jre-slim
  
WORKDIR /workspace

COPY spring-music-1.0.jar .

ENTRYPOINT ["java","-jar","-Dserver.port=8080","spring-music-1.0.jar"]
```

## 2. 컨테이너 이미지 빌드

```
# 이미지 빌드
$ docker build -t sue-spring-music .

# 이미지 확인
$ docker images
```

## 3. 컨테이너 앱 실행

```
# 컨테이너 실행
$ docker run -dp 8080:8080 sue-spring-music

# 프로세스 확인
$ docker ps
```

## 4. Docker hub에 이미지 올리기

- docker hub에 로그인 후 image push

```
# docker hub 로그인

$ docker login
Username: [docker hub username]
Password: [docker hub pw]

# 이미지 올리기
$ docker push [이미지 이름]:[tag]
$ docker push paastaccc/k8s-sample:0.1
```

- docker hub 접속 후 이미지 확인

## 5. Docker image 이름 및 tag 변경

```
# 아래 두 방법 모두 가능
$ docker tag [변경할 로컬 이미지 ID] [저장소]:[tag]
$ docker tag [변경할 로컬 이미지]:[tag] [변경할 저장소/이름]:[tag]

$ docker tag 1fd43d9c60b3 paastaccc/spring-music-sample:0.1
$ docker tag getting-started:latest paastaccc/spring-music-sample:0.1
```
- tag 변경 후 docker hub에 push

```
$ docker push paastaccc/spring-music-sample:0.1
```

---
# Docker Cli

## 1. 이미지 검색

- Docker hub에 있는 이미지들을 검색
```
$ docker search [옵션] <검색어>
$ docker search openjdk
NAME                            DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
openjdk                         OpenJDK is an open-source implementation of …   3083      [OK]       
adoptopenjdk/openjdk11          Docker Images for OpenJDK Version 11 binarie…   178                  
adoptopenjdk/openjdk8           Docker Images for OpenJDK Version 8 binaries…   108                  
adoptopenjdk/openjdk8-openj9    Docker Images for Eclipse OpenJ9 Version 8 b…   49                   
adoptopenjdk/openjdk11-openj9   Docker Images for Eclipse OpenJ9 Version 11 …   45                   
arm64v8/openjdk                 OpenJDK is an open-source implementation of …   40                   
adoptopenjdk/openjdk12          Docker Images for OpenJDK Version 12 binarie…   19                   
...
```
- openjdk 공식 이미지들을 포함해 public 이미지들이 STARS(인기) 순서대로 나열됨
