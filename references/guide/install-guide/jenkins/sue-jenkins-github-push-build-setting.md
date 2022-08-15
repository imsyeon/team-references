# GitHub push 시 build 자동화 설정

> - github에 push 시 jenkins에서 자동으로 build 하는 설정 방법
> - jenkins에 프로젝트 구성은 완료되어 있다고 가정하고 진행

## 1. Jenkins 프로젝트 구성 설정

- `프로젝트` > `구성` > `빌드 유발` > `GitHub hook trigger for GITScm polling` 체크 > 저장

![](https://velog.velcdn.com/images/imsooyeon/post/909f50da-ba60-4a1d-8ea1-73bcf2e2f65b/image.png)


## 2. GitHub 설정

- jenkins에 연동되어 있는 github Repository > `setting` > `webhooks` > `Payload URL` > http://{jenkins-server-주소}/github-webhook/ 입력 > 저장

![](https://velog.velcdn.com/images/imsooyeon/post/0efd486f-45e7-48a7-b46e-dffc80236e0c/image.png)

## 3. push 및 build 확인

- code 수정 후 commit & push 실행
- jenkins `build history` 확인
- 해당 `build history` consol 확인 시 build 확인 가능

![](https://velog.velcdn.com/images/imsooyeon/post/f9d0a243-dd6a-479d-9a60-1952e82756ba/image.png)

