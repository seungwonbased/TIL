# HTTP header: 일반 헤더

# HTTP header

![hgh](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hgh1.png)

- 용도
	- **HTTP 전송**에 필요한 모든 부가 정보
	- 메시지 바디의 내용, 크기, 압축, 인증, 요청 클라이언트 (브라우저) 정보, 서버 애플리케이션 정보, 캐시 관리 정보, …
	- 표준 헤더가 너무 많음
	- 필요하면 임의의 헤더 추가 가능
- 분류 - RFC2616 (과거)

![hgh](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hgh2.png)

- General header: 메시지 전체에 적용되는 정보
	- 예: Connection: close
- Request header: 요청 정보
	- 예: User-Agent: Mozilla/5.0
- Response header: 응답 정보
	- 예: Server: Apache
- Entity header: 엔티티 바디 정보
	- 예: Content-Type: text/html, Content-Length: 3423

## HTTP body

- Message body - RFC2616 (과거)

![hgh](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hgh3.png)
 
- 메시지 본문은 엔티티 본문을 전달하는데 사용
- 엔티티 본문은 요청이나 응답에서 전달할 실제 데이터
- 엔티티 헤더는 엔티티 본문의 데이터를 해석할 수 있는 정보 제공
	- 데이터 유형(HTML, JSON), 데이터 길이, 압축 정보 등

<aside>
💡 그런데…

</aside>

![hgh](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hgh4.png)

- RFC723x 변화
    - 엔티티 → 표현 (Representation)
    - **Representation = Representation metadata + Representation data**

## Message body - RFC7230 (최신)

![hgh](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hgh5.png)

- 엔티티라는 단어가 사라짐
- 메시지 바디를 통해 표현 데이터 전달
- 메시지 바디 = Payload
- **표현은 요청이나 응답에서 전달할 실제 데이터**
- 표현 헤더는 표현 데이터를 해석할 수 있는 정보 제공
	- 데이터 유형(HTML, JSON), 데이터 길이, 압축 정보 등
- \* 표현 헤더는 표현 메타데이터와 페이로드 메시지를 구분해야 하지만 여기서는 생략

# 표현 (Representation)

## 표현 헤더

- Content-Type: 표현 데이터의 형식 설명
	- 미디어 타입, 문자 인코딩
	- 예: text/html; charset=utf-8, application/json, image/png
- Content-Encoding: 표현 데이터 인코딩
	- 표현 데이터를 압축하기 위해 사용
	- 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가
	- 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
- Content-Language: 표현 데이터의 자연 언어
	- 예: ko, en, en-US
- Content-Length: 표현 데이터의 길이
	- 바이트 단위
	- Transfer-Encoding을 사용하면 Content-Length를 사용하면 안 됨

# 협상 (Content negotiation)

<aside>
💡 클라이언트가 선호하는 표현 요청

</aside>

- Accept: 클라이언트가 선호하는 미디어 타입 전달
- Accept-Charset: 클라이언트가 선호하는 문자 인코딩
- Accept-Encoding: 클라이언트가 선호하는 압축 인코딩
- Accept-Language: 클라이언트가 선호하는 자연 언어
- 협상 헤더는 요청 시에만 사용

## 우선 순위: Quality values q

- q 값은 0 ~ 1 사이의 값으로 클수록 우선 순위가 높음
![hgh](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hgh6.png)
- 생략하면 1
- 구체적인 것을 기준으로 미디어 타입을 맞춤

![hgh](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hgh7.png)

![hgh](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hgh8.png)

# 전송 방식

## 단순 전송

![hgh](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hgh9.png)

## 압축 전송

![hgh](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hgh10.png)

## 분할 전송

![hgh](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hgh11.png)

- Content-Length를 넣으면 안 됨!

## 범위 전송

![hgh](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hgh12.png)

# 일반 정보

## From

<aside>
💡 유저 에이전트의 이메일 정보

</aside>

- 일반적으로 잘 사용되지 않음
- 검색 엔진 같은 곳에서 주로 사용
- 요청에서 사용

## Referer

<aside>
💡 이전 웹 페이지 주소

</aside>

- 진짜 많이 쓰임
- 현재 요청된 페이지의 이전 웹 페이지 주소
- A → B로 이동하는 경우 B를 요청할 때 Referer: A를 포함해서 요청
- Referer를 사용해서 유입 경로 분석 가능
- 요청에서 사용
- * referer는 단어 referrer의 오타 ㅋㅋ

## User-Agent

<aside>
💡 유저 에이전트 애플리케이션 정보

</aside>

- 클라이언트의 애플리케이션 정보 (웹 브라우저 정보 등)
- 통계 정보
- 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
- 요청에서 사용

## Server

<aside>
💡 요청을 처리하는 Origin 서버의 소프트웨어 정보

</aside>

