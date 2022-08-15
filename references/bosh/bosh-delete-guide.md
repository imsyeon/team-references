# bosh 리소스 초기화 가이드

## 1. bosh clean-up 명령어 실행

```
$ bosh clean-up --all
```

## 2. bosh 삭제

```
$ ./delete-deploy-aws.sh
```

## 3. Home 디렉토리에서 폴더 삭제

```
$ rm -rf ~/.bosh/
$ rm -rf ~/.cache/
$ rm -rf ~/.cf/
$ rm -rf ~/.credhub/
$ rm -rf ~/.uaac.yml/
```

## 4. creds.yml 삭제

```
$ rm /home/ubuntu/workspace/paasta-deployment/bosh/aws/creds.yml
```

## 5. .profile 주석

- bosh 로그인 시 사용했던 환경변수 주석 처리

```
$ vi ~/.profile
...
#export BOSH_ENVIRONMENT='10.0.1.6'
#export BOSH_CLIENT='admin'
#export BOSH_CLIENT_SECRET=`bosh -e ${PAASTA_BOSH_DIRECTOR} int ${PAASTA_DEPLOYMENT_WORKSPACE}/bosh/${PAASTA_BOSH_IAAS}/creds.yml --path /admin_password`
```