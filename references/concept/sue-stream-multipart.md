#  스트림(Stream)

- `데이터의 흐름`을 의미함
- 프로그램에서는 외부에서 데이터를 읽거나 외부로 데이터를 출력하는 작업이 빈번하게 일어나는데, 이때 데이터는 통로를 통해서 이동   
→ 이 통로를 `Stream`이라고 함
- 자바에는 이러한 기능을 수행하기 위해 `InputStream`과 `OutputStream`이 존재
- InputStream
    + 파일 데이터를 읽거나 네트워크 소켓을 통해 데이터를 읽거나 키보드에서 입력한 데이터를 읽을 때 사용
- OutputStream
    + 데이터를 써서 보내는 객체

- 전달 방식에 따라 스트림은 두 가지로 분류
    + 바이트 스트림( Byte Stream )
        - binary 데이터를 입출력하는 스트림
        - 이미지, 동영상 등을 송수신할 때 주로 사용
    + 문자 스트림( Character Stream )
        - 말 그대로 text 데이터를 입출력하는데 사용하는 스트림
        - HTML 문서, 텍스트 파일을 송수신할 때 주로 사용


# Multipart

- HTTP 요청의 한 종류로 HTTP 서버로 파일 혹은 데이터를 보내기 위한 요청 방식
- web client가 요청을 보낼 때, http 프로토콜의 바디 부분에 데이터를 여러 부분으로 나눠서 보냄
- web client가 서버에 파일을 업로드 시 http 프로토콜의 바디 부분에 파일 정보를 담아서 전송   
- 여러 개의 파일을 한 번에 전송 시 body 부분에 파일이 여러 개의 부분으로 연결되어 전송됨
    + 이렇게 여러 부분으로 나눠져서 전송되는 것을 Multipart data
- 보통 파일을 전송 시 사용
- 큰 용량의 바이너리 데이터를 전송하는데 적합
- 보통 Multipart 요청 시 HTTP 메소드는 POST를 사용
- Content-Type :: multipart/form-data
- boundary라는 것이 존재
    + 각 엔티티를 나누는 역할
- 내용의 끝에는 항상 \r\n을 개행(CRLF)으로 사용
- boundary로 나누어진 각 부분을 content-transfer-encoding 헤더에 정의된대로 인코딩이 가능함

---

> 참고 사이트
> - https://velog.io/@jakeseo_me/Multipart-%EC%A0%95%EB%A6%AC
> - https://coding-factory.tistory.com/281
> - https://lannstark.tistory.com/34


# Multipart와 multipart/form-data

## Client → Server 업로드 과정

- 파일 업로드 시 클라이언트가 웹 브라우저라면 form을 통해 파일을 등록해서 전송하게 됨 
- 이때 웹 브라우저가 보내는 HTTP 메세지는 Content-Type 속성이 `multipart/form-data`로 지정되고, 정해진 형식에 따라 메시지를 인코딩하여 전송
- 이를 처리하기 위한 서버는 Multipart 메시지에 대해서 각 파트별로 분리하여 개별 파일의 정보를 얻게 됨
- 이미지 파일을 전송한다고 해서 첨부파일을 보내는 것처럼 png나 jpg 파일 자체가 전송되는 것이 아님
- 이미지 파일도 문자로 이뤄져 있기 때문에 이미지 파일을 문자로 생성하여 HTTP request body에 담아 서버로 전송
