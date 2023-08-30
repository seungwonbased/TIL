# Servlet

# Servlet이란?

- 클라이언트의 요청을 처리하고, 그 결과를 반환하는 Servlet 클래스의 구현 규칙을 지킨 자바 웹 프로그래밍 기술
    - 자바를 사용하여 웹을 만들기 위해 필요한 기술
    - 클라이언트의 요청에 대한 결과를 전송하는 자바 프로그램

# 특징

- urlPatterns(/XXX)의 URL이 호출되면 서블릿 코드 실행
- HTTP 요청 정보를 편리하게 사용할 수 있는 HttpServletRequest
- HTTP 응답 정보를 편리하게 제공할 수 있는 HttpServletResponse
- 개발자는 이를 통해 HTTP 스펙을 매우 편리하게 사용
    
    ![servlet](https://github.com/seungwonbased/TIL/blob/main/Spring/assets/servlet1.png)
    
    - HTTP 요청 시 응답 흐름
        - WAS는 Request, Response 객체를 새로 만들어 Servlet 객체 호출
        - 개발자는 Request 객체에서 HTTP 요청 정보를 편리하게 꺼내 사용
        - 개발자는 Response 객체에 HTTP 응답 정보를 편리하게 입력
        - WAS는 Response 객체에 담겨있는 내용으로 HTTP 응답 정보를 생성

# Servlet container

- Servlet을 관리해주는 컨테이너
- Servlet이 어떠한 역할을 수행하는 정의서라면 Servlet container는 그 정의서를 보고 수행
- 클라이언트의 요청을 받아주고 응답할 수 있도록 웹 서버와 소켓으로 통신
- Tomcat처럼 Servlet을 지원하는 WAS를 Servlet container라고 함
- Servlet 객체를 생성, 초기화, 호출, 종료하는 생명주기 관리
- Servlet 객체는 Singleton으로 관리
    - 요청이 올 때마다 새로운 객체를 계속 생성하는 것은 비효율적
    - 최초 로딩 시점에 Servlet 객체를 미리 만들어두고 재활용
    - 모든 고객 요청은 동일한 서블릿 객체 인스턴스에 접근
    - **공유 변수 사용 주의**
    - 서블릿 컨테이너 종료 시 함께 종료
- JSP도 Servlet으로 변환되어서 사용
- 동시 요청을 위한 멀티 쓰레드 처리 지원
- 역할
    - 웹 서버와 통신 지원
    - 서블릿 생명 주기 관리
    - 멀티 쓰레드 지원 및 관리
    - 선언적인 보안 관리

# Servlet 환경 구성

```java
@ServletComponent // 스프링 부트는 서블릿을 직접 등록해서 사용할 수 있도록 지원
```

```java
@WebServlet(name = 서블릿 이름, urlPatterns = URL 매핑) // 서블릿 애노테이션

// HTTP 요청을 통해 매핑된 URL이 호출되면 서블릿 컨테이너는 다음 메서드 실행
protected void servlet(HttpServletRequest request, HttpServletResponse response)
```

# HTTP 요청 메시지 로그로 확인

- 다음 설정을 추가

```java
logging.level.org.apache.coyote.http11=debug
```

- 운영 서버에 이와 같이 모든 요청 정보를 다 남기면 성능 저하 발생 가능
- 개발 단계에서만 사용하자

# HttpServletRequest

- HTTP 메시지의 Start-line, header 등의 정보 조회