# Concept 설명 요약

## 1. CVE (Common Vulnerabilities and Exposure)
+ 공개적으로 알려진 소프트웨어의 보안 취약점을 가리키는 고유 표기를 뜻함
+ MITRE가 소프트웨어와 펌웨어의 취약점들을 파악하고 분류
+ 공격자가 시스템이나 네트워크에 직접 접속할 수 있도록 만드는 소프트웨어 코드의 오류
+ 정보 보안 취약점 표준 코드
+ CVE번호는 항목 또는 후보 상태가 있음
    - 항목 상태는 CVE 리스트에 CVE 식별 번호가 포함되는 것을 수락한 상태
    - 후보 상태는 리스트에 포함되기 위해 검토를 하고 있는 상태
+ CVE 번호 규칙 : `CVE-{해당년도}-{취약점 번호}`
    - 과거 : `CVE-YYYY-NNNN` ex) CVE-2014-0000
    - 현재 : `CVE-YYYY-NNNN...N` ex) CVE-2014-12345

<br />

## 2. CCE (Common Configuration Enumeration)
+ 사용자에게 허용된 권한 이상의 동작을 허용하거나, 범위 이상의 정보 열람, 변조, 유출을 가능하게 하는 시스템 설정 상의 취약점
+ 정보시스템의 설정 값을 통하여 진단
    - 서버, 네트워크, DBMS, WEB/WAS, PC 등

<br />

## 3. agile
+ 짧은 주기의 개발단위를 반복하여 하나의 큰 프로젝트를 완성해 나가는 방식
     - 앞을 예측하여 개발하는 것이 아닌 일정한 주기를 가지고 검토해 필요할 때 마다 수정하여 살을 붙이는 방식
     - 기능이 실제로 구동되는 모습을 고객사에서 확인 후 다음 단계로 넘어 감
     - 고객의 요구 변화에 유연하고도 신속하게 대응
+ Less Document-Oriented, 문서를 통한 개발이 아닌 Code-Oriented, 실질적인 코딩을 통한 방법론
+ 애자일의 핵심은 협력과 피드백
+ **유연하게 일을 진행하고, 변화에 잘 대응하는 것이 핵심**
+ 애자일의 핵심은 협력과 피드백
+ 방대한 문서보다 실행되는 SW를 중시
+ 스크럼, XP, 칸반, Lean, 기능 중심 개발 등

<br />

## 4. waterfall
+ 단계별 순차적인 진행을 강조하는 방법론
+ 오랜 기간 사용된 기법으로 적용사례가 많음
+ 단계별로 정형화된 접근방식을 사용해 기술적인 위험 요소가 적음
+ 소프트웨어 개발의 모든 단계를 확실히 매듭짓고 넘어가며, 완료된 단계는 다시 되돌리기 어려운 특성을 가진 개발 방법론
+ 개발 주기 : 요구사항 분석 → 설계 → 구현 → 검증(테스트) → 유지보수
+ 단점
    - 요구사항 분석과 타당성 검토에 많은 시간 소요
    - 한 과정이 끝나면 중간에 요구 사항 추가 및 변경이 어려움
    - 단계별 발생한 오류들에 대한 즉각적인 피드백을 하기 어려움
    - 개발이 완료되기 전까지 고객사의 요구사항을 100% 담아내기 힘듦
    - 목표 시스템이 후반에 가서야 구체화 되므로 중요한 문제점이 프로젝트 후반부에 발견

<br />

## 5. MSA (MicroService Architecture)
+ 전통적인 모놀리식(monolithic)의 단점을 보완하여 애플리케이션을 핵심 기능으로 세분화 하는 MicroService(MS)라는 아키텍처 기반의 접근 방식이 탄생함
+ API를 통해서만 상호작용
    - 서비스의 end-point(접근점)을 API 형태로 외부에 노출
    - 실질적인 세부 사항은 모두 추상화
+ 다른 서비스들과 유연하게 결합
    - 언어의 제약이 없음 
+ 향후 확장 및 새로운 기능 통합 등에 대비할 수 있음
    - 높은 확장성
+ 분산형 개발을 통해 효율적인 개발 가능
+ 개별 서비스가 다른 서비스에 부정적인 영향을 주지 않으면서 작동
+ 각 서비스는 독립된 서버로 타 컴포넌트와 의존성이 없기 때문에 독립된 배포
+ 서비스 확장 및 유지보수 용이

