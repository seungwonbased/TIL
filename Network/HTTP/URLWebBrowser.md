# URI와 웹 브라우저 요청 흐름

# URI (Uniform Resource Identifier)

> “URI는 Locator, Name 또는 둘 다 추가로 분류될 수 있다.”

![ub](https://github.com/seungwonbased/TIL/blob/main/Network/assets/ub1.png)

![ub](https://github.com/seungwonbased/TIL/blob/main/Network/assets/ub2.png)

- URI란?
	- **U**niform: 리소스를 식별하는 통일된 방식
	- **R**esource: 자원, URI로 식별할 수 있는 모든 것 (제한 없음)
	- **I**dentifier: 다른 항목과 구분하는 데 필요한 정보
- URL, URN이란?
	- **L**ocator: 리소스가 있는 위치를 가정
	- **N**ame: 리소스에 이름을 부여
	- 위치는 변할 수 있지만, 이름은 변하지 않음
	- URN 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화되지 않음
	- 앞으로 URI = URL 같은 의미로 이야기

## URL

- 전체 문법    

![ub](https://github.com/seungwonbased/TIL/blob/main/Network/assets/ub3.png)

- 프로토콜: https
- 호스트명: www.google.com
- 포트 번호: 443
- 패스: /search
- 쿼리 파라미터: q=hello&hl=ko
- Scheme
	- 주로 프로토콜 사용
		- 프로토콜: 어떤 방식으로 자원에 접근할 것인가 하는 규약 (예: HTTP, HTTPS, FTP 등)
		- HTTP는 80 포트, HTTPS는 443 포트를 주로 사용, 포트는 생략 가능
		- HTTPS는 HTTP에 보안 추가
- Userinfo@
	- URL에 사용자 정보를 포함해서 인증
	- 거의 사용하지 않음
- Host
	- 호스트명
	- 도메인명 또는 IP address를 직접 사용 가능
- Port
	- 접속 포트
	- 일반적으로 생략, 생략시 HTTP는 80 HTTPS는 443
- Path
	- 리소스 경로, 계층적 구조
- Query
	- key=value 형태
	- ?로 시작, &로 추가 가능
		- 예: ?keyA=valueA&keyB=valueB&…
- Fragment
	- html 내부 북마크 등에 사용
	- 서버에 전송하는 정보는 아님

# 웹 브라우저 요청 흐름

![ub](https://github.com/seungwonbased/TIL/blob/main/Network/assets/ub4.png)

![ub](https://github.com/seungwonbased/TIL/blob/main/Network/assets/ub5.png)

![ub](https://github.com/seungwonbased/TIL/blob/main/Network/assets/ub6.png)

![ub](https://github.com/seungwonbased/TIL/blob/main/Network/assets/ub7.png)

![ub](https://github.com/seungwonbased/TIL/blob/main/Network/assets/ub8.png)

![ub](https://github.com/seungwonbased/TIL/blob/main/Network/assets/ub9.png)

![ub](https://github.com/seungwonbased/TIL/blob/main/Network/assets/ub10.png)

![ub](https://github.com/seungwonbased/TIL/blob/main/Network/assets/ub11.png)
