# Linux hosts
/etc/hosts 파일 :: 특정 URL 주소에 접속할 때 DNS 서버에 질의하지 않고 지정된 IP 주소로 연결해 주는 설정 파일

## 1. DNS(Domain Name System) 
    + 사람이 읽을 수 있는 도메인 이름을 머신이 읽을 수 있는 IP 주소로 변환
    + 예시 :: naver.com > 223.130.200.107

## 2. hosts 수정
- IPv4 :: {target IP} {domain}의 방식으로 추가
    + domain :: 사용자가 정의한 주소
    + target IP :: 사용자가 정의한 domain을 입력 했을 때 사용되는 실제 IP 
- 예시
```
$ sudo vi /etc/hosts

127.0.0.1 localhost
10.100.1.6 test.domain
```

## 3. hosts 적용 확인
- domain이 바라보는 IP를 확인하는 명령어인 nslookup 사용
- 적용 전
```
$ nslookup test.domain
Server:		127.0.0.53
Address:	127.0.0.53#53

** server can't find test.domain: NXDOMAIN
```
- 적용 후
    + test.domain 입력 시 10.100.1.6를 바라보는 것 을 확인
```
$ nslookup test.domain
Server:		127.0.0.53
Address:	127.0.0.53#53

Non-authoritative answer:
Name:	test.domain
Address: 10.100.1.6
```