<br />

## 6. DevOps
+ 개발과 운영을 하나의 조직으로 합쳐서 팀을 운영하는 문화이자 방법론
    - Development와 Operations의 합성어
    - 기존의 개발 업무와 관리 업무로 나누어진 두 역할 사이의 커뮤니케이션, 협업, 통합을 강조
+ 기술을 통해 프로세스를 자동화 및 최적화할 수 있음
+ 빠르게 변화하는 비즈니스 환경과 사용자 요구에 빠른 속도로 대처 가능
+ DevOps는 CALMS라고 해서, 크게 5가지 핵심 요소로 나누어짐
    - 문화(Culture)
    - 자동화(Automation)
    - 간소화(Lean)
    - 측정(Measurement)
    - 공유(Share)
+ 구성원에게 개발 책임감과 코드의 소유권을 높여주고 개발 프로세스 간소화
+ IT 운영과 관련된 서비스의 자동화가 핵심
+ 애플리케이션과 서비스를 빠른 속도로 제공할 수 있도록 조직의 역량을 향상시키는 문화 철학, 방식 및 도구의 조합
+ 개발, 테스트, 배포를 모두 자동화 시켜 개발 사이클이 끊임없이 순환되도록 함
<br>→ 개발 속도 최대화

<br />

## 7. DevSecOps
+ 개발, 보안, 운영을 뜻함
+ 소프트웨어 보안을 전체 소프트웨어 전달 프로세스의 핵심 부분으로 만드는 개념
+ DevSecOps는 DevOps를 수용하고 전체 CI/CD 개발 파이프라인에 보안을 통합하는 것을 의미
    - 개발자는 코드 작성 시 보안에 대해 생각을 하게 됨
    - 소프트웨어는 배포되기 전 보안 문제 테스트를 받게 됨
    - IT팀은 배포 후 나타나는 보안 문제를 신속하게 해결하기 위한 계획 수립 가능
+ 소프트웨어 개발 주기의 모든 단계에서 자동으로 보안 구현
+ 소프트웨어 개발 주기의 마지막 단계가 아닌, 초기 설계 과정의 각 단계에서 보안 솔루션을 통합
+ 개발 사이클 초기에 보안정책을 설계하여 최소한의 비용으로 취약점 보완

<br />

## 8. TDD (Test Driven Development)
+ 테스트 주도 개발
    - 반복 테스트를 이용한 소프트웨어 방법론
+ 작은 단위의 테스트 케이스를 작성 후 이를 통과하는 코드를 추가하는 단계를 반복하여 구현
    - 테스트 코드를 작성한 뒤에 실제 코드를 작성
    - 예외 사항들을 거쳐 테스트가 통과된 코드만을 개발 단계에서 실제 코드로 작성
    - 코드의 버그가 줄어들고 소스코드는 간결해짐
+ 지속적으로 프로토타입을 완성하는 애자일 방법론 중 하나
+ 테스트 케이스 작성 → 테스트 케이스를 통과하는 코드 작성 → 작성한 코드 리팩토링 반복
+ 테스팅을 자동화 시킴과 동시에 보다 정확한 테스트 근거를 산출
+ 피드백과 협력 증진 → 불확실성에 대한 대비 가능
+ TDD의 대표적인 Tool 'JUnit'

<br />

## 9. IP (Internet Protocol)
+ 인터넷에 연결되어 있는 모든 장치들을 식별할 수 있도록 각각의 장비에게 부여되는 고유 주소
+ 인터넷에서 컴퓨터의 위치를 찾아서 데이터를 전송하기 위해 지켜야 할 규약
+ 네트워크 부분 + 호스트 부분으로 구성
+ 방식
    |방식|내용|
    |:---:|:---|
    |공인 IP|ICANN, 인터넷 진흥원(KISA)등의 IP 주소 할당 공인기관에서 할당한 인터넷 상에서 사용할 수 있는 IP주소|
    |사설 IP|내부 네트워크 상에서만 사용되는 주소로 인터넷상에서는 사용할 수 없는 IP주소|
    |고정 IP|ISP에 의해 해당 사용자 전용으로 부여되는 인터넷 주소<br/> 컴퓨터에 고정적으로 부여된 IP로 한번 부여되면 IP를 반납하기 전까지는 다른 장비에 부여할 수 없는 IP 주소|

