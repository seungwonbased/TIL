# Cron 형식

- White Space로 5개 값을 구분해 설정

```shell
1 11 3 * * [SHELL_PATH]
분 시 일 월 요일 쉘_스크립트_경로
```

- 분
	- 분(minute)을 의미
	- 몇 분에 실행 할 것인지 설정
	- e.g., 1 11 3 * * <SHELL_PATH>
		- XX일 XX시 '01분'에 실행
		- `*`을 설정할 경우, 매 분 (1분 마다) 실행을 의미
- 시
	- 시간(hour)을 의미
	- 몇 시에 실행 할 것인지 설정
	- e.g., 1 11 3 * * <SHELL_PATH>
		- XX일 '11시' 01분에 실행
		- `*`을 설정할 경우, 매 시간 (1시간 마다) 실행을 의미
- 일
	- 일(day of month)을 의미
	- 몇 일에 실행 할 것인지 설정
	- e.g., 1 11 3 * * <SHELL_PATH>
		- '3일' 11시 01분에 실행
		- `*`을 설정할 경우, 매일 실행을 의미
- 월
	- 월(month)을 의미
	- 몇 월에 실행 할 것인지 설정
	- e.g., 1 11 3 5 * <SHELL_PATH>
		- '5월' 3일 11시 01분에 실행
		- `*`을 설정할 경우, 매 월 실행을 의미
- 요일
	- 요일(day of week)을 의미
	- 무슨 요일에 실행 할 것인지 설정
	- 0 ~ 6 사이의 값을 설정 (0: 일요일, 6: 토요일)
	- e.g., 1 11 3 * 3 <SHELL_PATH>
		- 매달 3일 11시 01분 수요일에 실행
		- `*`을 설정할 경우, 매일 실행을 의미

## Examples

|Expressions|Desc|
|---|---|
|`* * * * *`|매일 1분마다 실행|
|`5 * * * *`|매일 매시간 05분에 실행 (1시간 간격으로 실행)|
|`*/5 * * * *`|매일 5분마다 실행|
|`*/10 * * * *`|매일 10분마다 실행|
|`0,10,20,30,40,50 * * * *`|매일 10분마다 실행|
|`0 18 * * *`|매일 18시 00분에 실행|
|`45 22 * * *`|매일 22시 45분에 실행|
|`28 03 * * *`|매일 03시 28분에 실행|
|`* 1 * * *`|매일 01시 00분 ~ 01시 59분 사이에 1분 간격으로 실행|
|`0 */1 * * *`|매일 1시간 간격으로 실행 (매시간 00분)|
|`0 */12 * * *`|매일 12시간마다 실행|
|`0 6,12 * * *`|매일 06시, 12시에 실행|
|`10 2-5 * * *`|매일 02시 ~ 05시 사이 매시간 10분에 실행|
||(02시 10분, 03시 10분, 04시 10분, 05시 10분)|
|`5 8-20/3 * * *`|매일 08시 ~ 20시 사이 3시간 간격으로 05분에 실행|
||(08시 05분, 11시 05분, 14시 05분, 17시 05분, 20시 05분)|
|`42 4 10 * *`|매달 10일 04시 42분에 실행|
|`30 5 1,15 * *`|매달 1일과 15일 05시 30분에 실행|
|`0-10 17 1 * *`|매달 1일 17시 00분 ~ 17시 10분까지 1분 단위로 실행|
|`0 17 * * 1`|매주 월요일 17시 00분에 실행|
|`0,10 17 * * 0,2,3`|매주 일, 화, 수요일 17시 00분과 17시 10분에 실행|
|`0 0 1,15 * 1`|매달 1일과 15일 그리고 월요일 24시 00분에 실행|
|`0 6,12 * * 0,3`|수, 일요일마다 06시, 12시에 실행|
|`0 21 * * 1-6`|월 ~ 토 21시 00분에 실행|
