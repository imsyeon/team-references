# REST API
## 1. REST(Representational State Transfer)
+ 월드 와이드 웹과 같은 분산 하이퍼미디어 시스템을 위한 소프트웨어 아키텍처의 한 형식
+ 자원을 이름(자원의 표현)으로 구분하여 해당 자원의 상태(정보)를 주고 받는 모든 것
    - HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시
    - HTTP Method를 통해 해당 자원에 대한 CRUD Operation을 적용
+ 구성 요소
    - 자원(Resource) : URI
      ```html
      http://restapi.example.com/houses/apartments
      ```
    - 행위(Verb) : HTTP Method
    - 표현(Representation of Resource) : 형식 ex) JSON, XML 등 
        + JSON 예시
          ```json
          {
              "orderID":3,
              "productID":2,
              "quantity":4,
              "orderValue":16.60,
              "links": [
                  { "rel":"product", "href":"https://adventure-works.com/customers/3", "action":"GET" },
                  { "rel":"product", "href":"https://adventure-works.com/customers/3", "action":"PUT" }
              ]
          }
          ```
      + XML 예시
        ```xml
        <Booking>
            <firstname>Jim</firstname>
            <lastname>Brown</lastname>
            <bookingdates>
                <checkin>2018-01-01</checkin>
                <checkout>2018-01-03</checkout>
            </bookingdates>
        </Booking>
        ```
+ "RESTful" → REST 아키텍처 원칙을 모두 만족하는 것
<br/>

## 2. REST API(Representational State Transfer API)
+ REST 기반으로 서비스 API를 구현한 것
    - API(Application Programming Interface)
        - 데이터와 기능의 집합 제공 및 정보 교환 가능하도록 하는 것
<br/>

## 3. HTTP Method
+ 자원에 대한 행위를 나타내는 Method는 CRUD(Create/Read/Update/Delete)에 각각 매칭됨

|메소드|설명|CRUD|
|:---:|:---|:---|
|POST|현재 리소스(Collection)보다 한 단계 아래의 리소스(Document) 생성|Create|
|GET|현재 리소스(Collection, Document) 조회|Read|
|PUT|현재 리소스(Document)의 정보 수정(전체)|Update|
|PATCH|현재 리소스(Document) 수정(일부)|Update|
|DELETE|현재 리소스(Document) 삭제|Delete|

- 예시
```html
<!-- POST 메소드로 '뽀로로'라는 이름을 가진 학생을 생성하기 위한 요청 -->
HttpRequest
POST /student
{
  “name”: “뽀로로”,
  “grade”: 1
}

<!-- 응답 : 고유 구분값이 id가 1로 설정되어 '뽀로로'라는 학생 생성 -->
HttpResponse
HTTP/1.1 200 OK
{
  “id”: 1,
  “name”: “뽀로로”,
  “grade”: 1
}
```
<br/>

## 4. POST vs PUT
| |POST|PUT|
|:---:|:---|:---|
|역할|리소스 생성|리소스 생성 및 수정|
|요청 시|매번 새로운 리소스 생성|매번 같은 리소스 반환|
|멱등성|X|O|
|Resource Identifier|X|O|
|리소스 결정권|서버|클라이언트|

- 예시
```html
<!-- POST 메서드로 2번 요청 -->
POST /student
{
    “name”: “뽀로로”,
    “grade”: 1
}

<!-- 결과 : id가 1과 2인 뽀로로가 두 개 생성 -->
HTTP/1.1 200 OK
{ “id”: 1, “name”: “뽀로로”, “grade”: 1 }

HTTP/1.1 200 OK
{ “id”: 2, “name”: “뽀로로”, “grade”: 1 }
```
```html
<!-- PUT 메서드로 2번 요청 -->
PUT /student/3
{
    “name”: ”에디”
    “grade”: 2
}

<!-- 결과 : 리소스가 존재하지 않으면 최초 생성 후 같은 리소스 반환  -->
HTTP/1.1 200 OK
{ “id”: “3”, “name”: “에디”, “grade”: 2 }
```
