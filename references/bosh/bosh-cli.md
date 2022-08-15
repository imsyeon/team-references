# BOSH CLI

## 1. 기본 사용법

+ 기본 Syntax

    ```shell
    $ bosh [<options>] <command> [<args>]
    ```

<br />

## 2. bosh environment
+ Director 정보를 출력
    ```shell
    $ bosh -e [my-env] environment (Alias: env)
    ```

+ 사용 예시 :: micro-bosh Director 정보를 출력
    ```shell
    $ bosh env -e micro-bosh
        Using environment '10.200.41.6' as client 'admin'

        Name               micro-bosh  
        UUID               74cf8940-6ce9-4666-bcbd-df01025c097a  
        Version            271.11.0 (00000000)  
        Director Stemcell  ubuntu-bionic/1.36  
        CPI                openstack_cpi  
        Features           compiled_package_cache: disabled  
                        config_server: enabled  
                        local_dns: enabled  
                        power_dns: disabled  
                        snapshots: disabled  
        User               admin  

        Succeeded
    ```

<br />

## 3. bosh create-env
+ Manifest를 기반으로 단일의 VM을 생성
+ Director 환경을 만드는 데 사용
    ```shell
    $  bosh create-env [deploymentFile] [--state path] [-v ...] [-o ...] [--vars-store path]
    ```

+ 사용 예시
    ```shell
    $ bosh create-env ~/workspace/bosh-deployment/bosh.yml \
      --state state.json \
      --vars-store ./creds.yml \
      -o ~/workspace/bosh-deployment/virtualbox/cpi.yml \
      -o ~/workspace/bosh-deployment/virtualbox/outbound-network.yml \
      -o ~/workspace/bosh-deployment/bosh-lite.yml \
      -o ~/workspace/bosh-deployment/jumpbox-user.yml \
      -v director_name=vbox \
      -v internal_ip=192.168.56.6 \
      -v internal_gw=192.168.56.1 \
      -v internal_cidr=192.168.56.0/24 \
      -v network_name=vboxnet0 \
      -v outbound_network_name=NatNetwork
    ```

<br />

## 4. bosh delete-env
+ Manifest를 기반으로 생성된 VM을 삭제 시 사용
+ create-env명령에 제공된 동일한 플래그를 명령어로 입력해야 함
    ```shell
    $ bosh delete-env [deploymentFile] [--state path] [-v ...] [-o ...] [--vars-store path]
    ```
+ 사용 예시 
    ```shell
    $ bosh delete-env ~/workspace/bosh-deployment/bosh.yml \
      --state state.json \
      --vars-store ./creds.yml \
      -o ~/workspace/bosh-deployment/virtualbox/cpi.yml \
      -o ~/workspace/bosh-deployment/virtualbox/outbound-network.yml \
      -o ~/workspace/bosh-deployment/bosh-lite.yml \
      -o ~/workspace/bosh-deployment/jumpbox-user.yml \
      -v director_name=vbox \
      -v internal_ip=192.168.56.6 \
      -v internal_gw=192.168.56.1 \
      -v internal_cidr=192.168.56.0/24 \
      -v network_name=vboxnet0 \
      -v outbound_network_name=NatNetwork
    ```

<br />

## 5. bosh int(interpolate)
- 변수를 Manifest에 추가하여 결과값을 stdout으로 보냄
- 배포하기 전 Manifest를 수정하고 작성하기 위한 작업 파일 및 변수를 제공
    ```shell
    $ bosh interpolate manifest.yml [-v ...] [-o ...] [--vars-store path] [--path op-path] (Alias: int)
    ```

+ 사용 예시 :: creds.yml 파일에서 /director_ssl/ca에 해당하는 내용을 출력
    ```shell
    $ bosh int ~/workspace/paasta-5.5.4/deployment/paasta-deployment/bosh/openstack/creds.yml --path /director_ssl/ca
    ```

