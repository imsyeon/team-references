# Linux command

## 1. pwd(print working directory)
- 현재 작업중인 디렉토리 정보 출력
    
    ```shell
    ## home directory에서 실행
    $ pwd
    /home/ubuntu
    ```

<br />

## 2. cd(change directory)
- 경로 이동
- 예시
    + 절대 경로
        ```shell
        $ cd /home/ubuntu/workspace/user/joy

        $ pwd
        /home/ubuntu/workspace/user/joy
        ```
    + 상대 경로
        ```shell
        $ pwd
        /home/ubuntu/workspace/user/joy

        $ cd ..

        $ pwd
        /home/ubuntu/workspace/user/
        ```

<br />

## 3. ls(list)
- 디렉토리 목록 확인
- 기본 Syntax
    ```shell
    $ ls [옵션] [파일명 혹은 폴더명]
    ```
- 예시
    + 기본
        ```shell
        $ ls
        file1  joyBoard
        ```
    + 리스트 형식으로 출력 ex) 권한, 소유자, 갱신일 등
        ```shell
        $ ls -l
        total 4
        -rw-rw-r-- 1 ubuntu ubuntu    0 Oct 22 16:21 file1
        drwxrwxr-x 2 ubuntu ubuntu 4096 Oct 22 01:46 joyBoard
        ```
    + 숨겨진 파일을 포함한 경로 안의 모든 파일 출력
        ```shell
        $ ls -a
        .  ..  file1  joyBoard
        ```

<br />

## 4. ll
- 숨겨진 파일을 포함한 경로 안의 모든 파일을 리스트 형식으로 출력
- 예시
    ```shell
    $ ll
    total 12
    drwxrwxr-x 3 ubuntu ubuntu 4096 Oct 22 16:21 ./
    drwxrwxr-x 6 ubuntu ubuntu 4096 Oct 19 10:30 ../
    -rw-rw-r-- 1 ubuntu ubuntu    0 Oct 22 16:21 file1
    drwxrwxr-x 2 ubuntu ubuntu 4096 Oct 22 01:46 joyBoard/
    ```

<br />

## 5. cp(copy)
- 파일 또는 디렉토리 복사
- 디렉토리 복사 시 `-r` 옵션 지정 필요
- 예시
    + 파일 복사(file1 → joyfile)
        ```shell
        $ ls
        file1

        $ cp file1 joyfile

        $ ls
        file1  joyfile
        ``` 
    + 디렉토리 복사(testdir → joydir)
        ```shell
        $ ls
        testdir

        $ cp -r testdir joydir

        $ ls
        joydir testdir
        ```

<br />

## 6. mv(move)
- 파일 또는 디렉토리를 원하는 위치로 이동
- 이름을 변경하는 용도로도 사용
- 기본 Syntax
  ```shell
  $ mv [-option] [sources] [target]
  ```
- 예시
    + 파일 이동
        ```shell
        $ ls
        joyfile testdir

        $ mv joyfile testdir

        $ ls
        testdir
        $ ls testdir
        joyfile
        ```
    + 디렉토리 이동
        ```shell
        $ ls
        testdir joydir

        $ mv joydir testdir

        $ ls
        testdir
        $ ls testdir
        joydir
        ```

<br />

## 7. mkdir(make directory)
- 디렉토리 생성
- 기본 Syntax
  ```shell
  $ mkdir [-m mode] [-p] [디렉토리명]
  ```
- 옵션
  + `-p` : 하위 디렉토리까지 한 번에 생성
- 예시
  + 기본
    ```shell
    $ mkdir testdir
    
    $ ls
    testdir
    ```
  + 하위 디렉토리까지 생성
    ```shell
    $ mkdir -p testdir/innerdir/
    
    $ ls
    testdir
    $ ls testdir
    innerdir
    ```

<br />

## 8. rm(remove)
- 파일 또는 디렉토리 삭제
- 디렉토리 삭제 시 하위 디렉토리까지 모두 삭제됨
- 기본 Syntax
  ```shell
  $ rm [옵션] [파일명 / 디렉토리명]
  ```
- 옵션
  + `-r` : 디렉토리 삭제 시 지정
  + `-f` : 삭제 여부를 묻지 않고 바로 삭제
- 예시
  + 파일 삭제
    ```shell
    $ ls
    file1 joyfile
    
    $ rm file1
    
    $ ls
    joyfile
    ```
  + 삭제 여부 묻지 않고 디렉토리 삭제
    ```shell
    $ ls
    joydir testdir
    
    $ rm -rf testdir
    
    $ ls
    joydir
    ```

<br />

## 9. touch
- 파일이나 디렉토리의 최근 업데이트 일자를 현재 시간으로 변경
- 파일이나 디렉토리가 존재하지 않을 경우 **빈 파일** 생성
- 파일의 날짜와 시간 수정
- 기본 Syntax
  ```shell
  $ touch [OPTION] [파일이름]
  ```
- 예시
  - testfile 파일 생성
    ```shell
    $ touch testfile
    $ ll
    -rw-rw-r-- 1 ubuntu ubuntu    0 Oct 31 21:01 testfile
    ```
  - 현 시간으로 파일의 접근 시간, 변경 시간 수정
    ```
    $ touch -a test.txt
    ```
  - 파일의 날짜 정보를 원하는 형식으로 변경
    ```
    $ touch -t 201306141200 test.txt
    ```

