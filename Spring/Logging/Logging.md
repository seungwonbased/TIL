# Logging

<aside>
💡 운영 시스템에서는 ‘System.out.println()’ 같은 시스템 콘솔을 사용해서 정보를 출력하지 않고 별도의 로깅 라이브러리를 사용하여 로그를 출력

</aside>

- ‘System.out.println()’에 비해 로그는 로그 레벨에 따라 상황에 맞게 출력을 조절할 수 있음
- 파일이나 네트워크 등 로그를 별도의 위치에 남길 수 있음
    - 특히 파일로 남길 때는 일별, 특정 용량에 따라 로그를 분할하는 것도 가능
- 성능도 더 좋음! (내부 버퍼링, 멀티 쓰레드 등)
- 그래서 실무에서는 꼭 로그를 사용하자!

# Logging library

## SLF4J

[SLF4J](http://www.slf4j.org)

- 로그 라이브러리는 Logback, Log4J2 등 수많은 라이브러리가 있는데, 그것을 통합해서 인터페이스로 제공하는 것이 SLF4J 라이브러리
- 인터페이스

## Logback

[Logback Home](http://logback.qos.ch)

- 스프링 부트가 기본으로 제공하는 로그 라이브러리
- 실무에서는 Logback을 주로 사용
- 구현체

# 예시

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@Slf4j
@RestController
/*
* @Controller는 반환값이 String이면 뷰 이름으로 인식됨 -> 뷰를 찾고 뷰가 렌더링 됨
* @RestController는 반환값으로 뷰를 찾는 것이 아니라 HTTP body에 바로 입력
*/
public class LogTestController {
	// Logger 패키지 주의! -> slf4j
	// @Slf4j 어노테이션 넣으면 밑의 코드 안 적어도 됨 (Lombok)
	// private final Logger log = LoggerFactory.getLogger(getClass());

	@RequestMapping("/log-test")
	public String logTest() {
		String name = "Spring";

		log.trace("trace log={}", name);
		log.debug("debug log={}", name);
		log.info("info log={}", name);
		log.warn("warn log={}", name);
		log.error("error log={}", name);

		return "ok";
	}
}
```

- application.properties에 들어가서 하위 로그 레벨 설정 가능, 밑으로 갈수록 하위 로그 (더 특정한 로그)
- 상위 레벨로 설정하면 그 밑에 하위 로그는 다 찍힘
- 개발 서버에서는 **debug** 정도, 로컬에서는 **trace** 정도, 운영 서버에는 **info** 레벨
    1. trace
    2. debug
    3. info
    4. warn
    5. error

```java
// 기본은 info 레벨
// 전체 로그 레벨 설정
// 기본은 info 그대로 두는 것이 좋음
logging.level.root=trace or debug or ...

// 특정 패키지 로그 레벨 설정
// 필요할 때만 레벨 설정하는 것이 좋음
logging.level,hello.springmvc=trace or debug or ...
```

## 출력 포맷

![logging](https://github.com/seungwonbased/TIL/blob/main/Spring/assets/logging1.png)

- 시간, 레벨, PID, thread 정보 등

# 지켜야할 규칙

```java
log.trace("trace log={}", name);
log.debug("debug log={}", name);
log.info("info log={}", name);
log.warn("warn log={}", name);
log.error("error log={}", name);
```

### 만약 아래와 같이 한다면?

```java
log.trace("trace log=" + name);
log.debug("debug log=" + name);
log.info("info log=" + name);
log.warn("warn log=" + name);
log.error("error log=" + name);
```

- 실행은 됨

### 그러나 하면 안 됨

### 왜?

- 자바는 **문자열 더하기**가 있으면 메서드 호출 전에 일단 먼저 연산해서 결과를 가지고 있음
- 만약 내가 설정한 레벨이 info인데 trace와 debug의 문자열 값까지 자바가 더해서, 가지고 있다면?
    
    **→ 리소스 낭비**, 트래픽이 많은 경우 로그는 굉장히 많이 찍힐 수 있기 때문에 **매우 비효율적, 혼남**