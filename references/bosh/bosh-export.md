# BOSH 로그인 환경변수

### 1. export BOSH_ENVIRONMENT

- BOSH_ENVIRONMENT 환경변수 설정 시 --environment 옵션 생략 가능

```
# .profile에 환경변수 설정
$ vi .profile
export BOSH_ENVIRONMENT='micro-bosh'

# bosh ip alias 설정
$ bosh alias-env micro-bosh -e 10.0.1.6 --ca-cert <(bosh int ./aws/creds.yml --path /director_ssl/ca)
```
### 2. export BOSH_CLIENT_SECRET
- bosh int(interpolate) 명령어를 사용해서 creds.yml에 있는 admin_password를 갖고 옴

```
export BOSH_CLIENT_SECRET=$(bosh int ./{iaas}/creds.yml --path /admin_password)
```

```
#creds.yml 안에 admin_password
$ vi creds.yml
admin_password: j0f1p12x4tn4b7spmptn
 
$ bosh int ./aws/creds.yml --path /admin_password
j0f1p12x4tn4b7spmptn
```
 


