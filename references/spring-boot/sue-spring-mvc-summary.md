# MVC

- 디자인 패턴
- Model, View, Controller를 의미
- MVC 패턴의 사용 목적
    - 서로 분리되어 각자의 역할에 집중
    - 유지보수, 애플리케이션의 확장성, 유연성 증가
    - 코드 중복을 방지

## 1. Model
- 동작을 수행하는 코드
- 모델의 상태 변화가 있을 때 Controller와 View에 통보하거나 직접 Model의 상태를 읽어오기도 함
- DB와 상호작용하며 비즈니스 로직을 처리하는 모듈

## 2. View
-  Client에게 보여지는 결과 화면을 반환하는 모듈
- Controller를 통해 Model에 질의를 보내고, 그 값을 사용자에게 적절하게 보여줌
- Model이나 Controller에 대한 정보를 알면 안 되며 단순히 표시해주는 역할

## 3. Controller
- Client 요청이 들어왔을 때 그 입력을 처리
- 어떤 로직을 실행시킬 것인지 Model과 View를 연결해주며 제어하는 모듈
- Model 또는 View에 대한 정보를 알아야 함
