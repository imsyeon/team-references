# NextCloud API call guide

- NextCloud는 WebDAV api를 제공

] WebDAV란?
] 
] - 웹 분산 저작 및 버전 관리 (Web Distributed Authoring and Versioning)
] - 웹을 통하여 웹서버에 파일을 관리(목록 조회, 수정, 삭제, 이동 등)할 수 있는 확장된 하이퍼텍스트(HTTP) 전송 프로토콜
] - WebDAV에서 확장된 메소드(Method)
]     + HEAD, GET, PUT, POST 등의 기본 HTTP 메소드 외에 WebDAV에는 확장된 다음 메소드들이 존재함
]     + PROPFIND
]         - 리소스를 위한 속성을 검색
]         - ex) 파일 목록과 속성 검색
]     + MKCOL
]         - 새로운 컬렉션을 생성
]         - ex) 폴더 생성
]     + COPY
]         - 리소스와 컬렉션을 복사
]         - ex) 파일 복사
]     + MOVE
]         - 리소스와 컬렉션을 이동
]         - ex) 파일 이동
]     + LOCK, UNLOCK　
]         - 리소스에 overwrite 방지를 위해 락을 걸거나 해제
]     + OPTIONS
]         - 서버가 지원하는 메소드 출력
]     + DELETE
]         - 리소스와 컬렉션을 삭제 
]         - ex)파일 삭제

## 1. WebDAV 주소 확인 방법

- NextCloud 접속 ] 설정 ] WebDAV 확인

    ![스크린샷 2022-02-10 오후 12 53 39](https://user-images.githubusercontent.com/86212069/153340347-21197348-d4c0-47dc-a22f-df73df380ec2.png)

## 2. 파일 조회

- [user] :: 사용자 이름
- [pass] :: 비밀번호
- [nextcloud root] ::  nextcloud 루트 경로 ex) https://abc.com/nextcloud
- [folder] :: 파일이 위치한 폴더

```
# termianl
# 해당 폴더 내의 파일 정보 확인
$ curl -u [user]:[pass] 'http://[nextcloud root]/remote.php/dav/files/[user]/[folder]' -X PROPFIND

$ curl -u admin:admin 'http://15.164.221.83/nextcloud/remote.php/dav/files/admin/new-test' -X PROPFIND
```

```
# postman
PROPFIND remote.php/dav/files/[user]/[folder]
```

## 3. 파일 업로드

- [file] :: 업로드할 파일명
- postman file 전송 시 working dir 설정
    + 참고 :: https://growingsaja.tistory.com/468

```
# terminal
$ curl -X PUT -u [user]:[pass] 'http://[nextcloud root]/remote.php/dav/files/[user]/[file]

$ curl -X PUT -u admin:admin 'http://15.164.221.83/nextcloud/remote.php/dav/files/admin/new-test/test.png' 
```
```
# postman
PUT remote.php/dav/files/[user]/[file]
```


## 4. 파일 다운로드

```
# terminal
$ curl -X GET -u [user]:[pass] https://[nextcloud root]/remote.php/dav/files/[user]/[path of file to download] --output [path to save]

$ curl -X GET -u admin:admin 'http://15.164.221.83/nextcloud/remote.php/dav/files/admin/new-test/spring-music.war' --output ~/workspace/spring-music.war
```
```
# postman
GET remote.php/dav/files/[user]/[file]
```

## 폴더 생성

- 해당 `MKCOL` 메소드는 postman에서 실행 시 custom 필요
    + 참고 :: https://blog.postman.com/custom-http-methods-more-flexibility-and-autonomy/

```
# terminal
$ curl -X MKCOL -u [user]:[pass] https://[nextcloud root]/remote.php/dav/files/[user]/[folder path to create]

$ curl -X MKCOL -u admin:admin 'http://15.164.221.83/nextcloud/remote.php/dav/files/admin/create-test'
```
```
# postman
MKCOL remote.php/dav/files/[user]/[new folder]
```

## 폴더 삭제

```
# terminal
$ curl -X DELETE -u [user]:[pass] https://[nextcloud root]/remote.php/dav/files/[user]/[folder path to delete]

$ curl -X DELETE -u admin:admin 'http://15.164.221.83/nextcloud/remote.php/dav/files/admin/create-test'
```

```
# postman
DELETE remote.php/dav/files/user/path/to/file
```
---
> ### 참고 사이트
> - https://docs.nextcloud.com/server/latest/developer_manual/client_apis/WebDAV/basic.html#webdav-basics
> - https://linuxfun.org/en/2021/07/02/nextcloud-operation-api-with-curl-en/