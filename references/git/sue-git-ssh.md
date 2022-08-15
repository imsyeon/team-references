# git ssh

- private repository를 clone 하기 위해 ssh 기반 인증방식 사용 
- ssh 방식으로 clone 하는 순서
  1. 키생성
     - ssh key 조회

       ```shell
       $ cd ~/.ssh
       $ ls
       ...
       ```
     - ssh key 생성
       ```shell
       $ ssh-keygen -t ed25519 -C "your_email@example.com"
       ```

     - 비밀번호 지정
       - 원하지 않을 시 엔터 입력
         ```shell
         Enter passphrase (empty for no passphrase):
         Enter same passphrase again:
         ```
     - /Users/lainyzine/.ssh/id_ed25519
       - 공개키는 /Users/lainyzine/.ssh/id_ed25519.pub에 저장
       - 개인키는 절대 공개되어서는 안 됨
         ```shell
         Your identification has been saved in /Users/lainyzine/.ssh/id_ed25519.
         Your public key has been saved in /Users/lainyzine/.ssh/id_ed25519.pub.
         The key fingerprint is:
         SHA256:MRR2EWPjezAxwlrxCgfqjq43CJjtZVl8Heo5YU/912I lainyzine.com@gmail.com
         The key's randomart image is:
         +--[ED25519 256]--+
         |      ..*oXo     |
         |     . +o*.=     |
         |    ...o+o=o     |
         |   .  +o=+o+.    |
         |.o  .o +S=. ..  .|
         |+ .o+   + ..  E o|
         |.o.o.    .   . o |
         |..+              |
         |.o..             |
         +----[SHA256]-----+
         ```
  2. 키등록
     - 공개키 GitHub에 등록
       ```shell
       # macOS
       $ pbcopy < ~/.ssh/id_ed25519.pub
 
       # Windows
       $ clip < ~/.ssh/id_ed25519.pub
       ```
     - GitHub에 공개키 등록
       - https://github.com/settings/ssh/new

     - ssh key 등록 성공 여부 확인
       ```shell
       $ ssh -T git@github.com
       ```
  3. clone
     ```shell
     $ git clone git@github.com:lainyzine/example-private.git example-private-2
     ```