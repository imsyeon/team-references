# ELK Stack install GUIDE
- 데이터 수집 및 분석에 쓰이는 오픈소스 프로젝트
- Elasticsearch, Logstash, Kibana, Beats를 포함

<br />

## 1. 종류
> - docker
> - apt 패키지 관리자
> - tar file

이 중 tar file을 이용하여 시스템에 직접 설치하는 방법으로 진행

<br />

## 2. 필요 환경
- [OpenJDK 11](kevin-open-jdk-install-guide.md) (JAVA_HOME 세팅 필요)
- Ubuntu 18.04

<br />

## 3. 설치
### 3.1 elasticsearch
- DB 저장 및 검색 기능을 담당
- Index를 지정하여 저장(DB의 table역할)
- url query를 이용해 검색 가능

1. elasticsearch 다운로드
   ```shell
   $ wget -P ${workspace} https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.15.0-linux-x86_64.tar.gz
   $ wget -P ${workspace} https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.15.0-linux-x86_64.tar.gz.sha512
   
   $ cd ${workspace}
   $ shasum -a 512 -c elasticsearch-7.15.0-linux-x86_64.tar.gz.sha512
   ```

2. elasticsearch tar.gz 압축해제
   ```shell
   $ tar -xzf ${workspace}/elasticsearch-7.15.0-linux-x86_64.tar.gz
   ```

3. elasticsearch 설정 변경
   ```shell
   $ ${workspace}/elasticsearch-7.15.0/config/elasticsearch.yml
   
   # 포트변경
   http.port: 8085
   
   # 외부접속 설정
   network.host: 0.0.0.0
   cluster.initial_master_nodes: ["node-1", "node-2"]
   ```

4. elasticsearch 실행
   ```shell
   $ ${workspace}/bin/elasticsearch -d
   
   # 웹으로 접속 ${VM IP}:${설정한 포트}
   # VM에서 접속 curl localhost:${설정한 포트}
   {
      "name" : "c3-inception-04",
      "cluster_name" : "elasticsearch",
      "cluster_uuid" : "R3t_lIQiQUeoJeAybhyWUQ",
      "version" : {
        "number" : "7.15.0",
        "build_flavor" : "default",
        "build_type" : "tar",
        "build_hash" : "79d65f6e357953a5b3cbcc5e2c7c21073d89aa29",
        "build_date" : "2021-09-16T03:05:29.143308416Z",
        "build_snapshot" : false,
        "lucene_version" : "8.9.0",
        "minimum_wire_compatibility_version" : "6.8.0",
        "minimum_index_compatibility_version" : "6.0.0-beta1"
      },
      "tagline" : "You Know, for Search"
   }
   
   # 인덱스 목록 검색
   http://${VM IP}:8085/_cat/indices
   ```
<br />

### 3.2 kibana
- Elasticsearch에 저장된 데이터를 UI로 표출
- 집계 기능을 이용해 다양한 차트 생성
- 다양한 차트를 한번에 볼 수 있는 대시보드 커스텀 가능

1. kibana 다운로드
   ```shell
   $ cd ${workspace}
   $ curl -O https://artifacts.elastic.co/downloads/kibana/kibana-7.15.0-linux-x86_64.tar.gz
   $ curl https://artifacts.elastic.co/downloads/kibana/kibana-7.15.0-linux-x86_64.tar.gz.sha512 | shasum -a 512 -c -
   ```

2. kibana 압축 해제
   ```shell
   $ tar -xzf ${workspace}/kibana-7.15.0-linux-x86_64.tar.gz
   ```

3. kibana 설정 변경
   ```shell
   $ vi ${workspace}/kibana-7.15.0-linux-x86_64/config/kibana.yml
   
   # 포트변경
   server.port: 8084
   
   # 외부접속 설정
   server.host: "0.0.0.0"
   
   elasticsearch.hosts: ["http://localhost:8085"]
   ```

4. kibana 실행
   ```shell
   $ ${workspace}/kibana-7.15.0-linux-x86_64/bin/kibana
   
   # 웹으로 접속 ${VM IP}:${설정한 포트}
   ```
<br />

### 3.3 logstash
- 다양한 데이터 저장소로부터 데이터를 수집
- output 형식을 이용해 다양한 경로로 데이터 전송 및 반환 데이터 포멧 지정

1. logstash 다운로드
   ```shell
   $ cd ${workspace}
   $ curl -O https://artifacts.elastic.co/downloads/logstash/logstash-7.15.0-linux-x86_64.tar.gz
   $ curl https://artifacts.elastic.co/downloads/logstash/logstash-7.15.0-linux-x86_64.tar.gz.sha512 | shasum -a 512 -c -
   ```
2. logstash 압축해제 
   ```shell
   $ tar -xzf logstash-7.15.0-linux-x86_64.tar.gz
   ```

