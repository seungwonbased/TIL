# Spring Initializr

[Spring Initializr](https://start.spring.io/)

# Gradle vs Maven

    빌드 관리 도구이다. 프로젝트를 진행할 때 자신이 작성한 코드 뿐만 아니라 많은 라이브러리를 활용하여 개발한다. 이 때 사용되는 수많은 라이브러리들을 관리하는 것이 힘들 때 문제를 해결해 줄 수 있는 도구이다. 내가 사용할 라이브러리 뿐만 아니라 해당 라이브러리가 작동하는 데 필요한 다른 라이브러리까지 관리하여 네트워크를 통해 자동으로 다운로드 해준다. 버전, 종족성 정보를 명시하여 자동으로 관리해준다.

<aside>
💡 Build: 소스 코드 파일을 컴퓨터에서 실행할 수 있는 독립적인 형태로 변환하는 과정과 그 결과       Build = Compile + 그 외 작업

</aside>

## Maven

- 빌드 중인 프로젝트, 빌드 순서, 다양한 외부 라이브러리 종속성 관계를 pom.xml 파일에 명시
- 외부 저장소에서 필요한 라이브러리와 플러그인들을 다운로드 한 다음, 로컬 시스템의 캐시에 모두 저장

## Gradle

- Groovy 언어를 사용하여 xml 파일을 설정 파일로 사용하는 Maven보다 코드가 간결
- 큰 규모로 예상되는 multi-project 빌드를 도울 수 있음
- 업데이트가 이미 반영된 빌드의 부분은 재실행되지 않아 빌드 시간이 단축될 수 있음

## 비교

### Maven

- 고정적이고 선형적인 단계의 모델
- 멀티 프로젝트에서 특정 설정을 다른 모듈에서 사용하려면 상속을 받아야 함

### Gradle

- 작업 의존성 그래프 기반 모델
- 빌드 시간 단축
- 설정 주입 방식
- Concurrent에 안전한 캐시 사용

# JAR vs WAR

    Java의 JAR 툴을 이용하여 생성된 압축 (아카이브) 파일이며 애플리케이션을 쉽게 배포하고 동작시킬 수 있도록 관련 파일(리소스, 속성 파일 등)들을 패키징(압축)해주는 것이 주 역할이다.

## JAR (**J**ava Archive)

- 자바 프로젝트를 압축한 파일
- Class와 같은 자바 리소스와 속성 파일, 라이브러리 및 액세서리 파일이 포함
- .jar 파일은 플랫폼에 귀속되는 점만 제외하면 .zip 파일과 동일한 구조
- JDK(Java Development Kit)에 포함하고 있는 JRE(Java Runtime Environment)만 가지고도 실행 가능
- 주로 Jar를 사용

## WAR (Web Application Archive)

- Servlet, JSP 컨테이너에 배치할 수 있는 웹 애플리케이션 압축 파일
- JSP, servlet, jar, class, xml, html, javascript 등 servlet context 관련 파일로 패키징 되어있음
- 웹 응용 프로그램을 위한 포맷이므로 웹 관련 자원만 포함하고 있음
- 이를 사용하면 웹 애플리케이션을 쉽게 배포하고 테스트할 수 있음
- WEB-INF 및 META-INF 디렉토리로 사전 정의된 구조를 사용하며 .war 파일을 실행하려면 Tomcat, Weblogic, Websphere 등의 웹 서버 또는 웹 컨테이너(WAS)가 필요
- WAR 파일도 자바의 JAR 옵션(java -jar)을 이용해 생성하는 JAR 파일의 일종으로 웹 애플리케이션 전체를 패키징하기 위한 JAR 파일로 여기면 됨
- → Tomcat 등이 설치된 별도의 서버에 .war를 넣어서 쓸 때 사용
- JSP를 쓸 때 사용

# Group ID, Artifact ID, Name

## Group ID

- 일반적으로 작성하는 회사의 도메인 명을 거꾸로 씀
    
    → com.naver
    
- Package 네이밍 룰을 따름