<br />

## 10. cat(concatenate)
- 파일 이름을 인자로 받아 내용을 이어줌
- 기본 Syntax
  ```shell
  $ cat [옵션] [파일명]
  ```
- 예시
  + 파일 내용 출력
    ```shell
    $ cat testfile
    hello, world!
    ```
  + 여러 개의 파일을 합쳐 하나의 파일로 생성
    ```shell
    $ ls
    file1 file2
    
    $ cat file1 file2 > file3
    
    $ ls
    file1 file2 file3
    ```
  + 기존 한 파일의 내용을 다른 파일에 덧붙임
    ```shell
    ## file1, file2의 내용 확인
    $ cat file1
    my name is revit.
    $ cat file2
    Nice to meet you!
    
    $ cat file1 >> file2
    
    $ cat file2
    Nice to meet you!
    my name is revit.
    ```
  + 새로운 파일 생성
    ```shell
    $ ls
    file1 file2
    
    $ cat > testfile
    this is testfile.  # 내용 작성, 종료 시 ctrl + d 로 내용 저장
    
    $ ls
    file1 file2 testfile
    $ cat testfile
    this is testfile.
    ```

<br />

## 11. tail
- 파일의 뒷부분을 보고싶은 행 수 만큼 출력
- 기본 Syntax
  ```shell
  $ tail [옵션] [파일]
  ```
- 옵션
  + `-[숫자]` : 지정한 숫자만큼 행 출력
  + `-f`
    - 파일 내용을 화면에 계속 띄워 줌
    - 파일 내용이 변경되면 업데이트된 내용을 갱신해줌
    - 주로 실시간으로 내용이 추가되는 로그 파일을 모니터링 할 때 유용하게 사용
- 예시
  + 기본 출력(10줄)
    ```shell
    $ tail loggr-syslog-agent.stderr.log
    2021/10/25 09:24:22 failed to fetch bindings: Get "https://q-s0.scheduler.default.paasta.bosh:9000/bindings": dial tcp 10.200.1.128:9000: connect: connection refused
    2021/10/25 09:24:44 failed to fetch bindings: Get "https://q-s0.scheduler.default.paasta.bosh:9000/bindings": dial tcp 10.200.1.128:9000: connect: connection refused
    2021/10/25 09:25:05 failed to fetch bindings: Get "https://q-s0.scheduler.default.paasta.bosh:9000/bindings": dial tcp 10.200.1.128:9000: connect: connection refused
    2021/10/25 09:25:27 failed to fetch bindings: Get "https://q-s0.scheduler.default.paasta.bosh:9000/bindings": dial tcp 10.200.1.128:9000: connect: connection refused
    2021/10/25 09:25:49 failed to fetch bindings: Get "https://q-s0.scheduler.default.paasta.bosh:9000/bindings": dial tcp 10.200.1.128:9000: connect: connection refused
    2021/10/25 09:26:10 failed to fetch bindings: Get "https://q-s0.scheduler.default.paasta.bosh:9000/bindings": dial tcp 10.200.1.128:9000: connect: connection refused
    2021/10/25 09:26:32 failed to fetch bindings: Get "https://q-s0.scheduler.default.paasta.bosh:9000/bindings": dial tcp 10.200.1.128:9000: connect: connection refused
    2021/10/25 09:26:54 failed to fetch bindings: Get "https://q-s0.scheduler.default.paasta.bosh:9000/bindings": dial tcp 10.200.1.128:9000: connect: connection refused
    2021/10/25 09:27:15 failed to fetch bindings: Get "https://q-s0.scheduler.default.paasta.bosh:9000/bindings": dial tcp 10.200.1.128:9000: connect: connection refused
    2021/10/25 09:27:37 failed to fetch bindings: Get "https://q-s0.scheduler.default.paasta.bosh:9000/bindings": dial tcp 10.200.1.128:9000: connect: connection refused
    ```
  + 3줄 출력
    ```shell
    ## 3줄 출력
    $ tail -3 loggr-syslog-agent.stderr.log
    2021/10/25 09:26:54 failed to fetch bindings: Get "https://q-s0.scheduler.default.paasta.bosh:9000/bindings": dial tcp 10.200.1.128:9000: connect: connection refused
    2021/10/25 09:27:15 failed to fetch bindings: Get "https://q-s0.scheduler.default.paasta.bosh:9000/bindings": dial tcp 10.200.1.128:9000: connect: connection refused
    2021/10/25 09:27:37 failed to fetch bindings: Get "https://q-s0.scheduler.default.paasta.bosh:9000/bindings": dial tcp 10.200.1.128:9000: connect: connection refused
    ```
  + -f 옵션 적용(ctrl + c 로 종료)
    ```shell
    $ tail -f loggr-syslog-agent.stderr.log
    2021/10/25 09:24:22 failed to fetch bindings: Get "https://q-s0.scheduler.default.paasta.bosh:9000/bindings": dial tcp 10.200.1.128:9000: connect: connection refused
    2021/10/25 09:24:44 failed to fetch bindings: Get "https://q-s0.scheduler.default.paasta.bosh:9000/bindings": dial tcp 10.200.1.128:9000: connect: connection refused
    2021/10/25 09:25:05 failed to fetch bindings: Get "https://q-s0.scheduler.default.paasta.bosh:9000/bindings": dial tcp 10.200.1.128:9000: connect: connection refused
    2021/10/25 09:25:27 failed to fetch bindings: Get "https://q-s0.scheduler.default.paasta.bosh:9000/bindings": dial tcp 10.200.1.128:9000: connect: connection refused
    ...(생략)...
    ```

