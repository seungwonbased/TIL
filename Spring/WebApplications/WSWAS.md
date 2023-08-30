# Web Server, Web Application Server

# Web - HTTP 기반

- 모든 것을 HTTP 메시지로 전송
    - HTML, text
    - Image, audio, video, file
    - JSON, XML (API)
    - 거의 모든 형태의 데이터 전송 가능
- 서버 간에 데이터를 주고 받을 때도 대부분 HTTP 사용

# Web Server

- HTTP를 기반으로 동작
- **정적 리소스** 제공, 기타 부가기능
    - 정적 HTML, CSS, JS, image, video

# Web Application Server (WAS)

- HTTP를 기반으로 동작
- 웹 서버 기능 포함 (정적 리소스 제공 가능)
- 프로그램 코드를 실행해서 애플리케이션 로직 수행
    - 동적 HTML, HTTP API (JSON)
    - Servlet, JSP, Spring MVC
    - 예: Tomcat, Jetty, Undertow, …

# Web Server vs WAS

- 웹 서버는 정적 리소스(파일), WAS는 애플리케이션 로직
- 용어와 경계가 사실 모호함
    - 웹 서버에도 프로그램을 실행하는 기능을 포함하기도 함
    - WAS 또한 웹 서버의 기능 제공
- Servlet container 기능을 제공하면 WAS
- WAS는 애플리케이션 코드를 실행하는데 더 특화

# 웹 시스템 구성 - WAS, DB

![wswas](https://github.com/seungwonbased/TIL/blob/main/Spring/assets/wswas1.png)

- 정적 리소스는 웹 서버가 처리
- 웹 서버는 애플리케이션 로직과 같은 동적인 처리가 필요하면 WAS에 요청을 위임
- WAS는 중요한 애플리케이션 로직 처리
- 효율적인 리소스 관리
    - 정적 리소스가 많이 사용되면 웹 서버 증설
    - 애플리케이션 리소스가 많이 사용되면 WAS 증설
- 웹 서버는 잘 죽지 않음: 정적 리소스만 제공하기 때문에
- WAS는 잘 죽음: 애플리케이션 로직이 동작
- WAS나 DB에 장애가 발생 시 웹 서버에서 오류 화면 HTML 제공 가능