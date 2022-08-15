# PaaS-TA 전문가 교육 정리

## 1. IaaS vs PaaS
- IaaS(Infrastructure as a Service)
    + CPU나 하드웨어 등 컴퓨팅 리소스(자원)를 네트워크를 통해 서비스로 제공하는 모델
    + 물리적 리소스를 가상화하여 유연한 Infrastructure 제공
    + 운영체제와 같은 물리적 환경에 가까운 개념
- PaaS(Platform as a Service)
    + 기업의 애플리케이션 실행 환경 및 개발 환경을 서비스로써 제공하는 모델
    + 애플리케이션 실행 환경이나 데이터베이스 등 미리 마련<br>→ 단기간에 애플리케이션 개발하여 서비스 제공 가능
    + 개발에서 배포까지 라이프사이클이 짧음

<br/>

## 2. Bosh 구성
- 컴포넌트 → Bosh Director에 의해 관리됨

  |요소|설명|비고|
  |:---:|:---|:---|
  |CLI|Director와 상호작용을 위한 CLI||
  |Director|VM 생성/수정 시 설정 정보 레지스트리에 저장|현재 Database에서 관리|
  |NATS|컴포넌트간 통신을 위한 메시지 채널||
  |Registry|VM 생성을 위한 설정정보 저장|현재 사장된 기술|
  |Health Monitor|BOSH Agent로부터 클라우드의 상태 정보 수집||
  |blobstore|Release, compilation Package data 저장소||
  |UAA|Bosh 사용자 인증 인가 처리||
  |Database|• Director가 사용하는 데이터베이스<br>• Deployment 시 필요로 하는 Stemcell / Release / Deployment 메타 정보 저장||
  |Agent|VM에 설치되어 Director로부터 명령을 받아 개별 작업 수행||
  |DNS|배포된 VM의 DNS Resolution||
- 구성요소 → 배포 시 필수 요소

  |요소|설명|
  |:---:|:---|
  |Release|• application deploy에 필요한 요소들을 패키지화<br>• 설치 패키지, 설정 템플릿, 스크립트|
  |Stemcell|• 버전이 지정 된 기본 운영 시스템 이미지<br>• OS 이미지, 에이전트|
  |Manifest|• 배포를 위한 선언적 정보(설정 파일)<br>• 릴리즈, 스템셀, 네트워크, 속성|

<br/>

## 3. CF cli
- start vs restart vs restage
    + start : 종료한 app 기동
      ```shell
      $ cf start joy-board
      Starting app joy-board in org joy-org / space joy-space as admin...
      
      Waiting for app to start...
      
      ...(생략)...
      
           state     since                  cpu    memory       disk           details
      #0   running   2021-10-31T09:45:16Z   0.0%   176M of 1G   155.9M of 1G
      
      # app 상태 확인
      $ cf apps
      Getting apps in org joy-org / space joy-space as admin...
      OK
      
      name        requested state   instances   memory   disk   urls
      joy-board   started           1/1         1G       1G     joy-board.10.200.0.10.nip.io
      ```
    + restart : app이 기동되는 상태에서 재기동
      ```shell
      $ cf restart joy-board
      Restarting app joy-board in org joy-org / space joy-space as admin...
      
      Stopping app...
      
      Waiting for app to start...
      
      ...(생략)...
      
           state     since                  cpu    memory       disk           detail
      #0   running   2021-10-31T09:52:07Z   0.4%   235M of 1G   155.9M of 1G
      ```
    + restage : 이미 push한 app 변경 시(환경변수 설정 또는 서비스 바인딩)
      ```shell
      # spring-music-pinpoint app에 pinpoint_monitoring_service 바인드
      $ cf bind-service spring-music-pinpoint pinpoint_monitoring_service
      Binding service pinpoint_monitoring_service to app spring-music-pinpoint in org joy-org / space joy-space as admin...
      OK
      
      TIP: Use 'cf restage spring-music-pinpoint' to ensure your env variable changes take effect
      
      
      $ cf restage spring-music-pinpoint
      Restaging app spring-music-pinpoint in org org / space space as admin...
      Downloading binary_buildpack...
      Downloading go_buildpack...
      Downloading staticfile_buildpack...
      
      ...(생략)...
      
      App started
      
      
      OK
      ```

<br/>

## 4. Service Package
- Service API
    + Cloud Controller와 Service Broker 사이의 규약 정의
    + 하나 이상의 Service가 하나의 Broker에 의해 제공 될 수 있음
    + 로드 밸런싱이 가능하게 수평 확장성 있게 제공 될 수 있음
        - 로드 밸런싱 : 서버 과부하 방지를 위해 한 곳의 엔드포인트로 들어오는 트래픽을 각 인스턴스에 분산
        - 확장
            + 수직 확장 → 서버 하나의 용량을 늘림
            + 수평 확장 → 여러 서버에 분산


- Provision API
    + Broker가 Cloud Controller로 부터 provision 요구 수신 → 새로운 서비스 인스턴스 생성
    + non-data 서비스인 경우의 provision은 기존 시스템에 계정을 얻는 의미<br>
    ex) 기존 서버에서 계정을 얻어와 접속하는 경우
    + HTTP 메소드가 `PUT`인 이유<br>
    → 생성 요청한 인스턴스 ID의 존재 여부를 체크하기 때문
