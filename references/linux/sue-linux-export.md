# 환경변수
## 1. 로그인 환경변수 
### .profile
- 영구적인 환경 설정 스크립트
- 로그인을 위한 환경설정을 저장 
- 로그인 시 필요한 설정들을 작성 후 저장 시 로그인 할 때 마다 자동으로 불러옴
###  /etc/profile
-  시스템 환경 설정 파일을 의미함
- 모든 계정에 공통 적용
    - ex) root 계정 로그인 시 /etc/profile을 적용
- 모든 사용자가 로그인 할 때 마다 무조건 실행
### ~/.profile
- 사용자 환경 설정 파일
- 해당하는 로그인 계정 사용자에게만 적용
    - ex) unbuntu 계정으로 로그인 시 .profile 적용
      ```shell
      $ vi .profile
      
      # BOSH 로그인
      export PAASTA_INCEPTION_USER_NAME='ubuntu'
      export PAASTA_DEPLOYMENT_WORKSPACE=/home/${PAASTA_INCEPTION_USER_NAME}/workspace/paasta-5.5.0/deployment
      export PAASTA_USER_WORKSPACE=/home/${PAASTA_INCEPTION_USER_NAME}/workspace/user/joy
      export PAASTA_BOSH_DIRECTOR='micro-bosh'
      export PAASTA_BOSH_IAAS='aws'
      export BOSH_ENVIRONMENT='10.0.1.6'
      export BOSH_CLIENT='admin'
      export BOSH_CLIENT_SECRET=`bosh -e ${PAASTA_BOSH_DIRECTOR} int ${PAASTA_DEPLOYMENT_WORKSPACE}/paasta-deployment/bosh/${PAASTA_BOSH_IAAS}/creds.yml --path /admin_password`
      ```

<br />

## 2. 전역환경변수
- export 명령어를 사용해 환경 변수로 저장
- 터미널이 열려있는 동안만 사용 가능
    - 터미널을 닫게 되면 사라짐
- 선언 및 초기화
    ```shell
    $ export 변수명 = 값
        
    $ export JAVA_HOME=/usr/java/jdk1.6.0_45    
    ```
- 변수 선언 확인
    ```shell
    $ env

    SSH_CONNECTION=61.253.112.26 64350 10.0.0.101 22
    LESSCLOSE=/usr/bin/lesspipe %s %s
    LANG=C.UTF-8
    DISPLAY=localhost:10.0
    XDG_SESSION_ID=181
    USER=ubuntu
    PWD=/home/ubuntu
    HOME=/home/ubuntu
    SSH_CLIENT=61.253.112.26 64350 22
    XDG_DATA_DIRS=/usr/local/share:/usr/share:/var/lib/snapd/desktop
    SSH_TTY=/dev/pts/0
    ...
    ```
- 환경변수 해제
    ```shell
    $ unset 환경변수명
  
    $ unset JAVA_HOME
    ```




