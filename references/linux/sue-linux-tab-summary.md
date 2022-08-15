## 1. Linux tab 자동 완성 기능
- 디렉토리에 있는 파일이나 디렉토리의 첫 번째 문자만 입력 후 'tab' 키를 누르면 나머지 글자가 자동 완성됨
- 'tab'을 두 번 연속 누르면 디렉토리나 파일 리스트 확인 가능
    + 첫 번째 문자 입력 후 tab을 두 번 입력 시 해당 문자로 시작하는 파일이나 디렉토리 리스트 확인 가능

<br />

## 2. Linux tab 자동 완성 기능 설정
- linux 최소 설치 시 tab이 안 먹는 경우가 있음
- 별도의 패키지 설치 필요</br>
    1. bash-completion 설치
       ```shell
       # 데비안, 우분투 계열 apt-get 사용
       $ apt-get install bash-completion
       ```
       또는
       ```shell
       # 레드햇 계열 yum 사용
       $ yum install bash-completion
       ```
    2. 적용
       ```shell
       $ source /etc/bash_completion
       ```