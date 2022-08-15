# Jenkins 프로젝트(Job) 생성 및 자동 배포 구성하기

## 1. 프로젝트(Job) 생성

- `Dashboard` > `새로운 item` 선택 
![](https://velog.velcdn.com/images/imsooyeon/post/14d0765d-744c-4a8c-981c-4faf166c06be/image.png)

- `Freestyle project` 선택 및 프로젝트 이름 작성 > `ok` 선택
![](https://velog.velcdn.com/images/imsooyeon/post/d1e45ea6-187c-4e30-87d2-b6b2713e3c5e/image.png)

## 2. Github 연동

- github 연동을 위한 과정

![](https://velog.velcdn.com/images/imsooyeon/post/9e1e4208-7fa4-407d-9d77-bf0dd35d0f82/image.png)

- 소스 코드 관리에서 `Git` 선택 > Repository URL 입력
- Repository URL :: 소스 코드를 관리하는 내 repository의 주소 입력

![](https://velog.velcdn.com/images/imsooyeon/post/3822cb7e-4f0a-49a4-8f9a-5441fefa58df/image.png)

- Credentials 등록 > `Add` 선택 > `jenkins` > `Username with password` 선택
- `username` : github Id 입력
- `Password` : github access token 정보 입력
- `id` : 원하는 credential 이름 입력

![](https://velog.velcdn.com/images/imsooyeon/post/6ecdec51-4603-49ba-aa8e-4a6a61a2404d/image.png)

- branch 입력 (특정 branch를 지정해서 입력 가능)
- ex) `*/main`

![](https://velog.velcdn.com/images/imsooyeon/post/209c7613-1f46-43cb-baaf-0adb48ddba2c/image.png)

> 참고
> - github push 시 자동 build 및 배포 설정은 sue-jenkins-github-push-build.md 참고하여 설정

## 3. gradle 설정 및 build

- gradle 설정을 우선 진행 후 build 설정 진행
- `Dashboard` > `Jenkins 관리` > `Global Tool Configuration` > `Gradle installation` 선택 
- 원하는 이름 입력 및 spring boot 프로젝트에서 사용하고 있는 gradle 버전을 지정

![](https://velog.velcdn.com/images/imsooyeon/post/38d6ab77-5f44-4397-9455-e2cae070e011/image.png)


- 이전에 생성한 프로젝트 선택 후 `구성` 선택 > `Add build step` 클릭 후 `invoke Gradle script` 선택 > 아까 생성한 Gradle 버전 선택
- Tasks에 job이 실행될 때 수행할 Script 작성
- Tasks에 clean / bootWar 작성

![](https://velog.velcdn.com/images/imsooyeon/post/1132ca0b-7376-44d6-99a9-2953ecfc9d23/image.png)

## 4. build 후 Deploy

- `Add build step` 클릭 후 `Execute shell` 선택
- Command에 배포 명령어를 입력
- `java -jar build/libs/sue-member-0.0.1-SNAPSHOT.war`
- `저장`

![](https://velog.velcdn.com/images/imsooyeon/post/fc10a560-bc10-4d5f-8316-b84fdd7bcb4b/image.png)

- 해당 프로젝트에서 `Build Now` 클릭
- Build history에서 build 확인 가능
- 해당 build 클릭 후 Console Output 선택 시 해당 프로젝트의 build 과정 확인 가능

![](https://velog.velcdn.com/images/imsooyeon/post/0af9204a-fe6b-413b-8e21-417d9d19fc34/image.png)

![](https://velog.velcdn.com/images/imsooyeon/post/022a5431-dff5-48f9-9a71-4644fcf991cf/image.png)