<br />

## 12. find
- 특정 파일이나 디렉토리 검색
- 기본 Syntax
  ```shell
  $ find [검색 경로] -name [파일명]
  ```
- 옵션
    + `-exec` : 명령어 실행
    + `-type` : 디렉토리나 파일만 지정하여 검색
- 예시    
    + 확장자명 검색
        ```shell
        $ find /home/ubuntu/workspace/user/joy/joyBoard -name '*.war'
        /home/ubuntu/workspace/user/joy/joyBoard/demo-0.0.1-SNAPSHOT.war
        ```
    + 확장자가 .PNG인 파일만 찾아 삭제
        ```shell
        $ ls
        testfile.txt error1.PNG
        
        $ find ./ -name "*.PNG" -exec rm {} \;
        
        $ ls
        testfile.txt
        ```
    + 확장자가 .txt인 파일만 찾아 파일 내용 수정
        ```shell
        $ cat testfile.txt
        Hi, my name is revit.
        
        $ find ./ -name "*.txt" -exec sed -i 's/Hi/Hello/g' {} \;
        
        $ cat testfile.txt
        Hello, my name is revit.
        ```
    + 파일만 검색 
        ```shell
        $ find ./ -type f
        ./file2
        ./file1
        ./joyBoard/demo-0.0.1-SNAPSHOT.war
        ./file33
        ./testfile.txt
        ./testfile
        ```
    + 파일명 검색
        ```shell
        $ find /home/ubuntu/workspace/user/joy/ -name 'file1'
        /home/ubuntu/workspace/user/joy/file1
        ```
    + 파일 이름에 특정 문자열이 포함된 파일 검색
        ```shell
        $ find -name "*git*"

        ./.gradle/wrapper/dists/gradle-7.2-bin/2dnblmf4td7x66yl1d74lt32g/gradle-7.2/lib/plugins/org.eclipse.jgit-5.7.0.202003110725-r.jar
        ./workspace/user/rex/BACKUP/20210510_paasta_deployment_backup/.git
        ./workspace/user/rex/BACKUP/20210510_paasta_deployment_backup/deployment/paasta-deployment/bosh/tests/.gitignore
        ./workspace/user/rex/BACKUP/20210510_paasta_deployment_backup/deployment/paasta-deployment/bosh/.gitignore
        ./workspace/user/rex/BACKUP/20210510_paasta_deployment_backup/deployment/paasta-deployment/paasta/units/vendor/github.com
        ./workspace/user/rex/BACKUP/20210510_paasta_deployment_backup/deployment/paasta-deployment/paasta/.gitignore
        ./workspace/user/rex/BACKUP/20211018-kevin-paasta-5.5.2/deployment/paasta-deployment/bosh/tests/.gitignore
        ...

        ```

<br />

## 13. chmod(change mode)
- 리눅스의 파일이나 디렉토리의 권한 변경
- 기본 Syntax
  ```shell
  $ chmod [OPTION] [MODE] [FILE]
  ```
- 예시
  + 현재 경로에 있는 모든 .sh 파일에 실행 권한 부여
    ```shell
    $ ll
    -rw-rw-r--  1 ubuntu ubuntu   251 Oct 25 15:26 deploy-gcp.sh
    -rw-rw-r--  1 ubuntu ubuntu   270 Oct 25 15:26 deploy-openstack.sh
    
    $ chmod +x ./*.sh
    
    $ ll
    -rwxrwxr-x  1 ubuntu ubuntu   251 Oct 25 15:26 deploy-gcp.sh*
    -rwxrwxr-x  1 ubuntu ubuntu   270 Oct 25 15:26 deploy-openstack.sh*
    ```
  + 파일을 소유한 그룹과 그 외 사용자의 모든 권한 제거
    ```shell
    $ ll
    -rw-rw-r-- 1 ubuntu ubuntu   18 Oct 31 21:04 file1
    
    $ chmod go-rwx file1
    
    $ ll
    -rw------- 1 ubuntu ubuntu   18 Oct 31 21:04 file1
    ```

<br />

## 14. ifconfig(interface configuration)
- 네트워크 인터페이스 설정 및 확인
- IP 주소, 서브넷 마스크, MAC 주소, 네트워크 상태 등 확인 및 설정 가능
- 기본 Syntax
  ```shell
  $ ifconfig [interface] [option] [address] [up/down]
  ```
- 예시
  + IP 주소 확인
    ```shell
    $ ifconfig
    ens3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1400
        inet 10.200.4.45  netmask 255.255.255.0  broadcast 10.200.4.255
        inet6 fe80::f816:3eff:fed2:fd53  prefixlen 64  scopeid 0x20<link>
        ether fa:16:3e:d2:fd:53  txqueuelen 1000  (Ethernet)
        RX packets 228713  bytes 2067431097 (2.0 GB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 263617  bytes 1978677503 (1.9 GB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    
    lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 1202  bytes 96240 (96.2 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1202  bytes 96240 (96.2 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    ```
  + enp0이라는 이더넷에 아이피 192.168.0.9를 설정하고, 서브넷마스크를 255.255.255.224로 설정
      ```shell
      ## ifconfig 이더넷명 주소 netmask 주소 broadcast 주소
      $ ifconfig enp0 192.168.0.9 netmask 255.255.255.224
      ```
  + 이더넷(네트워크 인터페이스) 활성화/비활성화
      ```shell
      ## ifconfig 이더넷명 [up/down]
      $ ifconfig enp0 up
      ```