3. logstash sample config 설정
   - beates로 수집한 정보를 input
   - 원하는 형식에 맞춰 elasticsearch에 output 저장 
   ```shell
   $ vi ${workspace}/logstash-7.15.0/config/logstash-sample.conf
   
   # Sample Logstash configuration for creating a simple
   # Beats -> Logstash -> Elasticsearch pipeline.
   input { 
     beats {
       port => 5044 #filebeat의 기본 포트
     }
   }

   output {
     elasticsearch {
       hosts => ["http://localhost:8085"]
       index => "%{[@metadata][beat]}-%{[@metadata][version]}-%+YYYY.MM.dd}"
       #user => "elastic"
       #password => "changeme"
     }
   }
   ```

4. logstash 실행
   ```shell
   # 메시지 입력하면 log형식으로 반환 (터미널에서 테스트, logstash 실행만 테스트함)
   $ ${workspace}/logstash-7.15.0/bin/logstash -e 'input { stdin { } } output { stdout {} }'
   
   # sample config 적용하여 테스트(elasticsearch에 저장하기 위한 테스트)
   $ ${workspace}/logstash-7.15.0/bin/logstash -f ${workspace}/logstash-7.15.0/config/logstash-sample.conf
   ```
<br />

### 3.4 filebeat
- logstash의 오버헤드를 줄이기 위해 도입
- 데이터를 수집하여 logstash로 전송

1. filebeat 다운로드
   ```shell
   $ wget -P ${workspace} https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.15.0-linux-x86_64.tar.gz
   ```

2. filebeat 압축 해제
   ```shell
   $ tar -xzf ${workspace}/filebeat-7.15.0-linux-x86_64.tar.gz
   ```

3. filebeat 환경설정
   - filebeat.inputs > type: file > enabled: true
   - filebeat.inputs > type: file > paths: 부분에 모니터링할 로그파일 지정
     ```shell
     $ vi ${workspace}/filebeat-7.15.0-linux-x86_64/filebeat.yml
     # ============================== Filebeat inputs ===============================
   
     filebeat.inputs:
   
     # Each - is an input. Most options can be set at the input level, so
     # you can use different inputs for various configurations.
     # Below are the input specific configurations.

       # filestream is an input for collecting log messages from files. It is going to replace log input in the future.
     - type: filestream
   
       # Change to true to enable this input configuration.
       enabled: true
   
       # Paths that should be crawled and fetched. Glob based paths.
       paths:
         - /var/log/*.log
   
       ```
   - output.elasticsearch > hosts에 실행되고 있는 elasticsearch ip:port 지정
     ```shell
     $ vi ${workspace}/filebeat-7.15.0-linux-x86_64/filebeat.yml
     # ---------------------------- Elasticsearch Output ----------------------------
     output.elasticsearch:
       # Array of hosts to connect to.
             hosts: ["localhost:8085"]
     ```

4. filebeat 실행
   - sudo 권한 실행이유 -> root 권한이 필요한 log를 모니터링 하기위함
     ```shell
     $ sudo ${workspace}/filebeat-7.15.0-linux-x86_64/filebeat -e -c filebeat.yml
    
     # elasticsearch의 filebeat인덱스에 로그가 저장되는지 확인
     http://${VM IP}:{elasticsearch port}/filebeat*/_search
     http://10.200.33.124:8085/filebeat*/_search
    
     ```
     > error) Exiting: error loading config file: config file ("filebeat.yml") must be owned by the beat user (uid=0) or root

   - sudo 권한으로 filebeat 실행 시 해당오류 발생(소유자 변경 필요)
   - root 권한이 필요한 log 파일을 임의의 사용자가 지정하는것을 방지(소유자만 쓰기 가능)
   - 조치방법
      ```shell
      # 소유자 변경
      $ sudo chown root ${workspace}/filebeat-7.15.0-linux-x86_64/filebeat.yml
      # 소유자만 쓰기가능
      $ sudo chmod go-w ${workspace}/filebeat-7.15.0-linux-x86_64/filebeat.yml
      ```
<br />

### 3.5 ELK stack 연동 테스트
1. kibana 페이지 접속
    ```shell
    ${VM IP}:{kibana port}
    http://10.200.33.124:8084/
    ```

2. 메뉴(三) > Management > Stack Management

3. 좌측메뉴 Management > kibana > Index Patterns

4. Create index pattern 버튼 클릭

5. Name 영역에 filebeat* 입력(filebeat로 시작하는 인덱스를 가져옴)

6. Timestamp field > @timestamp 지정 > 아래의 create index pattern버튼 클릭

7. 메뉴(三) > Analytics > Discover > flebeat* index pattern 선택

8. 좌측 Available fields의 항목을 선택하면 Selected fields로 이동하여 해당 항목만 테이블에 표시됨
   - agent.id
   - log.file.path
   - host.ip
   - message

