# IntelliJ IDEA issue

## 1. Spring Boot multi module
### 1.1 jsp 인식 못하는 오류
> 프로젝트는 실행되지만 웹 브라우저에 view 출력 안 됨

- 원인 및 분석
   + IntelliJ에서 module 안에 web module을 실행시킬 경우 working directory를 해당 모듈로 설정해야 함
   + working directory를 따로 설정하지 않을 경우, working directory로 **최상위 프로젝트**가 지정됨

- 조치
   1. 실행 중인 프로젝트가 있는 경우 모두 종료
   2. `Run/Debug Configuration` > `Edit Configuration`
      ![module](https://user-images.githubusercontent.com/74666378/137204774-3ecca5e2-8db6-47fb-bbcf-e1f8ba007ea6.jpg)
   3. Add New Configuration(`Alt` + `Insert`) > `Spring Boot` >`Configuration` > `Environment` > `Working directory`
      - 각 모듈에 working directory 제대로 지정 되어있는지 확인
        ![module2](https://user-images.githubusercontent.com/74666378/137206661-7db046fe-85a6-4cbc-a949-c6b614c48aea.png)
      - 지정 안 되어 있을 경우 찾아서 지정 후 `Apply` > `OK`