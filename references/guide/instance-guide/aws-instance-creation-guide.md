# AWS instance 생성 가이드
## 1. vpc 생성

- 네트워킹 및 콘텐츠 전송 > VPC

<img src = "https://user-images.githubusercontent.com/86212069/141727672-79eb6aec-154d-4925-ac84-559668b92224.png" width="100%" height="300">

- 이름 태그 입력
    - 이름 태그
        + ex) paasta-ccc-{prefix}-vpc

- IPV4 CIDR 블록 입력
    - 공인망 대역과 겹치게 되면 나중에 외부 통신이 불가능한 경우가 생길 수 있으니 사설망 대역으로 작성
    - 사설망 대역 :: 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16
    - 현재 팀에서 사용하고 있는 IP 대역
        + 10.0.0.0/16

<img src = "https://user-images.githubusercontent.com/86212069/141727699-37f3d9de-abdc-4148-9af9-5ed5cc41f4b7.png" width="100%" height="600">

- 생성 완료 시 리스트에서 확인 가능
- DNS 호스트 이름 활성화로 편집
    - 작업 > DNS 호스트 이름 편집 > 활성화
-  VPC 생성 완료 시 DHCP 옵션세트는 자동으로 생성됨

## 2. 라우팅 테이블 생성

- 네트워킹 및 콘텐츠 전송 > VPC > 라우팅 테이블
- public 라우팅 테이블 생성
    - 이름
        + ex) paasta-ccc-{prefix}-public-routing-table
    - VPC 지정
        + ex) paasta-ccc-{prefix}-vpc
- private 라우팅 테이블 생성
    - 이름
        + ex) paasta-ccc-{prefix}-private-routing-table
    - VPC 지정
        + ex) paasta-ccc-{prefix}-vpc

## 3. 서브넷 생성

- 네트워킹 및 콘텐츠 전송 > VPC > 서브넷
- VPC 내에서 서브넷 생성한 후 가용존과 연결 → 해당 서브넷에서 EC2 Instance 생성 가능
- public 서브넷 생성
    - VPC 지정
        + ex) paasta-ccc-{prefix}-vpc
    - 이름 태그
        + ex) paasta-ccc-{prefix}-public-subnet
    - 가용영역
        + ex) ap-northeast-2a
    - IPv4 CIDR은 VPC CIDR 범위에 포함되는 블록값을 입력
        + ex) 10.0.0.0/24

- private 서브넷 생성
    - 6개의 private subnet 생성
    - VPC 지정
        + ex) paasta-ccc-{prefix}-vpc
    - 이름 태그
        + ex) paasta-ccc-{prefix}-private-subnet-1
    - 가용영역
        + ex) ap-northeast-2a
    - IPv4 CIDR은 VPC CIDR 범위에 포함되는 블록값을 입력
        + paasta-ccc-{prefix}-private-subnet-1 :: 10.0.1.0/24
        + paasta-ccc-{prefix}-private-subnet-2 :: 10.0.41.0/24
        + paasta-ccc-{prefix}-private-subnet-3 :: 10.0.81.0/24
        + paasta-ccc-{prefix}-private-subnet-4 :: 10.0.121.0/24
        + paasta-ccc-{prefix}-private-subnet-5 :: 10.0.161.0/24
        + paasta-ccc-{prefix}-private-subnet-6 :: 10.0.201.0/24

<img src= "https://user-images.githubusercontent.com/86212069/141743885-e556d930-9f30-4c89-a3cd-81c59255d6c5.png"  width="100%" height="700">

## 4. 인터넷 게이트웨이 생성

- VPC에 인터넷 게이트웨이 연결
    - VPC > 인터넷 게이트웨이 > 인터넷 게이트웨이 생성
    - 이름 태그
        + ex) paasta-ccc-{prefix}-internet-gateway

- 작업 > VPC 연결
- VPC 선택 > 연결
    - 인터넷 게이트웨이 상태 :: `attached` 시 연결 성공
    
## 5. NAT 게이트웨이 생성

- 탄력적 IP 할당
    - VPC > 탄력적 IP > 탄력적 IP 주소 할당
    - 네트워크 경계 그룹
        + ex) ap-northeast-2
    - 할당 클릭
    - 이름 입력
        + ex) paasta-ccc-{prefix}-nat-gateway-elastic-ip

