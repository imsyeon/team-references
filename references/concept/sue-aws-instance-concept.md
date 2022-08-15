## 1. AMI (Amazon Machine Image)

- AMI를 통해 인스턴스를 구성
  - 이때 생성된 인스턴스는 AMI 사본
- AMI의 주 구성요소
  - 운영 체제, 애플리케이션 서버, 애플리케이션
- 한 AMI로 여러 인스턴스 실행 가능
- 커뮤니티 AMI 
  - 사용자들 사이에서 자유롭게 개발되고 공유되는 이미지

## 2. 인터넷 게이트웨이

- VPC의 인스턴스와 인터넷 간에 통신을 지원
- public IPv4 주소가 할당된 인스턴스에 대해 네트워크 주소 변환을 수행

## 3. NAT 게이트웨이 

- NAT 게이트웨이 :: NAT (네트워크 주소 변환, Network Address Translation) 서비스
- 외부 접속이 안 되는 private subnet의 인스턴스가 외부의 서비스에 연결할 수 있도록 도와주는 서비스
  - NAT 게이트웨이는 private subnet에서 외부로 요청하는 아웃바운드 트래픽을 받아 인터넷 게이트웨이와 연결
- 생성 시 public subnet 지정
- 인터넷 게이트웨이와 다르게 외부에서 접근 불가
- private subnet에서 외부 인터넷 접근 시 nat gateway의 공인 IP를 source ip로 설정됨