<br />

## 15. netstat(network statistics)
- 전송 제어 프로토콜, 라우팅 테이블, 네트워크 인터페이스, 네트워크 프로토콜 통계를 위한 네트워크 연결을 보여주는 명령 도구
- 기본 Syntax
  ```shell
  $ netstat [-m] [-n] [-s] [-i | -r] [-f address-family]
  ``` 
- 예시
  + 연결을 기다리는 목록과 프로그램 출력
    ```shell
    $ netstat -nap
    (Not all processes could be identified, non-owned process info
    will not be shown, you would have to be root to see it all.)
    Active Internet connections (servers and established)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
    tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -
    tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                   
    
    ...(생략)...

    Active UNIX domain sockets (servers and established)
    Proto RefCnt Flags       Type       State         I-Node   PID/Program name     Path
    unix  2      [ ]         DGRAM                    69776    9627/systemd         /run/user/1000/systemd/notify
    unix  2      [ ACC ]     SEQPACKET  LISTENING     13007    -                    /run/udev/control
    unix  2      [ ACC ]     STREAM     LISTENING     69779    9627/systemd         /run/user/1000/systemd/private

    ...(생략)...
    
    unix  3      [ ]         DGRAM                    12981    -
    unix  3      [ ]         STREAM     CONNECTED     17984    -
    unix  3      [ ]         STREAM     CONNECTED     16429    -                    /run/systemd/journal/stdout
    ```
  + 특정 포트가 사용 중인지 확인
    ```shell
    $ netstat -an | grep 6010
    tcp        0      0 127.0.0.1:6010          0.0.0.0:*               LISTEN
    tcp6       0      0 ::1:6010                :::*                    LISTEN
    ```
  + 라우팅 정보 출력
    ```shell
    $ netstat -rn
    Kernel IP routing table
    Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
    0.0.0.0         10.200.4.1      0.0.0.0         UG        0 0          0 ens3
    10.200.0.0      0.0.0.0         255.255.255.0   U         0 0          0 ens3
    10.200.1.0      0.0.0.0         255.255.255.0   U         0 0          0 ens3
    10.200.2.0      0.0.0.0         255.255.255.0   U         0 0          0 ens3
    10.200.3.0      0.0.0.0         255.255.255.0   U         0 0          0 ens3
    10.200.4.0      0.0.0.0         255.255.255.0   U         0 0          0 ens3
    10.200.5.0      0.0.0.0         255.255.255.0   U         0 0          0 ens3
    10.200.6.0      0.0.0.0         255.255.255.0   U         0 0          0 ens3
    169.254.169.254 10.200.4.2      255.255.255.255 UGH       0 0          0 ens3
    ```
  + TCP listening 상태의 포트와 프로그램 출력
    ```shell
    $ netstat -lnpt
    (Not all processes could be identified, non-owned process info
    will not be shown, you would have to be root to see it all.)
    Active Internet connections (only servers)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
    tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -
    tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -
    tcp        0      0 127.0.0.1:6010          0.0.0.0:*               LISTEN      -
    tcp6       0      0 :::22                   :::*                    LISTEN      -
    tcp6       0      0 ::1:6010                :::*                    LISTEN      -
    ```
- Tip
  ```shell
  $ netstat -antplF
  Proto Recv-Q Send-Q Local Address         Foreign Address         State       PID/Program name
  tcp        0      0 127.0.0.1:631         0.0.0.0:*               LISTEN      -
  tcp        0      0 0.0.0.0:9050          0.0.0.0:*               LISTEN      -
  ```
  + `Local Address` : 현재 열려 있거나 listening 하고 있는 ip, port
    - `0.0.0.0` : all interface를 받음
    - `127.0.0.1` : 자기자신만 호출 가능(loopback)

<br />

## 16. ping(packet internet groper)
- 네트워크의 상태를 점검 및 진단
- TCP/IP 프로토콜 중 ICMP를 통해 동작<br>→ ICMP를 지원하지 않거나 차단하는 기기를 대상으로 ping 수행 및 대응 불가능
- 기본 Syntax
  ```shell
  $ ping [목적지] [옵션]
  ```
