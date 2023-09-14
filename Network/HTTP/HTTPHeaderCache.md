# HTTP header: 캐시와 조건부 요청

# 캐시 기본 동작

## 캐시가 없을 때

- 데이터가 변경되지 않아도 동일한 데이터를 계속 네트워크로 다운로드 받아야 함
- 인터넷 네트워크는 매우 느리고 비쌈
- 브라우저 로딩 속도가 느림
- 느린 사용자 경험

## 캐시 적용

- 캐시 유효 시간동안 네트워크를 사용하지 않아도 됨
- 비싼 네트워크 사용량을 줄일 수 있음
- 브라우저 로딩 속도가 매우 빠름
- 빠른 사용자 경험

### 캐시 시간 초과

- 캐시 유효 시간이 초과하면 서버를 통해 데이터를 다시 조회하고 캐시를 갱신
- 이때 다시 네트워크 다운로드가 발생

# 검증 헤더와 조건부 요청

## 캐시 시간 초과

- 캐시 유효 시간이 초과해서 서버에 다시 요청하면 다음 두 가지 상황이 나타남
	1. 서버에서 기존 데이터를 변경
	2. 서버에서 기존 데이터를 변경하지 않음
		- 데이터를 다시 전송하는 대신에 저장해두었던 캐시를 재사용 할 수 있음
		- 단, 클라이언트의 데이터가 서버의 데이터와 같다는 사실을 검증할 수 있는 방법 필요

## 검증 헤더 추가

### Last-Modified ↔ If-Modified-Since

![hc](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hc1.png)

![hc](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hc2.png)

![hc](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hc3.png)

![hc](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hc4.png)

![hc](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hc5.png)

![hc](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hc6.png)

![hc](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hc7.png)

![hc](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hc8.png)

![hc](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hc9.png)

- 캐시 유효 시간이 초과해도 서버의 데이터가 갱신되지 않으면 304 Not Modified + 헤더 메타 정보만 응답 (Body x)
- 클라이언트는 서버가 보낸 응답 헤더 정보로 캐시의 메타 정보를 갱신
- 클라이언트는 캐시에 저장되어 있는 데이터 재활용
- 결과적으로 네트워크 다운로드가 발생하지만 용량이 적은 헤더 정보만 다운로드
- 실용적인 해결책
- 단점
	- 1초 미만 (0.x초) 단위로 캐시 조정 불가능
		- 1초 미만으로 조정할 일이… ㅋㅋ
	- 날짜 기반의 로직 사용
	- 데이터를 수정해서 날짜가 다르지만 같은 데이터로 수정해서 데이터 결과가 똑같은 경우 구별 불가능
	- 서버에서 별도의 캐시 로직을 관리 불가능

### ETag ↔ If-None-Match

- ETag: Entity Tag
- 캐시용 데이터에 임의의 고유한 버전 이름을 달아둠
- 데이터가 변경되면 이 이름을 변경 (Hash를 다시 생성)
	- Hash는 Content가 똑같으면 계속 같음!
- ETag를 보내서 같으면 유지, 다르면 다시 받기
- 캐시 제어 로직을 서버에서 완전히 관리
- 클라이언트는 단순히 이 값을 서버에 제공, 클라이언트는 캐시 매커니즘을 모름

# 캐시와 검증, 조건부 요청 헤더

## 캐시 제어 헤더

### Cache-Control

- 캐시 지시어 (Directives)
- Cache-Control: max-age
	- 캐시 유효 시간, 초 단위 (보통 길게 잡음)
- Cache-Control: no-cache
	- 데이터는 캐시해도 되지만, 항상 Origin 서버에 검증하고 사용 (중간 프록시 서버에서 검증 x)
- Cache-Control: no-store
	- 데이터에 민감한 정보가 있으므로 저장하면 안 됨 (메모리에서 사용하고 최대한 빨리 삭제)

### Pragma

- 캐시 만료일 지정 (하위 호환)
- Pragma: no-cache
- HTTP 1.0 하위 호환
- 지금은 거의 사용 x

### Expires

- 캐시 만료일 지정 (하위 호환)
- 캐시 만료일을 정확한 날짜로 지정
- HTTP 1.0부터 사용
- 지금은 더 유연한 Cache-Control: max-age 권장
- Cache-Control: max-age와 함께 사용되면 Expires는 무시됨

## 검증 헤더와 조건부 요청 헤더

### 검증 헤더 (Validator)

- ETag: “asidsa02jdfsk0”
- Last-Modified: Thu, 04 Jun 2023 08:14:23 GMT

### 조건부 요청 헤더

- If-Match, If-None-Match: ETag 값 사용
- If-Modified-Since, If-Unmodified-Since: Last-Modified 값 사용

# 프록시 캐시

## 원 서버 (Origin server) 직접 접근

![hc](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hc10.png)

## 프록시 캐시 도입

![hc](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hc11.png)

## Cache-Control: 캐시 지시어 (기타)

- Cache-Control: public
	- 응답이 public 캐시에 저장되어도 됨
- Cache-Control: private
	- 응답이 해당 사용자만을 위한 것임
	- private 캐시에 저장해야 함 (기본값)
- Cache-Control: s-maxage
	- 프록시 캐시에만 적용되는 max-age
- Age: 60 (HTTP 헤더)
	- 오리진 서버에서 응답 후 프록시 캐시 내에 머문 시간 (초 단위)

# 캐시 무효화

## Cache-Control: 확실한 캐시 무효화 응답

- Cache-Control: no-cache
	- 데이터는 캐시해도 되지만, 항상 오리진 서버에 검증하고 사용 (이름처럼 캐시를 아예 안하는 것이 아님!)
- Cache-Control: no-store
	- 데이터에 민감한 정보가 있으므로 저장하면 안 됨 (메모리에서 사용하고 최대한 빨리 삭제)
- Cache-Control: must-revalidate
	- 캐시 만료 후 최초 조회 시 원 서버에 검증해야 함
	- 원 서버 접근 실패 시 반드시 오류(504 Gateway Timeout)가 발생해야 함
	- must-revalidate은 캐시 유효 시간이라면 캐시를 사용함
- Pragma: no-cache
	- HTTP 1.0 하위 호환
		→ 이 정도 하면 확실하게 무효화 됨
    

![hc](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hc12.png)

![hc](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hc13.png)

![hc](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hc14.png)

![hc](https://github.com/seungwonbased/TIL/blob/main/Network/assets/hc15.png)

- 순간 네트워크 단절일 때
	- no-cache
		- 옛날 데이터라도 보여주자… 응답 가능할 수도
	- must-revalidate
		- 오류 발생
		- 돈과 관련된 문제라던가…