<br />

## 6. bosh alias-env
+ CLI 명령에 쉽게 액세스할 수 있도록 생성된 환경에 이름을 할당
    ```shell
    $ bosh alias-env [name] -e [location] [--ca-cert=path] 
    ```
+ 사용 예시 :: 10.0.1.6에 해당하는 디렉터의 명칭을 micro-bosh로 변경하며 CA인증서를 지정
    ```shell
    $ bosh alias-env micro-bosh -e 10.0.1.6 --ca-cert <(bosh int ~/workspace/paasta-5.5.4/deployment/paasta-deployment/bosh/openstack/creds.yml --path /director_ssl/ca)
    ```

<br />

## 7. bosh log-in
+ 주어진 사용자를 Director에 로그인
    ```shell
    $ bosh -e [my-env] log-in
    ```
+ 사용 예시 :: micro-bosh Director에 로그인
    ```shell
    $ bosh -e micro-bosh log-in
    User (): admin
    Password ():
    ```

<br />

## 8. bosh log-out
+ 주어진 사용자를 Director에서 로그아웃
    ```shell
    $ bosh -e [my-env] log-out
    ```
+ 사용 예시 :: test-env Director에서 로그아웃
    ```shell
    $ bosh -e test-env log-out 
    Logged out from '192.168.10.241'

    Succeeded
    ```

<br />

## 9. bosh stemcells
+ Director에 업로드 된 stemcell 조회
    ```shell
    $ bosh -e [my-env] stemcells (Alias: ss)
    ```
+ 사용 예시 :: test-env Director에 업로드 된 stemcell 조회
    ```shell
    $ bosh -e test-env ss

    Name                                       Version   OS             CPI  CID  
    bosh-openstack-kvm-ubuntu-xenial-go_agent  621.125*  ubuntu-xenial  -    6554394d-e841-4c71-9c46-cba168b37292  

    (*) Currently deployed

    1 stemcells
    ```

<br />

## 10. bosh upload-stemcell
+ Director에 stemcell 업로드
    ```shell
    $ bosh [OPTIONS] upload-stemcell [upload-stemcell-OPTIONS] URL
    ```

+ 사용 예시 :: test-env Director에 다운로드 된 stemcell 업로드
    ```shell
    $ bosh -e test-env us ~/Downloads/bosh-stemcell-3468.17-warden-boshlite-ubuntu-trusty-go_agent.tgz
    ```

<br />

## 11. bosh releases
+ Director에 업로드 된 릴리즈 조회
    ```shell
    $ bosh -e [my-env] releases (Alias: rs)
    ```

+ 사용 예시 :: micro-bosh에 업로드 된 릴리즈 조회
    ```shell
    $ bosh -e micro-bosh rs
    Using environment '10.200.1.6' as client 'admin'

    Name                   Version           Commit Hash
    binary-buildpack       1.0.37*           d26e6dd
    bosh-dns               1.29.0*           f1433e0
    bosh-dns-aliases       0.0.3*            eca9c5a  

    ...(생략)...

    staticfile-buildpack   1.5.19*           aa85282
    statsd-injector        1.11.15*          3bfafaf
    uaa                    75.1.0-PaaS-TA*   da12475+

    (*) Currently deployed
    (+) Uncommitted changes

    36 releases

    Succeeded
    ```

<br />

## 12. bosh cloud-config
+ Director의 cloud-config 정보 출력
  ```shell
  $ bosh -e [my-env] cloud-config (Alias: cc)
  ```

+ 사용 예시 :: micro-bosh의 cloud-config 정보 출력
    ```shell
    $ bosh -e micro-bosh cc
    Using environment '10.200.1.6' as client 'admin'

    azs:
    - cloud_properties:
    availability_zone: nova01
    name: z1
    - cloud_properties:
    availability_zone: nova01
    name: z2

    ...(생략)...

    - cloud_properties:
    instance_type: m1.small
    name: portal_medium
    - cloud_properties:
    instance_type: m1.small
    name: portal_large

    Succeeded
    ```