- 예시
  + target 호스트 연결 가능 여부 확인
    ```shell
    $ ping www.google.com
    PING www.google.com (142.251.42.164) 56(84) bytes of data.
    64 bytes from nrt12s46-in-f4.1e100.net (142.251.42.164): icmp_seq=1 ttl=111 time=40.0 ms
    64 bytes from nrt12s46-in-f4.1e100.net (142.251.42.164): icmp_seq=2 ttl=111 time=39.8 m
    ...
    ```
    ```shell
    $ ping 10.200.1.133
    PING 10.200.1.133 (10.200.1.133) 56(84) bytes of data.
    64 bytes from 10.200.1.133: icmp_seq=1 ttl=64 time=0.611 ms
    64 bytes from 10.200.1.133: icmp_seq=2 ttl=64 time=0.233 ms
    ...
    ```
  + ping이 종료된 후 보낼 ECHO_REQUEST 수 지정
    ```shell
    $ ping -c 5 www.google.com
    PING www.google.com (142.251.42.164) 56(84) bytes of data.
    64 bytes from nrt12s46-in-f4.1e100.net (142.251.42.164): icmp_seq=1 ttl=111 time=39.9 ms
    64 bytes from nrt12s46-in-f4.1e100.net (142.251.42.164): icmp_seq=2 ttl=111 time=40.0 ms
    64 bytes from nrt12s46-in-f4.1e100.net (142.251.42.164): icmp_seq=3 ttl=111 time=40.2 ms
    64 bytes from nrt12s46-in-f4.1e100.net (142.251.42.164): icmp_seq=4 ttl=111 time=40.0 ms
    64 bytes from nrt12s46-in-f4.1e100.net (142.251.42.164): icmp_seq=5 ttl=111 time=40.1 ms
    
    --- www.google.com ping statistics ---
    5 packets transmitted, 5 received, 0% packet loss, time 4005ms
    rtt min/avg/max/mdev = 39.958/40.098/40.293/0.211 ms
    ```

<br />

## 17. tar(Tape Archive)
- 디렉토리의 구조, 파일의 속성들을 보존하면서 여러 개의 파일을 하나의 큰 파일로 묶기 위해 주로 사용
    + tar : 단순히 파일과 폴더를 하나의 파일로 묶어주는 유틸
    + tar.gz : 파일 및 폴더들을 묶어서 압축
- 기본 Syntax
  ```shell
  $ tar [옵션] [경로]
  ```
- 예시
    + tar로 압축하기
        ```shell
        $ tar -cvf [파일명.tar] [폴더명]

        ## ex) abc 폴더를 aaa.tar로 압축
        $ tar -cvf aaa.tar abc
        abc/
        abc/test.txt
        abc/test2.txt

        $ ll
        total 5892
        drwxr-xr-x 3 ubuntu ubuntu    4096 Nov  1 09:47 ./
        drwxrwxr-x 4 ubuntu ubuntu    4096 Nov  1 09:43 ../
        -rw-rw-r-- 1 ubuntu ubuntu   10240 Nov  1 09:47 aaa.tar
        drwxrwxr-x 2 ubuntu ubuntu    4096 Nov  1 09:46 abc/
        -rw-rw-r-- 1 ubuntu ubuntu 6006320 Sep 28 07:37 credhub-linux-2.9.1.tgz
        ```
    + tar 압축 풀기
        ```shell
        $ tar -xvf [파일명.tar]

        ## ex) aaa.tar 파일 압축해제
        $ tar -xvf aaa.tar
        abc/
        abc/test.txt
        abc/test2.txt

        $ ll
        total 5892
        drwxr-xr-x 3 ubuntu ubuntu    4096 Nov  1 09:48 ./
        drwxrwxr-x 4 ubuntu ubuntu    4096 Nov  1 09:43 ../
        -rw-rw-r-- 1 ubuntu ubuntu   10240 Nov  1 09:47 aaa.tar
        drwxrwxr-x 2 ubuntu ubuntu    4096 Nov  1 09:46 abc/
        -rw-rw-r-- 1 ubuntu ubuntu 6006320 Sep 28 07:37 credhub-linux-2.9.1.tgz
        ```
    + tar.gz로 압축하기
        ```shell
        $ tar -zcvf [파일명.tar.gz] [폴더명]

        ## ex) abc 폴더를 aaa.tar.gz로 압축
        $ tar -zcvf aaa.tar.gz abc
        abc/
        abc/test.txt
        abc/test2.txt

        $ ll
        total 5884
        drwxr-xr-x 3 ubuntu ubuntu    4096 Nov  1 09:49 ./
        drwxrwxr-x 4 ubuntu ubuntu    4096 Nov  1 09:43 ../
        -rw-rw-r-- 1 ubuntu ubuntu     186 Nov  1 09:49 aaa.tar.gz
        drwxrwxr-x 2 ubuntu ubuntu    4096 Nov  1 09:46 abc/
        -rw-rw-r-- 1 ubuntu ubuntu 6006320 Sep 28 07:37 credhub-linux-2.9.1.tgz
        ```
    + tar.gz 압축 풀기
        ```shell
        $ tar -zxvf [파일명.tar.gz]

        ## ex) aaa.tar.gz 압축파일 해제
        $ tar -xzf aaa.tar.gz
        $ ll
        total 5884
        drwxr-xr-x 3 ubuntu ubuntu    4096 Nov  1 09:50 ./
        drwxrwxr-x 4 ubuntu ubuntu    4096 Nov  1 09:43 ../
        -rw-rw-r-- 1 ubuntu ubuntu     186 Nov  1 09:49 aaa.tar.gz
        drwxrwxr-x 2 ubuntu ubuntu    4096 Nov  1 09:46 abc/
        -rw-rw-r-- 1 ubuntu ubuntu 6006320 Sep 28 07:37 credhub-linux-2.9.1.tgz
        ```

<br />

## 18. scp(SecureCopy)
- 원격지에 있는 파일과 디렉터리를 보내거나 가져올 때 사용하는 파일 전송 프로토콜
- 기본 Syntax
    ```shell
    $ scp [options ...] [source] [target]
    ```
