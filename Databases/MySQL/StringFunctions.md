 String Functions

# DOC

[MySQL :: MySQL 8.0 Reference Manual :: 12.8 String Functions and Operators](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html)

# 자주 쓰는 것

- **CONCAT**: 데이터 조각, 문자열을 결합하는 동작을 수행
	- 예를 들어, 성과 이름을 결합 → 풀 네임
	- SELECT … CONCAT() AS ‘새로운 명명’ FROM … 또한 가능
 
```sql
CONCAT(col, another col, txt, another txt, …)
```    

- **CONCAT_WS**
	- CONCAT(’구분자’, column, another column, …)
- **SUBSTRING**: 문자열의 각 부분을 선택 가능
	- **인덱스는 1부터 시작!**
	- 음수 인덱스는 -1부터 시작
	- SUBSTR() also works
	- Can be combined with other ST functions

```sql
SUBSTRING(’작업하려는 문자열’, 시작 인덱스, 끝 인덱스)
```

```sql
SUBSTRING(’문자열’, 인덱스): 인덱스부터 끝까지
```

- **REPLACE**: 특정 문자를 제거하거나 모든 공백을 쉼표 혹은 다른 문자로 바꾸고 싶은 경우
	- Also can be combined with other ST functions

```sql
REPLACE(’작업하려는 문자열’, ‘제거하려는 문자열’, ‘삽입하려는 문자열')
```

- **REVERSE**: 문자열을 뒤집음

```sql
REVERSE(’작업하려는 문자열’)
```

- **CHAR_LENGTH**: 문자열의 문자의 개수

```sql
CHAR_LENGTH(’작업하려는 문자열’)
```

- **UPPER & LOWER**: 문자열의 대소문자 변경
   
```sql
UPPER(’작업하려는 문자열’)
```

```sql
LOWER(’작업하려는 문자열’)
``` 

> **문자열 함수를 nesting 할 때 가급적이면 반복을 줄일 것!**