- NAT 게이트웨이 생성
    - VPC > NAT 게이트웨이 > NAT 게이트웨이 생성
    - 이름 입력
        + ex) paasta-ccc-{prefix}-nat-gateway
    - public 서브넷 선택
        + ex) paasta-ccc-{prefix}-public-subnet
    - 연결유형
        + 퍼블릭
    - 탄력적 IP 할당 ID 
        + ex) paasta-ccc-{prefix}-nat-gateway-elastic-ip
    - 생성된 vpc 연결
        + ex) paasta-ccc-{prefix}-vpc
    
## 6. 라우팅 테이블 수정

- public 라우팅 테이블
    - 작업 > 서브넷 연결 편집
        - 해당 vpc의 public subnet 지정
            + ex) paasta-ccc-{prefix}-public-subnet

    - 작업 > 라우팅 편집
        - 대상 
            + ex) 0.0.0.0/0 (외부 통신)
        - 대상 (인터넷 게이트웨이 지정)
            + ex) paasta-ccc-{prefix}-internet-gateway

- private 라우팅 테이블
    - 작업 > 서브넷 연결 편집
        - 해당 vpc의 모든 private subnet 지정
            + ex) paasta-ccc-{prefix}-private-subnet1~6
    - 작업 > 라우팅 편집
        - 대상 
            + ex) 0.0.0.0/0 (외부 통신)
        - 대상 (NAT 게이트웨이 지정)
            + ex) paasta-ccc-{prefix}-nat-gateway
        
 

## 7. 보안 그룹 생성

- 네트워크 > 보안 > 보안 그룹 > 보안 그룹 생성
    - 보안 그룹 이름
        + ex) paasta-ccc-security
    - VPC 선택
        + ex) paasta-ccc-vpc
    - 인바운드 & 아웃바운드 규칙 생성
        - 유형
            + 모든 트래픽
        - 소스 유형
            + Anywhere-IPv4
            + Anywhere-IPv6 :: 추가로 생성

## 8. 키 페어 생성

- 컴퓨팅 > EC2 > 네트워크 및 보안 > 키 페어 > 키 페어 생성 버튼
    - 이름 입력
        + ex) aws-paasta-ccc-key
    - 키 페어 유형
        + RSA
    - 프라이빗 키 파일 형식
        + .pem

## 9. EC2 인스턴스 생성

- 컴퓨팅 > EC2 > 우측 상단에 region을 서울로 선택, 인스턴스 시작 버튼 클릭

- AMI 선택
    - Ubuntu Server 20.04 LTS 선택

- 인스턴스 유형 선택 후 인스턴스 세부 정보 구성 선택
    - m5.xlarge 선택 

 <img src= "https://user-images.githubusercontent.com/86212069/141876722-9b775815-23b8-4788-b55c-3d2c0372d8ef.png" width="100%">

- 생성한 VPC 선택
    + ex) paasta-ccc-vpc
- 생성한 서브넷 선택
    + ex) paasta-ccc-public-subnet
- public IP 
    + 자동 할당 `활성화` 선택
    + EC2 인스턴스 생성 시  public IP 자동으로 할당

- 용량 예약 :: 열기

  <img src= "https://user-images.githubusercontent.com/86212069/141874935-15a107c1-388e-445d-bdb1-7fccb19dbf48.png"  width="100%">

- 스토리지 추가
    - /dev/sda1 선택
    - 크기 :: 200 GiB

- 태그 추가
    - 설정하지 않음

- 보안 그룹 구성
    - 인스턴스에 대한 트래픽을 제어하는 방화벽 설정
    - 보안 그룹 구성 기본 설정 :: SSH 포트로 유입되는 모든 트래픽이 허용되도록 등록되어 있음
    - 기존 보안 그룹 선택
        - 기존에 생성한 보안 그룹 선택
            + ex) paasta-ccc-security

- 기존에 생성한 키페어 선택
    + ex) aws-paasta-ccc-key 선택

- 처음 실행 시 pending 상태 → 실행 완료 후 running 상태로 변경되는 것을 확인