<br />

## 13. bosh update-cloud-config
+ Director의 cloud-config 구성 요소를 추가 또는 수정
    ```shell
    $ bosh -e [my-env] update-cloud-config [config.yml] [-v ...] [-o ...] (Alias: ucc)
    ```

+ 사용 예시 :: micro-bosh Director의 cloud-config 수정
    ```shell
    $ bosh -e micro-bosh update-cloud-config ~/workspace/paasta-5.5.4/deployment/paasta-deployment/cloud-config/aws-cloud-config.yml
    Using environment '10.0.1.6' as client 'admin'

    Continue? [yN]: y

    Succeeded
    ```

<br />

## 14. bosh runtime-config
+ Director의 runtime-config 구성 요소 출력
    ```shell
    $ bosh -e my-env runtime-config (Alias: rc)
    ```

+ 사용 예시 :: micro-bosh Director의 runtime-config 구성 요소 출력
    ```shell
    $ bosh -e micro-bosh rc
    Using environment '10.0.1.6' as client 'admin'

    ---
    addons:
    - include:
        deployments:
        - paasta
        - pinpoint
        - pinpoint-monitoring
        stemcell:
        - os: ubuntu-trusty
        - os: ubuntu-xenial
        - os: ubuntu-bionic

    ...(생략)...

    - name: "/dns_api_server_tls"
    options:
        ca: "/dns_api_tls_ca"
        common_name: api.bosh-dns
        extended_key_usage:
        - server_auth
    type: certificate
    - name: "/dns_api_client_tls"
    options:
        ca: "/dns_api_tls_ca"
        common_name: api.bosh-dns
        extended_key_usage:
        - client_auth
    type: certificate

    Succeeded
    ```

<br />

## 15. bosh update-runtime-config
+ Director의 runtime-config 구성 요소를 추가 또는 수정
    ```shell
    $ bosh -e my-env update-runtime-config [config.yml] [-v ...] [-o ...] (Alias: urc)
    ```

+ 사용 예시 :: my-env Director의 runtime-config 구성 요소를 수정
    ```shell
    $ bosh -e my-env urc runtime.yml
    ```

<br />

## 16. bosh deployments
+ Director가 설치한 전체 배포 목록 출력
    ```shell
    $ bosh -e [my-env] deployments (Alias: ds)
    ```
- 사용 예시 :: micro-bosh Director가 설치한 전체 배포 목록 출력
    ```shell
    $ bosh -e micro-bosh ds
    Using environment '10.200.1.6' as client 'admin'

    Name    Release(s)                    Stemcell(s)                                        Team(s)
    paasta  binary-buildpack/1.0.37       bosh-openstack-kvm-ubuntu-xenial-go_agent/621.125  -
            bosh-dns/1.29.0                                                                    
            bosh-dns-aliases/0.0.3                                                             
            bpm/1.1.9                                                                          
            capi/1.109.0-PaaS-TA                                                               

            ...(생략)...
                                                            
            ruby-buildpack/1.8.37                                                              
            silk/2.36.0-PaaS-TA                                                                
            staticfile-buildpack/1.5.19                                                        
            statsd-injector/1.11.15                                                            
            uaa/75.1.0-PaaS-TA

    1 deployments

    Succeeded
    ```

<br />

## 17. bosh deployment
- Director가 지정한 이름의 배포 목록 조회
    ```shell
    $ bosh -e [my-env] -d [my-dep] deployment (Alias: dep)
    ```

