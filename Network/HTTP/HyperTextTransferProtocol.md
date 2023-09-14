# HTTP, Hyper Text Transfer Protocol

# 모든 것이 HTTP

- HTTP 메시지에 모든 것을 전송
	- HTML, Text
	- Image, audio, video, file
	- JSON, XML (API)
	- 거의 모든 형태의 데이터 전송 가능
	- 서버 간에 데이터를 주고 받을 때도 대부분 HTTP 사용
	- 지금은 HTTP 시대
- 기반 프로토콜
	- TCP: HTTP/1.1, HTTP/2
		- **HTTP/1.1**: 가장 많이 사용하기 때문에 가장 중요한 버전
	- UDP: HTTP/3
		- HTTP/3: 진행 중, TCP 대신에 UDP 사용, 성능 개선

# 특징

- 클라이언트-서버 구조
	- Request-Response 구조
	- 클라이언트는 서버에 요청을 보내고 응답을 대기
	- 서버가 요청에 대한 결과를 만들어서 응답
- 무상태 (Stateless) 프로토콜
	- 서버가 클라이언트의 상태를 보존하지 않음
	- 장점: 서버 확장성 높음 (Scale out)
	- 단점: 클라이언트가 추가 데이터 전송
        ![htx](https://github.com/seungwonbased/TIL/blob/main/Network/assets/htx1.png)
		- 상태 유지: 중간에 다른 점원으로 바뀌면 안됨
		- 무상태: 중간에 다른 점원으로 바뀌어도 됨
			- 갑자기 클라이언트 요청이 증가해도 서버 대거 투입 가능
			- 응답 서버를 쉽게 바꿀 수 있어 무한한 서버 증설 가능
	- 모든 것을 무상태로 설계할 수 있는 경우도 있고 없는 경우도 있음
		- 예: 로그인한 사용자의 경우 로그인 했다는 상태를 서버에 유지
		- 일반적으로 브라우저 쿠키와 서버 세션 등을 사용해서 상태 유지
		- 상태 유지는 최소한만 사용
- 비 연결성 (Connectionless)
    ![htx](https://github.com/seungwonbased/TIL/blob/main/Network/assets/htx2.png)
	- HTTP는 기본이 연결을 유지하지 않는 모델
	- 일반적으로 초 단위 이하의 빠른 속도로 응답
	- 한 시간 동안 수천 명이 서비스를 사용해도 실제 서버에서 동시에 처리하는 요청은 수십 개 이하로 매우 작음
	- 서버 자원을 매우 효율적으로 사용할 수 있음
	- 한계와 극복
		- TCP/IP 연결을 새로 맺어야 함: 3 way handshake 시간 추가
		- 웹 브라우저로 사이트를 요청하면 HTML 뿐만 아니라 JS, CSS, 추가 이미지 등 수많은 리소스가 함께 다운로드
		- 지금은 HTTP 지속 연결로 문제 해결
		- HTTP/2, HTTP/3에서 더 많은 최적화
- 단순함, 확장 가능
	- 메시지, 스펙이 단순

# HTTP 메시지

![htx](https://github.com/seungwonbased/TIL/blob/main/Network/assets/htx3.png)

- Start-line
	- 요청 메시지
		- HTTP 메서드
			- GET, POST, PUT, DELETE, …
			- 서버가 수행해야 할 동작 지정
		- 요청 대상
			- absolute-path\[?query]
			- absolute-path: “/”로 시작하는 경로
		- HTTP version
	- 응답 메시지
		- HTTP version
		- HTTP 상태 코드
			- 200: 성공
			- 400: 클라이언트 요청 오류
			- 500: 서버 내부 오류
		- 이유 문구: 사람이 이해할 수 있는 짧은 상태 코드 설명 글
- Header
	- 용도
		- HTTP 전송에 필요한 모든 부가 정보
		- 메시지 바디의 내용, 크기, 압축, 인증, 요청 클라이언트 (브라우저) 정보, 서버 애플리케이션 정보, 캐시 관리 정보, …
		- 표준 헤더가 너무 많음
		- 필요하면 임의의 헤더 추가 가능
- Message body
	- 용도
		- 실제 전송할 데이터
		- HTML, image, video, JSON 등 byte로 표현할 수 있는 모든 데이터 전송 가능

## 클라이언트 → 서버 데이터 전송

<aside>
💡 데이터 전달 방식은 크게 두 가지

</aside>

1. 쿼리 파라미터를 통한 데이터 전송
	- GET
	- 주로 정렬 필터 (검색어)
1. 메시지 바디를 통한 데이터 전송
	- POST, PUT, PATCH
	- 회원 가입, 상품 주문, 리소스 등록, 리소스 변경
- 네 가지 상황
	1. 정적 데이터 조회
		- 이미지, 정적 텍스트 문서
		- 쿼리 파라미터 미사용
		- GET을 사용해서 리소스 경로로 단순하게 조회 가능
	2. 동적 데이터 조회
		- 주로 검색, 게시판 목록에서 정렬 필터 (검색어)
		- 조회 조건을 줄여주는 필터, 조회 결과를 정렬하는 정렬 조건에 주로 사용
		- 쿼리 파라미터 사용
		- GET을 사용해서 데이터를 전달
	3. HTML Form을 통한 데이터 전송
		- 회원 가입, 상품 주문, 데이터 변경
		- HTML Form을 Submit 시 POST 전송
			- 예: 회원 가입, 상품 주문, 데이터 변경 등
		- Content-Type: application/x-www-form-urlencoded 사용
			- Form의 내용을 메시지 바디를 통해서 전송 (key=value, 쿼리 파라미터 형식)
			- 전송 데이터를 URL encoding 처리
				- 예: abc김 → abc%EA%N%43
		- GET 전송도 가능
	4. HTTP API를 통한 데이터 전송
		- 회원 가입, 상품 주문, 데이터 변경
		- 서버 to 서버
			- 백엔드 시스템 통신
		- 앱 클라이언트
			- iOS, Android
		- 웹 클라이언트
			- HTML에서 Form 전송 대신 자바 스크립트를 통한 통신에 사용
			- React, VueJS 같은 웹 클라이언트와 API 통신
		- POST, PUT, PATCH: 메시지 바디를 통해 데이터 전송
		- GET: 조회, 쿼리 파라미터로 데이터 전달
		- Content-Type: application/json을 주로 사용 (사실상 표준)