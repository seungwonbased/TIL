# HTTP 상태 코드

# HTTP 상태 코드

## 상태 코드

<aside>
💡 클라이언트가 보낸 요청의 처리 상태를 응답에서 알려주는 기능

</aside>

![code](https://github.com/seungwonbased/TIL/blob/main/Network/assets/code1.png)

- 만약 모르는 상태 코드가 나타나면?
	- 클라이언트가 인식할 수 없는 상태 코드를 서버가 반환할 경우, 클라이언트는 상위 상태 코드로 해석해서 처리
	- 예:
		- 299 ??? → 2xx (Successful)
		- 451 ??? → 4xx (Client Error)
		- 599 ??? → 5xx (Server Error)

### 1xx (Informational)

- 거의 사용하지 않으므로 생략

### 2xx (Successful)

- 클라이언트의 요청을 성공적으로 처리
- 200 OK: 요청 성공
- 201 Created: 요청 성공해서 새로운 리소스 생성됨
- 202 Accepted: 요청 접수되었으나 처리 완료되지 않음 (잘 사용 안됨)
- 204 No Content: 서버가 요청을 성공적으로 수행했지만 응답 페이로드 본문에 보낼 데이터가 없음
- …

### 3xx (Redirection)

- 요청을 완료하기 위해 유저 에이전트의 추가 조치 필요
- Redirection: 웹 브라우저가 3xx 응답의 결과에 대해 location 헤더가 있으면 location 위치로 자동 이동
- 301 Move Permanently, 308 Permanent Redirect: 리소스의 URI가 영구적으로 이동, 308은 본문 유지됨! 실무에선 그냥 본문 지우는 경우가 많음
- 302 Found, 307 Temporary Redirect, 303 See Other: 일시적인 리다이렉션, 따라서 검색 엔진 등에서 URL을 변경하면 안 됨
- 304 Not Modifired: 캐시를 목적으로 사용

### 4xx (Client Error)

- **오류의 원인이 클라이언트**
- 400 Bad Request: 클라이언트가 잘못된 요청을 해서 서버가 요청을 처리할 수 없음, 요청 파라미터가 잘못되거나 API 스펙에 맞지 않는 요청
- 401 Unauthorized: 클라이언트가 해당 리소스에 대한 인증이 필요, Authentication 되지 않음 (로그인이 안 됨, 이름이 아쉬운 부분)
	- 참고 (원래)
		- Authentication: 본인이 누구인지 확인 (로그인)
		- Authorization: 권한 부여 (ADMIN 권한처럼 특정 리소스에 접근할 수 있는 권한)
- 403 Forbidden: 서버가 요청을 이해했지만 거부
- 404 Not Found: 요청 리소스를 찾을 수 없음 (권한이 부족한 리소스에 접근할 때도)

### 5xx (Server Error)

- 서버 오류, 재시도 하면 성공할 수도 있음
- 500 Internal Server Error: 애매하면 500 오류
- 503 Service Unavailable: 서비스 이용 불가 (서버 과부화, 예정된 작업 수행 등)