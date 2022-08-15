# GCP 네트워크 구성 가이드

## 1. VPC 네트워크 생성

- GCP에서는 vpc 생성 시 subnet을 동시에 생성
- `VPC 네트워크 만들기` 선택
- VPC 이름 지정
	+ paas-ta-ta-vpc
- VPC 네트워크 ULA 내용 IPv6 범위
	+ `사용 중지됨` 선택
- 서브넷 생성 모드
	+ `커스텀` 선택
- 서브넷 추가
- 서브넷 이름 지정
	+ paas-ta-ta-public-subnet
	+ paas-ta-ta-private-subnet
- 시브넷 리전 선택 : `asia-notheast3` 선택
	+ `asia-notheast3` :: 서울 리전
- `IP4(단일 스펙)` 선택
	+ 필요한 경우 `이중 스택` 선택
- IP4 주소 범위 작성
	+ paas-ta-ta-public-subnet
		- 10.0.0.0/24
	+ paas-ta-ta-private-subnet
		- 10.0.1.0/24
- 비공개 Google 액세스
	- 사용 안 함
- 방화벽 규칙
	+ paas-ta-ta-vpc-allow-custom 선택
	+ VPC 생성 완료 후 방화벽 규칙 수정 필요
- 동적 라우팅 모드
	+ `리전` 선택
- `만들기` 선택

 VPC 네트워크 구성 완성

![](https://velog.velcdn.com/images/imsooyeon/post/673c1e8f-3131-4a07-984b-b435de5413b5/image.png)

## 2. 방화벽 규칙 수정

- 생성된 VPC 선택 > 방화벽 > 방화벽 규칙 추가
- 방화벽 이름 지정
	+ paas-ta-ta-vpc-allow-all
	+ paas-ta-ta-vpc-allow-http
	+ paas-ta-ta-vpc-allow-https
- 대상
	+ 네트워크의 모든 인스턴스
- 소스 필터
	+ IPv4 범위
- 소스 IPv4 범위
	+ 0.0.0.0/0
- 프로토콜 및 포트
	+ paas-ta-ta-vpc-allow-all
		- `모두 허용` 선택
	+ paas-ta-ta-vpc-allow-http
		- `지정된 프로토콜 및 포트`
		- TCP :: 80
	+ paas-ta-ta-vpc-allow-https
		- TCP :: 443
- 나머지는 기본 설정
- `저장` 선택

방화벽 정책 완성

![](https://velog.velcdn.com/images/imsooyeon/post/d63c25f4-3504-4c7f-9b11-b6c9ccc957dd/image.png)

- GCP에서 gateway는 별도 생성하지 않음

## 3. Cloud Router 생성

- 하이브리드 연결 > Cloud router > 라우터 만들기
- 이름
	+ paas-ta-ta-cloud-router
- 네트워크
	+ paas-ta-ta-vpc
- 리전
	+ asia-northeast3
- 나머지는 기본 설정
- `만들기` 선택

Cloud router 완성

![](https://velog.velcdn.com/images/imsooyeon/post/f5e6b7fd-1a2b-47c0-8604-e740c57ebd90/image.png)

## 4. Cloud NAT 생성
- 네트워크 서비스 > Cloud NAT > CLOUD NAT 게이트웨이 만들기
- 게이트웨이 이름
	+ paas-ta-ta-nat-gatway
- 네트워크
	+ paas-ta-ta-vpc
- 리전
	+ asia-northeast3
- Cloud Router
	+ paas-ta-ta-cloud-router
- 소스(내부)
	+ 커스텀
	+ private subnte 설정
- 나머지는 기본 설정
- `만들기` 선택

![](https://velog.velcdn.com/images/imsooyeon/post/1ad74e4b-33aa-4cd0-a8ed-e47a64a8a4a2/image.png)

![](https://velog.velcdn.com/images/imsooyeon/post/a2054051-0870-41d1-87de-ee12c97503bb/image.png)

> 참고
> - https://twowinsh87.github.io/etc/2020/09/13/etc-gcp-1/

## 5. 인스턴스 생성

- Compute Engine > VM 인스턴스 > 인스턴스 만들기
- 이름
	+ paas-ta-ta-inception
- 리전
	+ asia-northeast3
- 영역
	+ asia-northeast3-a - asia-northeast3-c 중 선택
- 시리즈
	+ E2
- 머신 유형
	+ e2-standard-4
	+ vCPU 4개, 16GB 메모리
- 부팅 디스크 > 변경
- 운영체제
	+ ubuntu
- 버전
	+ ubuntu 20.04 LTS amd64
- 크기
	+ 100
- 나머지는 기본 설정
- `선택` 선택

![](https://velog.velcdn.com/images/imsooyeon/post/bbbbb431-97db-4043-b74d-bb186befeaec/image.png)

- 방화벽
	+ HTTP, HTTPS 트래픽 허용 체크

- 나머지는 기본 설정
- `만들기` 선택
- 인스턴스 생성 완료 시 SSH를 눌러 브라우저에서 VM 접속 확인

## 6. SSH KEY 생성

- SSH 접속을 위한 key 생성
- 로컬 터미널에서 키 생성 명령어 실행
- key 생성 시 ~/.ssh 안에는 두 개의 key가 생성 됨
```shell
$ ssh-keygen -t rsa -f ~/.ssh/[키이름] -C [GCP 계정 or gmail계정] -b 2048

$ ssh-keygen -t rsa -f ~/.ssh/paas-ta-ta-gcp-key -C paasta_ccc -b 2048

# 생성 후 key 확인
$ ll ~/.ssh
total 152
-rw-------  1 imsooyeon  staff   1.8K  7 28 15:01 paas-ta-ta-gcp-key
-rw-r--r--  1 imsooyeon  staff   392B  7 28 15:01 paas-ta-ta-gcp-key.pub
```

- 인스턴스 선택 > 수정
- SSH 키
	+ 항목 추가 선택
	+ .pub으로 끝나는 키 추가

```shell
# 명령어 실행 시 나오는 key 내용 입력
$ cat ~/.ssh/paas-ta-ta-gcp-key.pub
```
- `저장` 선택
- 접속 확인
- 계정은 브라우저에서 터미널 접속 시 확인 후 입력
```shell
$ ssh -i [키 파일 경로] [계정]@[외부IP]
$ ssh -i ~/.ssh/paas-ta-ta-gcp-key paasta_ccc@34.64.253.254
```

> 참고
> - https://minimin2.tistory.com/172


## GCP CRENDENTAL JSON 생성
- BOSH 배포 시 GCP에 접근하기 위한 인증정보 발급 필요
- IAM 및 관리자 > 서비스 계정 > 계정 메일 선택 > 키 > 키 추가 > JSON > `만들기` 선택
- inception 접속
	+ ~/.ssh 하위에 JSON 파일 복사



