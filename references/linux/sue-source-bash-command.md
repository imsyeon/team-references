## source와 ./ (bash) 차이점

### source

- 현재 쉘에서 스크립트 파일을 실행
- source로 실행 시 스크립트 안에서 선언한 환경변수 및 디렉터리 위치 등 현재 실행 중인 쉘에 영향을 줘 환경변수와 위치가 변경됨

### bash

- 새로운 쉘을 만들어 스크립트 파일을 실행
- 셸에서 선언된 변수나 현재 작업 디렉터리(cd를 통한 이동)의 위치는 해당 쉘에서만 유효하며, 쉘이 종료되면 모두 사라짐
- ./과 sh은 모두 동일한 명령어

### 비교 예제

#### 1. PID 출력

```
# 현재 쉘의 PID
# 현재 pid를 출력하기 위해 $$ 변수 사용
$ echo $$
5732
```
```
$ vi test.sh
echo $$
```
```
# source로 실행
$ source test.sh
5732
```
- source로 실행 시 현재 쉘의 PID와 동일
- 즉, echo $$가 실행된 쉘과 동일한 쉘에서 스크립트 내부 명령어가 실행됨

```
# ./ (bash)로 실행
$ ./test.sh 
5806
```
- bash는 sub 쉘이 실행되어 echo $$가 실행한 쉘과 PID가 다름
- 즉, bash로 실행 시 sub 쉘에서 스크립트 내부의 명령어를 실행함

#### 2. cd 경로 유지

```
# /home/ubuntu에 파일 생성
$ vi test.sh
ehco "HERE IS : $PWD"
cd ..
ehco "HERE IS : $PWD"
```
```
# source 명령어 실행 시
$ pwd
/home/ubuntu

$ source test.sh
HERE IS : /home/ubuntu
HERE IS : /home

$ pwd
/home
```
- source test.sh는 현재 작업 디렉터리에 영향을 주어 이동한 것을 확인 할 수 있음

```
# bash 명령어 실행 시
$ pwd
/home/ubuntu

$ ./test.sh
HERE IS : /home/ubuntu
HERE IS : /home

$ pwd
/home/ubuntu
```
- bash test.sh는 스크립트 내부의 디렉터리만 이동하고 현재 디렉터리에는 영향을 주지 않음

> #### 참고자료
>- https://superuser.com/questions/176783/what-is-the-difference-between-executing-a-bash-script-vs-sourcing-it
>- https://www.bangseongbeom.com/source-dot.html
>- https://codingdog.tistory.com/entry/리눅스-sh-vs-source-명령어의-차이를-알아봅시다