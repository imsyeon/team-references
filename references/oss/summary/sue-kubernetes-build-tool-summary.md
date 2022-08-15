# Kubernetes 설치 도구

## 쿠버네티스 구성 방법

- 관리형 쿠버네티스
  + 퍼블릭 클라우드 업체에서 제공
  + 종류
    - EKS(Amazon Elastic Kubernetes Service)
    - AKS(Azure Kubernetes Service)
    - GKE(Google Kubernetes Service)
  + 구성이 다 갖춰져 있으며, 마스터 노드를 클라우드 업체에서 제공함
  + 학습용으로는 부적절

- 설치형 쿠버네티스
  + 오픈소스 프로젝트에 뿌리를 둔 상업용 소프트웨어
  + 종류
    - Red Hat OpenShift

- 구성형 쿠버네티스
  + 사용하는 시스템에 쿠버네티스 클러스터를 자동으로 구성해 주는 솔루션을 사용
  + 종류
    - kubeadm
    - kops(Kubernetes operations)
    - kubespray
    - Rancher

## 구성형 쿠버네티스

### kubeadm

- 쿠버네티스에서 제공하는 기본적인 설치 도구
- 빠르게 쿠버네티스 클러스터 생성이 가능함
- kubeadm init 및 kubeadm join 을 제공하도록 만들어진 도구
- Kubeadm은 자체 호스팅 레이아웃, 동적 검색 서비스 등을 포함하여 Kubernetes 클러스터의 수명 주기 관리에 대한 도메인 지식을 제공
- 레퍼런스가 많음

### kops(Kubernetes operations)

- AWS나 Digital Ocean 등의 클라우드 환경에서 쿠버네티스 환경 구축 시 사용
- 각 클라우드의 고유한 특징이 반영되어 있음
  + aws : S3, Route53 등의 연동 기능 등

### kubespray

- Kubespray는 Ansible 플레이북, 인벤토리, 프로비저닝 도구와 일반적인 운영체제, 쿠버네티스 클러스터의 설정 관리 작업에 대한 도메인 지식의 결합으로 만들어짐
- 대부분의 인기있는 리눅스 배포판들을 지원
- 대부분의 인프라 환경 모두 지원
- 매우 빠르고 쉽다는 장점을 가지고 있음
- Kubespray는 kubeadm수명 주기 관리 도메인 지식을 사용


