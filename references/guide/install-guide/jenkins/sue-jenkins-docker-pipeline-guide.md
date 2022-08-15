# Jenkins Docker pipeline guide

> - 다른 프로젝트에서 build 작업 완료 시 docker로 build 및 push 하는 pipeline 구성
> - 전제조건
>   + 스프링부트 프로젝트를 build 하는 job이 구성되어야 함
>   + jenkins가 구성되어 있는 환경에 Docker가 설치되어 있어야 함

## 1. Docker pipeline 플러그인 설치

- Docker를 사용하여 pipeline을 구축하기 위해서는 Docker pipeline 플러그인 설치가 필요
- `Dashboard` > `Jenkins 관리` > `플러그인 관리` > `설치 가능` > docker pipeline 검색 후 설치 > `설치가 끝나고 실행중인 작업이 없으면 Jenkins 재시작` 체크 시 jenkins 재시작 

![](https://velog.velcdn.com/images/imsooyeon/post/20f0163c-8725-4694-b218-9aed9a1583b1/image.png)


## 2. Docker Hub Credentials 등록

- DockerHub token을 등록해 줘야 jenkins를 통해 docker push 시 Docker Hub 로그인 과정에서 권한 관련 오류가 나지 않음
- Docker Hub token 발급
- Docker Hub 접속 > `Account Settings` 클릭 > `Security` > `New Access Tokens` 클릭 > `Access Token Description ` 입력 (원하는 이름으로 지정) > `Generate` 클릭 > token 복사
- token은 창이 꺼지면 다시 확인이 불가하므로 copy 후 저장 필수

![](https://velog.velcdn.com/images/imsooyeon/post/23e9c4de-e511-4c93-a952-b63860eb8334/image.png)

- Jenkins 대시보드에서 `Jenkins 관리` > `Manage Credentials` > `Add Credentials` 선택
- `Username`: Docker hub id 입력
- `Password`: Docker hub token 입력
- `ID`: 원하는 credetial 이름 지정
- ex) dockerhub-jenkins

![](https://velog.velcdn.com/images/imsooyeon/post/efe74ff7-13cd-4015-b4a4-42a110a4db8c/image.png)

## 3. pipeline 생성

- `Dashboard` > `새로운 item` > `Pipeline` 선택 후 이름 입력 > `ok`

![](https://velog.velcdn.com/images/imsooyeon/post/9a40992b-172e-4a52-87d5-5a301a5ef9e4/image.png)

- 스프링 부트 프로젝트 build 작업 완료 후 파이프라인이 실행되기 위해서는 별도의 설정이 필요
- `Build Triggers` 에서 `Build after other projects are built` 선택 > `Projects to watch`에 파이프라인과 연결할 project 이름을 입력 > `Trigger only if build is stable` 선택

![](https://velog.velcdn.com/images/imsooyeon/post/3da7e8bb-3248-4676-ac96-42221cf7acad/image.png)

- `Pipeline` 에 Pipline script 작성
- pipeline이 실행될 과정을 script로 작성
- pipeline script

```shell
pipeline { 
    environment { 
        repository = "sue/jenkins"  //docker hub id와 repository 이름
        DOCKERHUB_CREDENTIALS = credentials('sue-dockerhub') // jenkins에 등록해 놓은 docker hub credentials 이름
        dockerImage = '' 
  }
  agent any 
  stages { 
      stage('Building our image') { 
          steps { 
              script { 
                  sh "cp /var/lib/jenkins/workspace/sue_jenkins_project/build/libs/sue-member-0.0.1-SNAPSHOT.war /var/lib/jenkins/workspace/pipeline/" // war 파일을 파이프라인 폴더로 복사 
                  dockerImage = docker.build repository + ":$BUILD_NUMBER" 
              }
          } 
      }
      stage('Login'){
          steps{
              sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin' // docker hub 로그인
          }
      }
      stage('Deploy our image') { 
          steps { 
              script {
                sh 'docker push $repository:$BUILD_NUMBER' //docker push
              } 
          }
      } 
      stage('Cleaning up') { 
		  steps { 
              sh "docker rmi $repository:$BUILD_NUMBER" // docker image 제거
          }
      } 
  }
    }
    
```

![](https://velog.velcdn.com/images/imsooyeon/post/5855dc19-a082-4886-b89f-6affc7150ec1/image.png)

- 작성 완료 후 `저장` 선택

## 4. Dockerfile 작성

- image를 build 할 수 있는 Dockerfile 작성 필요
- 파이프라인 프로젝트가 있는 폴더 하위에 Dockerfile을 생성 

```shell
$ sudo vi /var/lib/jenkins/workspace/pipeline/Dockerfile

FROM openjdk:8-jre-slim
  
WORKDIR /app

COPY ./sue-member-0.0.1-SNAPSHOT.war .

ENTRYPOINT ["java","-jar","-Dserver.port=8080","sue-member-0.0.1-SNAPSHOT.war"]
```

## 5. Docker hub 로그인

- docker가 설치되어 있는 vm에서 docker hub 로그인 진행

```shell
$ docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: paastaccc
Password: 
WARNING! Your password will be stored unencrypted in /home/ubuntu/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```

## 6. pipeline 실행

- 스프링부트 프로젝트를 build 하는 job 실행 후 작업 완료 시 docker pipeline이 작업되는 과정 확인
- 스프링부트 프로젝트 build 시작

![](https://velog.velcdn.com/images/imsooyeon/post/886e5b59-2de2-4dc2-a0d7-497b4d38ab21/image.png)

- 스프링부트 프로젝트 완료 후 docker pipeline build 시작

![](https://velog.velcdn.com/images/imsooyeon/post/ed753663-38ab-40a3-8ba3-26c1f4885688/image.png)

- docker pipeline 콘솔 로그

![](https://velog.velcdn.com/images/imsooyeon/post/30d8f0fa-45ce-470d-867b-2d2723372953/image.png)

> ### 참고
> - Docker 실행 시 unix:///var/run/docker.sock의 permission denied 발생할 경우
> - /var/run/docker.sock 파일의 권한을 666으로 변경하여 그룹 내 다른 사용자도 접근 가능하게 변경
> - 에러 메세지
> - Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post
> - 해결방법
> ```
>$ sudo chmod 666 /var/run/docker.sock
>```