- 사용 예시 :: paasta deployment의 배포 목록 조회
    ```shell
    $ bosh -e micro-bosh -d paasta dep
    Using environment '10.200.1.6' as client 'admin'

    Name    Release(s)                    Stemcell(s)                                        Team(s)
    paasta  binary-buildpack/1.0.37       bosh-openstack-kvm-ubuntu-xenial-go_agent/621.125  -
            bosh-dns/1.29.0                                                                    
            bosh-dns-aliases/0.0.3                                                             
            bpm/1.1.9                                                                          
            capi/1.109.0-PaaS-TA                                                               

            ...(생략)...
                                                            
            ruby-buildpack/1.8.37                                                              
            silk/2.36.0-PaaS-TA                                                                
            staticfile-buildpack/1.5.19                                                        
            statsd-injector/1.11.15                                                            
            uaa/75.1.0-PaaS-TA

    1 deployments

    Succeeded
    ```

<br />

## 18. bosh deploy
+ Director가 지정한 배포명의 Manifest 파일을 통해 VM 설치
    ```shell
    $ bosh -e [my-env] -d [my-dep] deploy [manifest.yml] [-v ...] [-o ...]
    ```
+ 사용 예시 :: cf.yml 파일을 이용해 cf deployment 배포
    ```shell
    $ bosh -e vbox -d cf deploy cf.yml -v system_domain=sys.example.com -o large-footprint.yml
    ```

<br />

## 19. bosh delete-deployment
- Director가 지정한 배포명의 VM을 삭제
    ```shell
    $ bosh -e [my-env] -d [my-dep] delete-deployment [--force] (Alias: deld)
    ```

- 사용 예시 :: paasta deployment VM을 삭제
    ```shell
    $ bosh -e micro-bosh -d paasta deld
    Using environment '10.0.1.6' as client 'admin'

    Using deployment 'paasta'

    Continue? [yN]: y

    Task 90

    Task 90 | 09:27:00 | Deleting instances: uaa/4f83a1c2-fa24-4879-96cf-03679e963189 (0)
    Task 90 | 09:27:00 | Deleting instances: router/f9889cf3-df38-47c7-9f85-288806d4c393 (0)
    Task 90 | 09:27:00 | Deleting instances: api/e43daac7-5783-4fee-9d52-10712af1ebb0 (0)
    Task 90 | 09:27:00 | Deleting instances: diego-api/b35cbbd9-3a86-4144-a92e-d6848eed94e6 (0)

    ...(생략)...

    Task 90 | 09:30:32 | Removing deployment artifacts: Detaching stemcells (00:00:00)
    Task 90 | 09:30:32 | Removing deployment artifacts: Detaching releases (00:00:00)
    Task 90 | 09:30:32 | Deleting properties: Destroying deployment (00:00:00)

    Task 90 Started  Tue Oct 26 09:27:00 UTC 2021
    Task 90 Finished Tue Oct 26 09:30:32 UTC 2021
    Task 90 Duration 00:03:32
    Task 90 done

    Succeeded
    ```

<br />

## 20. bosh vms
- Director가 관리하는 vm 또는 deployment의 모든 vm 조회
    ```shell
    $ bosh -e [my-env] -d [my-dep] vms [--vitals]
    ```
- 사용에시 :: paasta deployment의 모든 vm 조회
    ```shell
    $ bosh -e micro-bosh -d paasta vms
    Using environment '10.200.1.6' as client 'admin'

    Task 65. Done

    Deployment 'paasta'

    Instance                                                  Process State  AZ  IPs           VM CID                                VM Type             Active  Stemcell
    api/1e565232-d4ec-4d58-becb-73267a68796f                  running        z1  10.200.1.126  ae7a515d-dbe7-4429-884c-8d08ec4beda5  small               true    bosh-openstack-kvm-ubuntu-xenial-go_agent/621.125
    cc-worker/a8c74d5e-1761-40ac-9548-8fe6de4a0e34            running        z1  10.200.1.127  371e5214-dada-4d2a-9b37-0514b4fcebf7  minimal             true    bosh-openstack-kvm-ubuntu-xenial-go_agent/621.125
    credhub/404f6674-54a8-4e8b-9476-564452e19096              running        z1  10.200.1.134  c54f9069-fb87-491e-83e5-7952427fcf91  minimal             true    bosh-openstack-kvm-ubuntu-xenial-go_agent/621.125

    ...(생략)...
    tcp-router/b78042a9-35cc-412a-ba64-26c2feeae0d3           running        z1  10.200.1.130  b619b81a-adae-4f0a-a747-dad550974df2  minimal             true    bosh-openstack-kvm-ubuntu-xenial-go_agent/621.125
    uaa/a37a6271-e9d4-4d59-87ed-910db3c6f27b                  running        z1  10.200.1.124  47a307e1-364a-40fa-81c9-a6e6fec5a9e3  minimal             true    bosh-openstack-kvm-ubuntu-xenial-go_agent/621.125

    15 vms

    Succeeded
    ```