+ 유형
    | |IPv4|IPv6|
    |:---:|:---|:---|
    |설명|· IP version 4의 약자<br>· 전 세계적으로 사용된 첫 번째 인터넷 프로토콜|· IPv4 고갈 문제를 해결하기 위해 등장한 주소<br>· IPv4의 주소체계를 128비트 크기로 확장한<br>&nbsp;&nbsp;차세대 인터넷 프로토콜 주소|
    |주소<br>체계|32bit|128bit|
    |표현|8비트씩 끊어 0\~255의 10진수 숫자로 나타냄|4자리의 16진수 숫자 8개로 표현|
    |구분|점(.)|쌍점(:)|
    |예시|93.184.216.34|2606:2800:0220:0001:0248:1893:25C8:1946|

<br />

## 10. subnet mask
+ IP주소에 마스크를 씌워서 어디까지가 네트워크 부분인지 표시
    |IP Address|Subnet mask|
    |:---:|:---:|
    |192.168.32.0|255.255.255.0|
    |11000000.10101000.00100000.00000000|	11111111.11111111.11111111.00000000|
    - 서브넷 마스크에서 1로 표시된 부분 → Network ID
    - 서브넷 마스크에서 0으로 표시된 부분 → Host ID
        + Host ID를 가지고 서브넷팅을 하게 됨
+ 브로드캐스트 도메인(Broadcast Domain)을 줄여서 트래픽 감소
+ 보안이 필요한 내용들을 보호해야할 때, 서브네트워크를 통해 별도로 관리
+ ex) IP주소 192.168.1.1에 서브넷마스크가 255.255.255.0인 경우
    - 255.255.255로 표시된 부분인 192.168.1. 까지는 네트워크 부분
    - 0으로 표시된 부분인 .1은 호스트 부분
    - 255는 이진법으로 표시하면 11111111이기 때문에 네트워크 부분
+ 주어진 IP 주소를 네트워크 환경에 맞게 나누어 주기 위해 사용되는 이진수의 조합
+ 필요한 네트워크 주소만 호스트 IP로 할당 할 수 있게 만들어 네트워크 낭비 방지
+ 네트워크 영역에 마스크(AND 연산자)를 씌워 계산

<br />

## 11. CIDR(Classless inter-Domain Routing)
+ 연속된 IP 주소의 범위를 표기하는 방법 중 하나
+ 기존 IP 주소 할당 방식이었던 클래스를 대체하며 IP 주소의 네트워크 영역, 호스트 영역을 유연하게 나누어 줌
+ 서브넷 마스크의 bit 수를 의미
+ 슬래시(/) 뒤에 몇 비트가 접두어인지 표시
  > ex) 192.168.0.0/**16**<br>
  - 앞의 16비트(192.168)가 접두어<br>
  - 주소 범위 : 192.168.0.0 ~ 192.168.255.255

<br />

## 12. router
+ 네트워크와 네트워크 간의 경로(Route)를 설정하고 가장 빠른 길로 트래픽을 이끌어주는 네트워크 장비
+ NAT(Network Address Translation), 방화벽, VPN(Virtual Private Network), QoS(Quality of Service)등 다양한 부가 기능을 함께 제공
+ 임의의 외부 네트워크(WAN)와 내부 네트워크(LAN)를 연결해주는 장비
+ 패킷의 위치를 추출하여 그 위치에 대한 최적의 경로를 지정 후, 이 경로를 따라 데이터 패킷을 다음 장치로 전향시키는 장치
+ 한 개의 인터넷 회선을 여러 대가 사용 가능하도록 쪼개주는 기능 수행

<br />

## 13. gateway
+ 현재 사용자가 위치한 네트워크에서 다른 네트워크로 이동하기 위해 반드시 거쳐야 하는 거점을 의미함
+ 컴퓨터 네트워크에서 서로 다른 통신망, 프로토콜을 사용하는 네트워크 간의 통신을 가능하게 하는 컴퓨터나 소프트웨어
+ 포로토콜이 다른 네트워크 상의 컴퓨터와 통신할 때 두 프로토콜을 적절히 변환해주는 변환기 역할
+ router보다 포괄적인 개념
+ OSI 7계층 중 모든 곳에서 동작 가능
<br>→ 전송방식이 다른 통신망도 흡수하여 서로 다른 기종끼리 접속 가능하게 함