- 예시
    + Local -> Remote 파일 전송
        ```shell
        $ scp /home/example.txt test@141.211.xx.xxx:/home/test
        ```
    + Local -> Remote 디렉터리 전송
        ```shell
        $ scp -r scp_directory scp_test ubuntu@10.20.1.110:/tmp
        ```
    + Remote -> Local 파일 전송
        ```shell
        $ scp test@141.211.xx.xxx:/home/test.txt /home/example
        ```
    + Remote(source) -> Remote(target) 파일 전송
        ```shell
        $ scp test@141.211.xx.xxx:/home/test.txt gildong@141.223.xx.xxx:/home/example
        ```

<br />

## 19. ssh(Secure SHell)
- 네트워크 상의 다른 컴퓨터에 로그인하거나 원격 시스템에서 명령을 실행
- 기본 Syntax
    ```shell
    $ ssh [host주소] -i [key file]
    ```
- 예시
    + ssh key를 사용하여 접속
        ```shell
        $ ssh ubuntu@10.200.24.15 -i c3-key.ppk
        ```
    + port를 지정하여 접속
        ```shell
        $ ssh -p 12022 ubuntu@10.200.24.15
        ```
    + ssh 접속 후 명령어 수행
        ```shell
        $ ssh ubuntu@10.200.24.15 ps -ef
        ```
<br />

## 20. man
- 명령에 대한 매뉴얼 확인
- 예시
    ```shell
    $ man ls

        NAME
        ls - list directory contents

    SYNOPSIS
        ls [OPTION]... [FILE]...

    DESCRIPTION
        List information about the FILEs (the current directory by default).  Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.

        Mandatory arguments to long options are mandatory for short options too.

        -a, --all
                do not ignore entries starting with .

        -A, --almost-all
                do not list implied . and ..

    ...
    ```

<br />

## 21. ps(Process Status)
+ 프로세스 목록 확인
+ 유닉스 BSD, System V에 따라 옵션이 달라짐
  - man 명령어 확인 후 사용
+ BSD는 'ps aux' 옵션을 많이 사용
+ System V는 'ps -ef' 옵션을 많이 사용
+ 특정 프로세스 확인 시 주로 grep 명령어와 함께 사용
+ 예시
    - 시스템에 동작중인 모든 프로세스를 출력
        ```shell
        $ ps -ef
        UID        PID  PPID  C STIME TTY          TIME CMD
        root         1     0  0 Oct27 ?        00:00:03 /sbin/init
        root         2     0  0 Oct27 ?        00:00:00 [kthreadd]
        root         4     2  0 Oct27 ?        00:00:00 [kworker/0:0H]
        root         6     2  0 Oct27 ?        00:00:00 [mm_percpu_wq]
        root         7     2  0 Oct27 ?        00:00:00 [ksoftirqd/0]
        root         8     2  0 Oct27 ?        00:00:01 [rcu_sched]
        ```
    - 시스템에 동작중인 모든 프로세스를 출력 (BSD)
        ```shell
        $ ps aux
        USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
        root         1  0.0  0.1 225188  8572 ?        Ss   09:02   0:02 /sbin/init
        root         2  0.0  0.0      0     0 ?        S    09:02   0:00 [kthreadd]
        root         3  0.0  0.0      0     0 ?        I    09:02   0:00 [kworker/0:0]
        ```
    - 특정 사용자가 사용하는 프로세스의 정보를 출력
        ```shell
        $ ps -U root -u root
        PID TTY          TIME CMD
            1 ?        00:00:03 systemd
            2 ?        00:00:00 kthreadd
            4 ?        00:00:00 kworker/0:0H
            6 ?        00:00:00 mm_percpu_wq
            7 ?        00:00:00 ksoftirqd/0
        ```

<br />

## 22. kill
- 프로세스에 특정한 signal을 보내는 명령어
- 종료되지 않는 프로세스를 종료 시킬 때 많이 사용
- 기본 Syntax
    ```shell
    $ kill [options or 시그널 (번호 또는 이름) ] <pid>
    ```
- 예시
    + signal의 종류 출력
        ```shell
        $ kill -l

        1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL       5) SIGTRAP
        6) SIGABRT      7) SIGBUS       8) SIGFPE       9) SIGKILL     10) SIGUSR1
        11) SIGSEGV     12) SIGUSR2     13) SIGPIPE     14) SIGALRM     15) SIGTERM
        16) SIGSTKFLT   17) SIGCHLD     18) SIGCONT     19) SIGSTOP     20) SIGTSTP
        21) SIGTTIN     22) SIGTTOU     23) SIGURG      24) SIGXCPU     25) SIGXFSZ
        26) SIGVTALRM   27) SIGPROF     28) SIGWINCH    29) SIGIO       30) SIGPWR
        ```
    + 안전하게 종료
        ```shell
        $ kill -INT 123
        or
        $ kill -2 123
        ```
    + 강제 종료
        ```shell
        $ kill -9 123
        ```

<br />

## 23. history
- 이전에 입력한 명령어 및 고유 식별 번호 출력
- 예시
    + 이전에 입력한 명령어 조회 
        ```shell
        $ history
        998  ll
        999  cf apps
        1000  cf create-buildpack java_buildpack_pinpoint java-buildpack-pinpoint-monitoring-offline-745b745.zip 12
        1001  bosh vms
        1002  cf buildpacks
        ```
    + history 번호로 명령어 실행
        ```shell
        $ history
        ... 
        135  ls -al bin\     
        136  cd ..           
        137  ls              
        138  cat company.json
        ...

        $ !137
        AdventureWorks.xlsx   company.json   Sales.xlsx
        ```
    + 바로 직전 명령어를 실행
        ```shell
        ## 이전 명령어에 추가해서 실행
        $ !! | grep Linux

        ## 이전 명령어를 sudo로 실행
        $ sudo !!

        ## 출력 결과를 파일이나 스크립트로 저장
        $ echo !! >> script.sh
        ```