- 사용 예시 :: paasta deployment의 모든 vm의 상세정보 조회
    ```shell
    $ bosh -e micro-bosh -d paasta vms --vitals
    Using environment '10.200.1.6' as client 'admin'

    Task 67. Done

    Deployment 'paasta'

    Instance                                                  Process State  AZ  IPs           VM CID                                VM Type             Active  Stemcell                                           VM Created At                 Uptime         Load              CPU    CPU    CPU   CPU   Memory        Swap         System      Ephemeral   Persistent
    (1m, 5m, 15m)     Total  User   Sys   Wait  Usage         Usage        Disk Usage  Disk Usage  Disk Usage
    api/1e565232-d4ec-4d58-becb-73267a68796f                  running        z1  10.200.1.126  ae7a515d-dbe7-4429-884c-8d08ec4beda5  small               true    bosh-openstack-kvm-ubuntu-xenial-go_agent/621.125  Mon Oct 25 09:15:44 UTC 2021  6d 1h 42m 32s  0.05, 0.06, 0.08  -      9.9%   1.5%  0.6%  42% (862 MB)  1% (15 MB)   53% (35i%)  7% (3i%)    -
    cc-worker/a8c74d5e-1761-40ac-9548-8fe6de4a0e34            running        z1  10.200.1.127  371e5214-dada-4d2a-9b37-0514b4fcebf7  minimal             true    bosh-openstack-kvm-ubuntu-xenial-go_agent/621.125  Mon Oct 25 09:15:55 UTC 2021  6d 1h 42m 24s  0.01, 0.02, 0.00  -      3.4%   0.4%  0.0%  25% (508 MB)  0% (0 B)     53% (35i%)  4% (2i%)    -
    credhub/404f6674-54a8-4e8b-9476-564452e19096              running        z1  10.200.1.134  c54f9069-fb87-491e-83e5-7952427fcf91  minimal             true    bosh-openstack-kvm-ubuntu-xenial-go_agent/621.125  Mon Oct 25 09:15:44 UTC 2021  6d 1h 42m 30s  0.00, 0.00, 0.00  -      0.9%   0.2%  0.0%  32% (644 MB)  0% (0 B)     53% (35i%)  3% (0i%)    -

    ...(생략)...

    tcp-router/b78042a9-35cc-412a-ba64-26c2feeae0d3           running        z1  10.200.1.130  b619b81a-adae-4f0a-a747-dad550974df2  minimal             true    bosh-openstack-kvm-ubuntu-xenial-go_agent/621.125  Mon Oct 25 09:15:39 UTC 2021  6d 1h 42m 33s  0.02, 0.02, 0.00  -      0.4%   0.2%  2.2%  17% (346 MB)  0% (0 B)     53% (35i%)  2% (0i%)    -
    uaa/a37a6271-e9d4-4d59-87ed-910db3c6f27b                  running        z1  10.200.1.124  47a307e1-364a-40fa-81c9-a6e6fec5a9e3  minimal             true    bosh-openstack-kvm-ubuntu-xenial-go_agent/621.125  Mon Oct 25 09:16:00 UTC 2021  6d 1h 42m 15s  0.04, 0.03, 0.00  -      3.5%   1.4%  0.3%  41% (833 MB)  0% (2.8 MB)  53% (35i%)  6% (0i%)    -

    15 vms

    Succeeded
    ```

