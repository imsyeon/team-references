# IAM (Identity and Access Management)

- AWS 리소스에 대한 액세스를 안전하게 제어할 수 있는 웹 서비스
- IAM을 사용하여 리소스를 사용하도록 인증 및 권한 부여된 대상을 제어

> 리소스(Resource): AWS에서는 컴퓨팅, 스토리지, 네트워크 등을 의미함

> 액세스(Access): 리소스를 조작 및 제어 CRUD(쓰기, 읽기, 변경, 삭제)

## aws 사용자 유형

- 루트 사용자
  + 최초로 AWS 계정 생성 시 전체 AWS 서비스 및 계정 리소스에 대해 완전한 액세스 권한을 지닌 **단일 로그인 자격 증명**(루트 사용자)으로 시작
  + 모든 리소스에 대한 모든 액세스가 가능

- IAM 사용자
  + 각 사용자 별로 AWS 서비스 또는 리소스 액세스에 제한을 둘 수 있음

> 일상적인 작업은 물론 관리 작업의 경우에도 루트 사용자를 사용하지 않는 것을 강력히 권장

## IAM Role

- IAM 역할(Role)은 신뢰하는 개체에 권한(Permission)을 부여하는 안전한 방법
- 역할(Role)은 하나 이상의 정책(Policy)을 기반으로 구성됨

- Role 사용이 가능한 주체들
  + 동일한 AWS 계정의 IAM 사용자
  + 역할과 다른 AWS 계정의 IAM 사용자
  + Amazon Elastic Compute Cloud(Amazon EC2)와 같은 AWS가 제공하는 웹 서비스
  + SAML 2.0, OpenID Connect 또는 사용자 지정 구축 자격 증명 브로커와 호환되는 외부 자격 증명 공급자(IdP) 서비스에 의해 인증된 외부 사용자

## IAM Policy

- IAM에서 자격 증명에 대한 권한 설정 시 사용
- Role 설정 시 원하는 권한을 정책으로 연결해서 사용

- 정책 타입 (6가지로 분류)
  + 자격 증명 기반 (Identity-based policies)
    - AWS 관리형 정책 - AWS에서 제공하는 글로벌 적용
    - AWS 고객 관리형 정책 - 고객의 계정에서 생성해서 사용관리
    - AWS 인라인 정책 - 단일 사용자, 그룹, 역할(Role)에 직접 추가하는 방식
  + 리소스 정책 기반 (Resource-based policies)
    - 리소스(S3 Bucket Policy, SQS Queue Policy, VPC Endpoint Policy ... 등에서 사용)에 연결하는 정책
  + 권한 경계 기반 정책 (Permissions boundaries)
  + 조직 SCP 기반 정책 (Organizations SCPs)
  + 액세스 제어 리스트 (Access control lists -ACLs)
  + 세션 정책 (Session policies)