<br />

## 24. source
- 스크립트 파일을 수정한 후에 수정된 값을 바로 적용하기 위해 사용하는 명령어
- bash의 내부 명령어로 명령어 뒤에 오는 파일을 읽어서 파일 내용 실행하는 역할
- 기본적으로 다운로드 경로의 마지막 슬래쉬('/') 다음에 오는 단어를 파일이름으로 사용
- 예시
    ```shell
    $ source /etc/profile
    ```

<br />

## 25. wget
- 파일을 다운로드하기 위한 명령어
- 기본 Syntax
    ```shell
    $ wget -O [저장할 파일명] [다운로드 url]
    ```
- 예시
    + 다른 이름으로 다운로드
        ```shell
        $ wget -O sample.zip https://nextcloud.paas-ta.org/index.php/s/x8Tg37WDFiL5ZDi/download
        --2021-12-02 14:46:09--  https://nextcloud.paas-ta.org/index.php/s/x8Tg37WDFiL5ZDi/download
        Resolving nextcloud.paas-ta.org (nextcloud.paas-ta.org)... 203.255.255.116
        Connecting to nextcloud.paas-ta.org (nextcloud.paas-ta.org)|203.255.255.116|:443... connected.
        HTTP request sent, awaiting response... 200 OK
        Length: 316727369 (302M) [application/zip]
        Saving to: ‘sample.zip’

        sample.zip                                           100%[======================================================================================================================>] 302.05M   312MB/s    in 1.0s    

        2021-12-02 14:46:10 (312 MB/s) - ‘sample.zip’ saved [316727369/316727369]

        ubuntu@c3-inception-03:~/workspace/user/kevin/test$ ll
        total 309316
        drwxrwxr-x  2 ubuntu ubuntu      4096 Dec  2 14:46 ./
        drwxr-xr-x 10 ubuntu ubuntu      4096 Dec  2 14:44 ../
        -rw-rw-r--  1 ubuntu ubuntu 316727369 Dec  2 14:46 sample.zip
        ```
    + 백그라운드로 다운로드
        ```shell
        $ wget -b sample.zip https://nextcloud.paas-ta.org/index.php/s/x8Tg37WDFiL5ZDi/download
        Continuing in background, pid 5922.
        Output will be written to ‘wget-log’.

        $ ll
        total 309316
        drwxrwxr-x  2 ubuntu ubuntu      4096 Dec  2 14:48 ./
        drwxr-xr-x 10 ubuntu ubuntu      4096 Dec  2 14:44 ../
        -rw-rw-r--  1 ubuntu ubuntu 316727369 Dec  2 14:48 download
        -rw-rw-r--  1 ubuntu ubuntu         0 Dec  2 14:48 wget-log
        ```
    + 중단된 다운로드 이어받기
        ```shell
        $ wget -c https://nextcloud.paas-ta.org/index.php/s/x8Tg37WDFiL5ZDi/download
        --2021-12-02 14:52:03--  https://nextcloud.paas-ta.org/index.php/s/x8Tg37WDFiL5ZDi/download
        Resolving nextcloud.paas-ta.org (nextcloud.paas-ta.org)... 203.255.255.116
        Connecting to nextcloud.paas-ta.org (nextcloud.paas-ta.org)|203.255.255.116|:443... connected.
        HTTP request sent, awaiting response... 206 Partial Content
        Length: 316727369 (302M), 237949001 (227M) remaining [application/zip]
        Saving to: ‘download’

        download                                             100%[+++++++++++++++++++++++++++++=========================================================================================>] 302.05M   332MB/s    in 0.7s    

        2021-12-02 14:52:04 (332 MB/s) - ‘download’ saved [316727369/316727369]

        $ ll
        total 309316
        drwxrwxr-x  2 ubuntu ubuntu      4096 Dec  2 14:51 ./
        drwxr-xr-x 10 ubuntu ubuntu      4096 Dec  2 14:44 ../
        -rw-rw-r--  1 ubuntu ubuntu 316727369 Dec  2 14:52 download
        ```
    + 다운로드 가능 여부 확인
        ```shell
        $ wget --spider https://nextcloud.paas-ta.org/index.php/s/x8Tg37WDFiL5ZDi/download
        Spider mode enabled. Check if remote file exists.
        --2021-12-02 14:57:44--  https://nextcloud.paas-ta.org/index.php/s/x8Tg37WDFiL5ZDi/download
        Resolving nextcloud.paas-ta.org (nextcloud.paas-ta.org)... 203.255.255.116
        Connecting to nextcloud.paas-ta.org (nextcloud.paas-ta.org)|203.255.255.116|:443... connected.
        HTTP request sent, awaiting response... 200 OK
        Length: 316727369 (302M) [application/zip]
        Remote file exists.
        ```
    + 다운로드 폴더 지정 (해당 폴더 없을 시 자동 생성)
        ```shell
        $ wget -P download_test https://nextcloud.paas-ta.org/index.php/s/x8Tg37WDFiL5ZDi/download
        --2021-12-02 15:02:28--  https://nextcloud.paas-ta.org/index.php/s/x8Tg37WDFiL5ZDi/download
        Resolving nextcloud.paas-ta.org (nextcloud.paas-ta.org)... 203.255.255.116
        Connecting to nextcloud.paas-ta.org (nextcloud.paas-ta.org)|203.255.255.116|:443... connected.
        HTTP request sent, awaiting response... 200 OK
        Length: 316727369 (302M) [application/zip]
        Saving to: ‘download_test/download’

        download                                             100%[======================================================================================================================>] 302.05M   329MB/s    in 0.9s    

        2021-12-02 15:02:29 (329 MB/s) - ‘download_test/download’ saved [316727369/316727369]

        $ ll
        total 12
        drwxrwxr-x  3 ubuntu ubuntu 4096 Dec  2 15:02 ./
        drwxr-xr-x 10 ubuntu ubuntu 4096 Dec  2 14:44 ../
        drwxrwxr-x  2 ubuntu ubuntu 4096 Dec  2 15:02 download_test/
        ```