<br />

## 21. bosh manifest
+ 디렉터가 지정한 배포명의 Manifest 파일을 출력
    ```shell
    $ bosh -e [my-env] -d [my-dep] manifest (Alias: man)
    ```

+ 사용 예시 :: paasta deployment의 Manifest 파일을 출력
    ```shell
    $ bosh -e micro-bosh -d paasta man

    route_registrar:
        routes:
        - name: doppler
            registration_interval: 20s
            server_cert_domain_san: doppler.10.200.0.10.nip.io
            tls_port: 8081
            uris:
            - doppler.10.200.0.10.nip.io
            - '*.doppler.10.200.0.10.nip.io'
        - name: rlp-gateway
            registration_interval: 20s
            server_cert_domain_san: log-stream.10.200.0.10.nip.io
            tls_port: 8088
            uris:
            - log-stream.10.200.0.10.nip.io
            - '*.log-stream.10.200.0.10.nip.io'
    release: routing
    name: log-api
    networks:
    - name: default
    stemcell: default
    vm_type: minimal
    ...
    ```

<br />

## 22. bosh ssh
+ bosh vm의 shell로 접속
    ```shell
    $ bosh -d [deployment] ssh [instance]/[index]
    ```
+ 사용 예시 :: paasta deployment의 haproxy vm의 shell로 접속
    ```shell
    $ bosh -d paasta ssh haproxy
    Using environment '10.200.41.6' as client 'admin'

    Using deployment 'paasta'

    Task 59. Done
    Unauthorized use is strictly prohibited. All access and activity
    is subject to logging and monitoring.
    Welcome to Ubuntu 18.04.6 LTS (GNU/Linux 5.4.0-89-generic x86_64)

    * Documentation:  https://help.ubuntu.com
    * Management:     https://landscape.canonical.com
    * Support:        https://ubuntu.com/advantage

    The programs included with the Ubuntu system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.

    Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
    applicable law.

    Last login: Wed Dec  8 02:14:16 2021 from 10.200.42.118
    To run a command as administrator (user "root"), use "sudo <command>".
    See "man sudo_root" for details.
    ```

<br />

## 23. bosh scp
+ bosh vm에 파일 전송
    ```shell
    $ bosh -d [deployment] scp [source] [destination]
    ```
+ 사용 예시 :: CCE_script.zip 파일을 api/0 vm의 /tmp 폴더에 업로드
    ```shell
    $ bosh -d paasta scp CCE_script.zip api/0:/tmp
    Using environment '10.0.1.6' as client 'admin'

    Using deployment 'paasta'

    Task 47. Done
    api/39606ce2-d54f-4ede-85d7-ead5021f3f7e: stderr | Unauthorized use is strictly prohibited. All access and activity
    api/39606ce2-d54f-4ede-85d7-ead5021f3f7e: stderr | is subject to logging and monitoring.

    Succeeded
    ```

+ 사용 예시 :: api/0 vm의 CCE_script.zip 파일을 현재 폴더에 다운로드
    ```shell
    $ bosh -d paasta scp api/0:/tmp/CCE_script.zip .
    Using environment '10.0.1.6' as client 'admin'

    Using deployment 'paasta'

    Task 49. Done
    api/39606ce2-d54f-4ede-85d7-ead5021f3f7e: stderr | Unauthorized use is strictly prohibited. All access and activity
    api/39606ce2-d54f-4ede-85d7-ead5021f3f7e: stderr | is subject to logging and monitoring.

    Succeeded

    ### 결과 확인 ###
    $ ll
    total 160
    drwxrwxr-x 2 ubuntu ubuntu   4096 Dec  7 16:59 ./
    drwxrwxr-x 4 ubuntu ubuntu   4096 Dec  7 16:58 ../
    -rw-rw-r-- 1 ubuntu ubuntu 154864 Dec  7 16:59 CCE_script.zip
    ```

