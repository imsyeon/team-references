# AWS Network concept

## 1. VPC (Virtual Private Cloud)

- 사용자의 AWS 계정 전용 가상 네트워크를 의미
- AWS 클라우드에서 다른 고객과 완벽하게 논리적으로 격리된 네트워크 공간을 제공
- 가상 네트워크에서 AWS 리소스를 만드는 데 사용하는 리소스
- VPC 생성 시 IPv4 주소 범위(프라이빗 IPv4 주소 범위)를 CIDR(Classless Inter-Domain Routing) 블록 형태로 지정
    + VPC의 기본 CIDR 블록
        - 10.0.0.0/16
- VPC 생성 가능한 CIDR 블록
    + IPv4 CIDR 블록
    + IPv6 CIDR 블록
    + IPv4 및 IPv6 CIDR 블록 둘 다(듀얼 스택) 할당
    + VPC 생성 시 허용된 CIDR 블록 크키
        - IPv4 :: /16~/28
        - IPv6 :: /56으로 고정
- VPC의 IP 주소 범위를 지정하고 서브넷과 게이트웨이를 추가 후 보안 그룹을 연결해서 사용

![](https://velog.velcdn.com/images/imsooyeon/post/8032168f-073e-419f-9e9b-d8950ce6a5dd/image.png)

> 출처
> - https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/how-it-works.html

## 2. Subnet

- 서브넷이란 거대한 네트워크 대역대의 VPC주소를 여러 개로 나눈 네트워크 주소
- Subnet 유형
    + public subnet
        - 인터넷 게이트웨이를 통해 public internet과 통신
    + private subnet
        - 외부와 차단되어 있음
        - 인터넷 inbound/outbound가 불가능
        - 다른 서브넷과의 연결만 가능
        - public internet과 통신하기 위해서 NAT 장치가 필요
    
![](https://velog.velcdn.com/images/imsooyeon/post/c036f387-8f7c-43aa-b504-5af7c7fe2deb/image.png)

## 3. 인터넷 게이트웨이

- VPC와 인터넷을 연결해 주는 하나의 관문
- VPC의 인스턴스와 인터넷 간에 통신을 지원
- public subnet과 연결하여 외부 통신이 가능하게 해줌

![](https://velog.velcdn.com/images/imsooyeon/post/caa65775-59e0-4970-8106-1b0a40948023/image.png)


## 4. NAT 게이트웨이

- NAT (Network Address Translation) 서비스
- private subnet이 인터넷과 통신하기 위한 outbound 인스턴스
- private subnet의 outbound 트래픽을 받아 인터넷 게이트웨이와 연결