<br />

## 26. 파이프라인
- 명령어의 연결
- 예시
    + 파이프(`|`)를 사용하여 명령어의 결과를 `|` 다음으로 전달할 수 있음
        ```shell
        ## ls --help에서 sort가 포함된 행을 출력
        $ ls --help | grep sort

        Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.
        -c                         with -lt: sort by, and show, ctime (time of last
                                    with -l: show ctime and sort by name;
                                    otherwise: sort by ctime, newest first
        -f                         do not sort, enable -aU, disable -ls --color
                                    can be augmented with a --sort option, but any
                                    use of --sort=none (-U) disables grouping
        ...
        ```
        ```shell                          
        ## ls --help에서 sort와 file이 포함된 행을 출력
        $ ls --help | grep sort | grep file

        -S                         sort by file size, largest first
        ```
    + 성공한 후에 다음 명령어 실행
        ```shell
        ## mkdir test 명령어가 성공해야 뒤에 명령어도 실행
        $ mkdir test && cd test && touch abc
        ```
    + 성공 여부와 상관없이 다음 명령어 실행
        ```shell
        ## mkdir test 명령어가 실패하더라도 cd 명령어, touch 명령어는 실행
        $ mkdir test(실패); cd test; touch abc
        ```

<br />

## 27. grep
- 텍스트 파일에서 원하는 문자열이 들어간 행을 찾아 출력하는 명령어
- 주로 log 파일에서 특정 날짜, error 메세지를 찾는데 유용하게 사용
- 기본 Syntax
    ```shell
    $ grep "문자열" [파일 이름][파일 이름]
    ```
- 예시
    + tomcat이 포함된 프로세스 출력
    ```shell
    $ ps -ef | grep tomcat

    22180 22151  0 10:19 pts/0    00:00:00 grep --color=auto tomcat
    ```

<br />

## 28. su(Switch user)
- 현재 사용자를 로그아웃 하지 않은 상태에서 다른 사용자의 계정으로 전환하는 명령어
- 기본 Syntax
  ```shell
  $ su [사용자]
  ```
- 예시
    + 사용자 전환
      ```shell
      $ su user01
      ```
    + 사용자 전환 및 전환된 사용자의 환경변수 적용
      ```shell
      $ su - user01
      ```

<br />

## 29. apt-get
- 패키지 설치/ 삭제 도구
- 데비안, 우분투 계열에서 사용
- 예시
    + 패키지 업그레이드
        ```shell
        $ apt-get upgrade
        $ apt-get upgrade [패키지명]
        ```
    + 패키지 삭제
        ```shell
        $ apt-get remove [패키지명]

        $ sudo apt-get remove openjdk*
        ```
    + 패키지 및 패키지의 환경설정 모두 삭제
        ```shell
        $ apt-get purge [패키지명]

        $ sudo apt-get purge openjdk*
        ```
    + remove 또는 purge 명령어로 다른 패키지 삭제 시 --auto-remove 옵션을 주면 패키지를 삭제하면서 불필요한 의존성 패키지들도 함께 삭제
        ```shell
        $ sudo apt-get remove --auto-remove openjdk*
        $ sudo apt-get purge --auto-remove openjdk*
        ```
    + 패키지 설치
        ```shell
        $ apt-get install
        ```

<br />

## 30. yum
- 패키지 설치/ 삭제 도구
- 레드햇 계열에서 사용
- 예시
    + 패키지 설치
        ```shell
        $ yum install 패키지
        ```
    + 패키지 설치 여부 확인
        ```shell
        $ yum list installed 패키지명
        ```
    + 설치가 가능한 모든 패키지 목록 출력
        ```shell
        $ yum list all
        ```
## 31. alias 설정

- 편의성을 위한 명령어 별칭 설정
- 일시적 방법과 영구적인 방법 존재
```
# 터미널 종료 시 설정 초기화 (일시적)
$ alias [별칭]='명령어'

$ alias pf='ps -ef'
alias pf='ps -ef'

# 현재 계정에 영구적으로 alias 설정
$ vi ~/.bashrc

alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
alias pf='ps -ef'

$ source ~/.bashrc
```

- alias 목록

```
$ alias
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l='ls -CF'
alias la='ls -A'
```

- alias 삭제

```
$ unalias [별칭]
$ unalias pf
```

