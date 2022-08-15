# Git command

## 1. 기본 사용법

- 새로운 로컬 저장소를 생성하기
  ```shell
  $ git init [project_name]
  ```
- 저장소 가져오기
  ```shell
  $ git clone [url]
  ```
- 변경(수정)된 staged 파일 보기
  ```shell
  $ git diff
  ```
- 로그마다 변경 사항 확인
  ```shell
  $ git log -p
  ```
- 명령 히스토리
  ```shell
  $ git reflog
  ```
- 명령상태 복구
  ```shell
  $ git reset --hard [돌아가고 싶은 sha1 번호]
  ```

<br />

## 2. branch
- 브랜치 생성하기
  ```shell
  $ git branch [브랜치 이름]
  ```
- 브랜치 목록 확인
  ```shell
  ## local 브랜치 확인
  $ git branch
  
  ## remote 브랜치 확인
  $ git branch -r
  ```
- 브랜치 변경하기
  ```shell
  $ git checkout [브랜치 이름]
  ```
- remote 브랜치 최신화
  ```shell
  $ git fetch -p
  ```
- 브랜치 접속
  ```shell
  ## local 브랜치 접속
  $ git checkout [브랜치 이름]
  
  ## remote 브랜치 접속
  $ git checkout -t [브랜치 이름]
  
  ## 브랜치 만들면서 접속
  $ git checkout -b [브랜치 이름]
  ```
- 브랜치 삭제하기
  ```shell
  $ git branch -d [브랜치 이름]
  
  ## remote 브랜치 삭제
  $ git push origin --delete [브랜치 이름]
  
  ## 유효하지 않은 remote 브랜치 삭제
  $ git remote prune origin
  또는
  $ git remote update --prune
  ```

<br />

## 3. stash
- 변경사항 임시저장
  ```shell
  $ git stash
  ```
- 임시 저장된 변경사항 복원
  ```shell
  $ git stash pop
  ```
- 임시저장 목록 확인
  ```shell
  $ git stash list
  ```
- stash 적용
  ```shell
  ## 가장 최근 stash를 가져와 적용
  $ git stash apply [stash ID]
  ```
- stash 삭제
  ```shell
  $ git stash drop [stash ID]
  ```

<br />

## 4. status
- 변경된 파일 상태 확인
  ```shell
  $ git status
  ```
  
<br />

## 5. add
- 저장소에 추가할 파일 선택(스테이징)
  ```shell
  $ git add [파일명]
  ```
- add 취소
  + 파일 상태를 Unstage로 변경
  ```shell
  ## 모든 add 파일 취소
  $ git reset
  
  ## 특정 add 파일 취소
  $ git reset HEAD -- [파일명]
  ```
  
<br />

## 6. commit
- 파일의 실제 변경사항 확정
  ```shell
  $ git commit -m "[커밋 메시지]"
  ```
- 스테이징과 커밋, 메시지를 동시에 진행
  ```shell
  $ git commit -am "message"
  ```
- commit 메시지 수정
  ```shell
  $ git commit --amend
  ```
- push 후 commit 메시지 수정(택 1)
  + 마지막 커밋 메시지 변경
    ```shell
    $ git commit --amend -m "원하는 변경 내용 입력 하기"
    $ git push origin [브랜치명] -f # 변경 내용 강제 push
    ```
  + git rebase 이용
    ```shell
    $ git rebase -i HEAD~[거슬러 올라갈 커밋의 수]
    ```
      - 사용 예시
        ```shell
        $ git rebase -i HEAD~1
        ```
        > 1. 변경하길 원하는 커밋 `i`로 입력모드에 진입
        > 2. pick을 reword로 변경
             ![gitcommit](https://user-images.githubusercontent.com/86212069/136512490-701ae84d-8402-4fc4-b711-64f3ffa253e1.png)
        > 3. 편집창 빠져나가기 (`esc` > `:wq`) </br>
             편집창을 빠져나가면 기존 커밋 메시지를 수정하는 창으로 전환됨
        > 4. 기존 커밋 메시지를 `i`로 입력모드에 들어가 변경
        > 5. 편집창 빠져나가기(`esc` > `:wq`)
        ```shell
        ## 변경 내용 강제 push
        ## 강제 push 해야 수정된 커밋 메시지 반영 가능
      
        $ git push origin [브랜치명] -f
        또는
        $ git push origin [브랜치명] --force-with-lease
        ```
- commit 취소
  ```shell
  ## commit을 취소하고 해당 파일들을 staged 상태로 워킹 디렉토리에 보존
  $ git reset --soft "HEAD^"
  
  ## commit을 취소하고 해당 파일들을 unstaged 상태로 워킹 디렉터리에 보존. 즉, unstaged 상태
  $ git reset ---mixed "HEAD^"
  ```

<br />

## 7. Git config 설정
- 사용자 정보 입력
  ```shell
  $ git config --global user.name "홍길동"
  $ git config --global user.email "support@webisfree.com"
  ```
- 사용자 정보 확인
  ```shell
  $ git config user.name
  $ git config user.email
  ```
- 삭제
  ```shell
  $ git config --unset --global user.name
  $ git config --unset --global user.email
  ```
- 리스트 확인
  ```shell
  $ git config --list
  ```
