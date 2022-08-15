# .gitignore
## 1. 사용법
```shell
# 확장자가 .js 인 파일은 무시
*.js

# .js 파일들은 모두 무시되지만, test.js만은 무시하지 않음
!test.js

# 현재 디렉토리에 있는 /test.js파일은 무시되지만,
# subDir/test.js 같이 특정 디렉토리 하위에 있는 test.js는 무시되지 않음
/test.js

# node_modules/ 디렉토리에 있는 모든 파일을 무시
node_modules/

# src/ 하위의 .js파일만 무시
src/*.js

# src/ 하위에 존재하는 모든 디렉토리의 .txt 파일을 무시
src/**/*.txt

# 현재 디렉토리와 그 하위 디렉토리 내에 존재하는 모든 .js 파일을 무시
/**/*.js

# 현재 디렉토리 내에 존재하는 모든 .js .ts 파일 무시
/*.{js, ts}

# 현재 디렉토리 내에 있는 ex1.js ex2.js ex3.js 파일 무시
/ex[1-3].js
```

<br />

## 2. remote에 이미 올라간 파일에 ignore 적용하기
```shell
$ git rm -r --cached .
$ git add .
$ git commit -m "[커밋메시지]"
$ git push [remote] [branch]
```