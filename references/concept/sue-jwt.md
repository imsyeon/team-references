# JWT

- 토근 방식으로 로그인 상태를 유지하는 방법
- 웹 토큰, JSON Web Token의 약자
- JWT는 .을 기준으로 세 파트로 구분
  + Header
  + payload
  + signature

![image](https://user-images.githubusercontent.com/86212069/167978189-683933e3-f798-4493-9d12-81845443b765.png)

- **Header**
  + typ와 alg 정보 존재
  + JWT를 어떻게 검증하는지에 대한 내용을 담고 있음
  + 토큰 타입, 어떤 암호화 알고리즘인지에 대한 정보를 포함
  + 해시 알고리즘의 이름을 적을 수 있음
  
```
{
	"alg": "HS256", 
	"typ": JWT 
}
```

- **payload**
  + 토큰에 담을 정보를 갖고 있음
  + 클라이언트의 고유 ID 값 및 유효 기간 등이 포함
  + 이 정보의 조각을 `클레임(claim)`이라고 하며, key-value 쌍으로 이루어져 있음
  + 여러 개의 클레임을 담을 수 있으며 클레임을 공개 혹은 비공개, 등록할 것인지 결정할 수 있음

- **signature**
  + 인코딩 된 Header와 Payload를 더한 뒤 secret key로 해싱하여 생성
  + 토큰을 인코딩하거나 유효성 검사 시 필요한 암호화 코드
  
  ### 인증 과정

  1. 클라이언트 로그인 요청 시 서버에서 회원 정보를 검증 후 클라이언트 고유 ID 등 정보를 Payload에 담음
  2. 암호화 할 secret key를 사용하여 Access Token(JWT) 발급 후 클라이언트에 전달
  3. 클라이언트는 전달받은 토큰을 쿠키나 local storage에 저장
  4. 클라이언트는 서버에 요청 시 저장해 둔 토큰을 포함시켜 함께 전달
  5. 서버는 토큰의 Signature를 secret key로 복호화한 다음, 위변조 여부 및 유효 기간 등을 확인
  6. Access Token이 만료 시 다시 로그인을 요청하며, 회원 정보에 맞는 Refresh token을 확인
  7. Refresh token이 유효하면 새로운 Access token을 발급해 클라이언트에 응답 보냄