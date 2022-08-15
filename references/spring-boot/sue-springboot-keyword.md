# Spring boot 관련 keyword

## 1. Annotation
### @Controller와 @RestController
- 차이점
    - HTTP Response Body가 생성되는 방식의 차이
        - @RestController 적용 시 모든 메소드가 뷰 대신 객체로 작성
### @Controller
- 전통적인 Spring MVC의 컨트롤러
- 주로 view를 반환하기 위해 사용

### @RestController
- Restful 웹서비스의 컨트롤러
- @Controller에 @ResponseBody가 결합된 어노테이션
- 객체만을 반환
    - 반환 되는 객체 데이터 타입 : JSON/XML 타입의 HTTP 응답을 직접 리턴
- @RestController 사용 시 @ResponseBody 생략


### @ResponseBody와 @RequestBody
- 스프링에서 비동기 처리 시 사용
    - 비동기 클라이언트 서버 통신을 위해서 데이터를 전송하게 됨
    - JSON 형식으로 서버와 클라이언트가 서로에게 전송

### @ResponseBody
- VO 객체를 JSON으로 바꿔서 HTTP body에 담는 스프링 어노테이션
- 메서드의 return 값을 HTTP Response의 body에 담는 역할
    - 요청한 형태에 맞춰서 메시지 변환기를 통해 결과값을 반환
    
### @RequestBody
- HTTP 요청 Body를 자바 객체로 전달받음
- HTTP 요청의 body 내용을 자바 객체로 매핑하는 역할

---

## 2. 외부 설정 파일
### application.properties와 application.yml
- Spring Boot의 일반적인 관행은 외부 구성을 사용하여 속성을 정의함
- 다른 환경에서 동일한 애플리케이션 코드 사용 가능

### application.properties
-  key-value 형식
- 값 내에서 $ {} 구문 과 함께 자리 표시자를 사용하여 다른 키, 시스템 속성 또는 환경 변수의 내용을 참조

```
app.name=MyApp
app.description=${app.name} is a Spring Boot application
```

- List 표현 가능
```
application.servers[0].ip=127.0.0.1
application.servers[0].path=/path1
application.servers[1].ip=127.0.0.2
application.servers[1].path=/path2
```

### application.yml
- 계층 구조
- prefix의 중복 제거 가능
- 가독성이 좋음

```
application:
    servers:
    -   ip: '127.0.0.1'
        path: '/path1'
    -   ip: '127.0.0.2'
        path: '/path2'
```

- @Value 사용하여 값 주입
```
public class ExternalService{
    @Value("${external.record-year}")
    private String recordYear;

}
```

- Envirionment Abstraction (환경 추상화)
    - 환경 API를 사용하여 속성 값을 얻을 수 있음
    
```
@Autowired
private Environment env;

public String getSomeKey(){
    return env.getProperty("key.something");
}
```

- ConfigurationProperties 어노테이션
    - @ConfigurationProperties를 사용하여 속성을 형식이 안전한 구조화 된 개체에 바인딩

```
@ConfigurationProperties(prefix = "mail")
public class ConfigProperties {
    String name;
    String description;
```