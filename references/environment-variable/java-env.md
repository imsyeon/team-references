# JAVA Environment Variable

## 1. 환경변수란?
- 환경변수란 프로세스가 컴퓨터에서 동작하는 방식에 영향을 미치는, 동적인 값들의 모임 
- 운영체제가 참조하는 변수

<br />

## 2. JRE(Java Runtime Environment)
- 자바 가상 머신(Java Virtual Machine), 자바 클래스 라이브러리, 자바 명령 및 기타 인프라를 포함한 컴파일 된 Java 프로그램을 실행하는데 필요한 패키지
- JVM이 자바 프로그램을 동작 시킬 때 필요한 라이브러리 파일들과 기타 파일들을 가지고 있음
- JVM의 실행환경을 구현함
- Servlet과 JSP 등을 실행 시켜주는 일종의 엔진
- .jre 폴더 및 파일
  |항목|설명|
  |:---:|:---|
  |**bin/**|• java 실행 프로그램이 포함<br>• JVM을 시작하는 java가 포함<br>• keytool 및 policytool과 같은 유틸리티도 포함|
  |**conf/**|사용자가 편집할 수 있는 구성파일 존재|
  |**lib/**|• 여러가지 supporting 파일이 존재<br>• jar 구성파일, 속성 파일, 글꼴, 번역, 인증서 등<br>• Java 표준 라이브러리의 .class 파일을 포함하는 모듈이 존재|

<br />

## 3. JDK(Java Development Kit)
- Java를 사용하기 위해 필요한 모든 기능을 갖춘 Java용 SDK(Software Development Kit)
- JRE에 있는 모든 것, 컴파일러(javac), jdb, javadoc과 같은 도구들을 포함
- JDK를 설치 시 JRE도 같이 설치됨 (**JDK = JRE + @** )
- 폴더 및 파일
  |항목|설명|
  |:---:|:---|
  |**bin/**|JDK에 포함된 모든 개발 도구에 대한 실행 명령어 파일 존재|
  |**lib/**|개발 도구에 사용하는 파일(jar파일, JDK의 도구 및 유틸리티) 존재|

<br />

## 4. JRE_HOME
- java를 실행만 하는 환경에서 JDK없이 JRE의 경로를 지정하기 위해 사용
  + JDK가 설치되어 있을 시 JDK 내부의 JRE 사용 가능
- JRE 작동 시 JRE_HOME을 찾음
  + JDK가 설치되어 있어도 JRE만 필요한 프로그램은 JAVA_HOME을 찾는 대신 JRE_HOME을 찾기도 함

<br />

## 5. JAVA_HOME
+ Java Development Kit(JDK) 또는 JRE(Java Runtime Environment)를 설치한 후 선택적으로 설정할 수 있는 운영 체제(OS) 환경 변수
+ JAVA_HOME 환경변수는 JDK 또는 JRE가 설치된 파일 시스템 위치
+ Windows, Ubuntu, Linux, Mac 및 Android를 포함하여 Java를 설치한 모든 OS에 구성되어야 함
+ JAVA_HOME 변수를 올바르게 구성해야 하는 프로그램들
  - Eclipse, NetBeans 및 Android 스튜디오
  - Apache Tomcat 및 WebSphere Portal
  - JProfiler 및 Java Mission Control
  - 메이븐과 ANT
  - Gradle과 Groovy
  - Jenkins 및 Hudson CI 도구
  - exe4j
+ 일반적으로 JDK 지정(JDK 작동 시 JAVA_HOME을 찾음)

<br />

## 6. PATH 변수
- PATH를 사용하면 \bin 디렉토리가 추가되어 JDK 또는 JRE에 패키지 된 다양한 Java 유틸리티를 모두 명령줄의 어디에서나 사용 가능
- 자주 쓰는 프로그램의 경로를 PATH에 등록시켜 두면 그 경로에 존재하는 프로그램을 어떠한 장소에서든 실행시킬 수 있도록 해 줌
- 프로그램이 JAVA_HOME 변수를 찾을 수 없는 경우 PATH를 통해 사용할 수 있는 유틸리티를 확인하여 Java 런타임 도구에 계속 액세스함