- Server: Apache/2.2.22 (Debian)
- 응답에서 사용

## Date

<aside>
💡 메시지가 발생한 날짜와 시간

</aside>

- 응답에서 사용

# 특별한 정보

## Host

<aside>
💡 요청한 호스트 정보 (도메인)

</aside>

- **필수값! 아주 중요함**

![hgh](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hgh13.png)

## Location

<aside>
💡 페이지 리다이렉션

</aside>

- 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면 Location 위치로 자동 이동 (Redirect)
- 응답 코드 3xx에서 설명
- 201 (Created): Location 값은 요청에 의해 생성된 리소스 URI를 의미
- 3xx (Redirection): Location 값은 요청을 자동으로 리디렉션 하기 위한 대상 리소스를 가리킴

## Allow

<aside>
💡 허용 가능한 HTTP 메서드

</aside>

- 405 (Method Not Allowed)에서 응답에 포함해야 함
- 예: Allow: GET, HEAD, PUT
- 서버에서 많이 구현되어 있지는 않음

## Retry-After

<aside>
💡 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간

</aside>

- 503 (Service Unavailable): 서비스가 언제까지 불능인지 알려줄 수 있음
- 예:
	- Retry-After: Fri, 31 Dec 1999 23:59:59 GMT (날짜 표기)
	- Retry-After: 120 (초 단위 표기)
- 잘 안 씀

# 인증

## Authorization

<aside>
💡 클라이언트 인증 정보를 서버에 전달

</aside>

- 예: Authorization: Basic xxxxxxxxxxxxxxx

## WWW-Authenticate

<aside>
💡 리소스 접근 시 필요한 인증 방법 정의

</aside>

- 401 Unauthorized 응답과 함께 사용
- 예: WWW-Authenticate: Newauth realm=”apps”, type=1, title=”Login to \”apps\””, Basic realm=”simple”

# Cookie

- Set-Cookie: 서버에서 클라이언트로 쿠키 전달 (응답)
- Cookie: 클라이언트가 서버에서 받은 쿠키를 저장하고 HTTP 요청 시 서버로 전달

> “HTTP는 **무상태, Stateless** 프로토콜"

- 클라이언트와 서버가 요청과 응답을 주고 받으면 연결이 끊어짐
- 클라이언트가 다시 요청하면 서버는 이전 요청을 기억하지 못함
- 클라이언트와 서버는 서로 상태를 유지하지 않음
	→ 쿠키 미사용 시 대안이 있다면 모든 요청에 사용자 정보를 포함
    → 개발의 어려움, 보안 문제 등
    → 브라우저를 완전히 종료하고 다시 연다면?

## 쿠키 사용

![hgh](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hgh14.png)

![hgh](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hgh15.png)

![hgh](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hgh16.png)

- 사용처
	- 사용자 로그인 세션 관리
	- 광고 정보 트래킹
- 쿠키 정보는 항상 서버에 전송됨
	- 네트워크 트래픽 추가 유발
	- 최소한의 정보만 사용 (세션 id, 인증 토큰)
	- 서버에 전송하지 않고 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지 참고
- 주의점!
	- 보안에 민감한 데이터는 저장하면 안 됨 (주민번호, 신용카드 번호 등)

## 쿠키: 생명 주기

- expires
	- Set-Cookie: expires=Sat, 26-Dec-2020 04:39:21 GMT
	- 만료일이 되면 쿠키 삭제
- max-age
	- Set-Cookie: max-age=3600 (3600초)
	- 0이나 음수를 지정하면 쿠키 삭제
- 세션 쿠키: 만료 날짜를 생략하면 브라우저 종료 전까지만 유지
- 영속 쿠키: 만료 날짜를 입력하면 해당 날짜까지 유지

## 쿠키: 도메인

- 명시: 명시한 문서 기준 도메인 + 서브 도메인 포함
	- 예: domain=example.org를 지정해서 쿠키 생성
		- example.org는 물론이고
		- dev.example.org도 쿠키 접근
- 생략: 현재 문서 기준 도메인만 적용
	- 도메인 지정을 생략
		- example.org에서만 쿠키 접근

## 쿠키: 경로

- 예: path=/home
- 이 경로를 포함한 하위 경로 페이지만 쿠키 접근
- 일반적으로 path=/ 와 같이 루트로 지정
- 위 예시에서는
	- /home: 가능
	- /home/level1: 가능
	- /hello: 불가능

## 쿠키: 보안

- Secure
	- 쿠키는 http, https를 구분하지 않고 전송
	- Secure를 적용하면 https인 경우에만 전송
- HttpOnly
	- XSS 공격 방지
	- 자바스크립트에서 접근 불가
	- HTTP 전송에만 사용
- SameSite
	- XSRF 공격 방지
	- 요청 도메인과 쿠기에 설정된 도메인이 같은 경우만 쿠키 전송