<br />

## 24. bosh tasks
- 이전에 실행한 작업 및 활성된 작업에 대한 task 출력
    ```shell
    $ bosh -e [my-env] tasks [--recent[=num]] [--all] (Alias: ts)
    ```
- 사용 예시 :: 현재 활성화 된 task 출력
    ```shell
    $ bosh -e micro-bosh ts
    Using environment '10.200.1.6' as client 'admin'

    ID  State   Started At  Finished At  User   Deployment  Description     Result
    18  queued  -           -            admin  -           create release  -

    1 tasks

    Succeeded
    ```

- 사용 예시 :: 모든 task를 최근 순으로 조회
    ```shell
    $ bosh -e micro-bosh ts -r --all
    Using environment '10.200.1.6' as client 'admin'

    ID  State  Started At                    Finished At                   User       Deployment  Description                                            Result
    67  done   Sun Oct 31 10:58:13 UTC 2021  Sun Oct 31 10:58:13 UTC 2021  admin      paasta      retrieve vm-stats                                      -
    66  done   Sun Oct 31 10:53:37 UTC 2021  Sun Oct 31 10:53:38 UTC 2021  admin      paasta      retrieve vm-stats                                      -
    65  done   Sun Oct 31 10:50:12 UTC 2021  Sun Oct 31 10:50:12 UTC 2021  admin      paasta      retrieve vm-stats                                      -
    64  done   Sun Oct 31 10:43:54 UTC 2021  Sun Oct 31 10:43:55 UTC 2021  admin      paasta      retrieve vm-stats                                      -

    ...(생략)...

    39  done   Mon Oct 25 08:45:01 UTC 2021  Mon Oct 25 08:45:06 UTC 2021  admin      -           create release                                         Created release 'nats/39'
    37  done   Mon Oct 25 07:01:31 UTC 2021  Mon Oct 25 07:01:41 UTC 2021  admin      -           create release                                         Created release 'paasta-conf/1.0.2'

    30 tasks

    Succeeded
    ```

- 사용 예시 :: task를 최근 순으로 6개 조회
    ```shell
    $ bosh -e micro-bosh ts -r=6
    Using environment '10.200.1.6' as client 'admin'

    ID  State  Started At                    Finished At                   User       Deployment  Description          Result
    63  done   Sun Oct 31 07:00:01 UTC 2021  Sun Oct 31 07:00:01 UTC 2021  scheduler  paasta      snapshot deployment  snapshots of deployment 'paasta' created
    60  done   Sat Oct 30 07:00:01 UTC 2021  Sat Oct 30 07:00:01 UTC 2021  scheduler  paasta      snapshot deployment  snapshots of deployment 'paasta' created
    54  done   Fri Oct 29 07:00:01 UTC 2021  Fri Oct 29 07:00:01 UTC 2021  scheduler  paasta      snapshot deployment  snapshots of deployment 'paasta' created
    50  done   Thu Oct 28 07:00:01 UTC 2021  Thu Oct 28 07:00:01 UTC 2021  scheduler  paasta      snapshot deployment  snapshots of deployment 'paasta' created
    47  done   Wed Oct 27 07:00:01 UTC 2021  Wed Oct 27 07:00:01 UTC 2021  scheduler  paasta      snapshot deployment  snapshots of deployment 'paasta' created
    44  done   Tue Oct 26 07:00:02 UTC 2021  Tue Oct 26 07:00:02 UTC 2021  scheduler  paasta      snapshot deployment  snapshots of deployment 'paasta' created

    6 tasks

    Succeeded
    ```

<br />

