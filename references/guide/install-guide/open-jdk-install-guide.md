## OpenJDK install GUIDE
1. apt 업데이트
    ```shell
    $ sudo apt-get update && sudo apt-get upgrade
    ```

<br/>

2. openjdk-11-jdk 설치
    ```shell
    $ sudo apt-get install openjdk-11-jdk
    ```

<br/>

3. 환경 변수 설정
    ```shell
    $ vim ~/.bashrc

    # .bashrc 끝 부분에 추가
    export JAVA_HOME=$(dirname $(dirname $(readlink -f $(which java))))
    export PATH=$PATH:$JAVA_HOME/bin
   
   # 환경 변수 설정 적용
   $ source ~/.bashrc
   
    # 환경변수 설정 내용 확인
    $ echo $JAVA_HOME
    ```