## 25. bosh task
- task 아이디를 기준으로 상세 조회
    ```shell
    $ bosh -e [my-env] task [id] [--debug] [--result] [--event] [--cpi] (Alias: t)
    ```

- 예시 :: 아이디가 18인 task의 Debug 로그 출력
    ```shell
    $ bosh -e micro-bosh t 18 --debug
    Using environment '10.200.1.6' as client 'admin'

    Task 18

    I, [2021-10-25T15:51:24.023583 #30] [0x2ad090debd64]  INFO -- TaskHelper: Director Version: 271.8.0
    I, [2021-10-25T15:51:24.023616 #30] [0x2ad090debd64]  INFO -- TaskHelper: Enqueuing task: 18
    ```

<br />

## 26. bosh cancel-task
- task 취소
- 다음 checkpoint에서 task 취소
- task가 취소될 때까지 대기하지 않음

    ```shell
    $ bosh -e [my-env] cancel-task [id] (Alias: ct)
    ```

- 예시 :: 아이디가 281인 task 취소
    ```shell
    $ bosh -e vbox ct 281
    ```

<br />

## 27. bosh clean-up
- releases, stemcells, orphaned disks 그리고 사용되지 않는 다른 리소스를 clean up
    ```shell
    $ bosh -e [my-env] clean-up [--all]
    ```

- 예시 :: micro-bosh Director의 사용되지 않는 리소스 정리
    ```shell
    $ bosh -e micro-bosh clean-up --all

    Using environment '10.0.1.6' as client 'admin'

    Continue? [yN]: y

    Task 99

    Task 99 | 09:31:40 | Deleting releases: haproxy/10.1.0
    Task 99 | 09:31:40 | Deleting releases: cf-cli/1.29.0
    Task 99 | 09:31:40 | Deleting releases: binary-buildpack/1.0.36

    ...(생략)...

    Task 99 | 09:32:05 | Deleting orphaned disks: vol-05f05d7064b29501f (00:00:15)
    Task 99 | 09:32:05 | Deleting orphaned disks: vol-07f1cefdbaea166a7 (00:00:15)
    Task 99 | 09:32:05 | Deleting dns blobs: DNS blobs (00:00:00)

    Task 99 Started  Tue Oct 26 09:31:40 UTC 2021
    Task 99 Finished Tue Oct 26 09:32:05 UTC 2021
    Task 99 Duration 00:00:25
    Task 99 done

    Succeeded
    ```

<br />

### 28. bosh help
- 사용 가능한 모든 명령어와 global option 목록 조회
- 각각의 command에 대해서는 `-h`, `--help` 사용
    ```shell
    $ bosh help
    ```

- 예시
    ```shell
    $ bosh help
    Usage:
    bosh [OPTIONS] <command>

    Application Options:
    -v, --version          Show CLI version
        --config=          Config file path [$BOSH_CONFIG]
    -e, --environment=     Director environment name or URL [$BOSH_ENVIRONMENT]
        --ca-cert=         Director CA certificate path or value [$BOSH_CA_CERT]

    ...(생략)...

    Available commands:
    add-blob                Add blob                                           https://bosh.io/docs/cli-v2#add-blob
    alias-env               Alias environment to save URL and CA certificate   https://bosh.io/docs/cli-v2#alias-env

    ...(생략)...

    Succeeded
    ```

- 예시
    ```shell
    $ bosh upload-release -h
    Usage:
    bosh [OPTIONS] upload-release [upload-release-OPTIONS] [URL]

    Upload release

    https://bosh.io/docs/cli-v2#upload-release

    Application Options:

    -v, --version                    Show CLI version
        --config=                    Config file path (default: ~/.bosh/config) [$BOSH_CONFIG]
    -e, --environment=               Director environment name or URL [$BOSH_ENVIRONMENT]
        --ca-cert=                   Director CA certificate path or value [$BOSH_CA_CERT]

    ...(생략)...

    Succeeded
    ```
