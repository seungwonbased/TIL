# DataTypes

# Characters

## VARCHAR

- 가변 길이
- 유연함

## CHAR

- 고정 길이, 항상 동일한 양이 할당됨
- 부족하면 공백으로 채워지고 넘치면 나머지는 잘림
- VARCHAR보다 빠름
- 우편번호, Y/N flag, M/F sex, …

<aside>
💡 VARCHAR, CHAR 속도 차이가 크진 않음

</aside>

# Numbers

## INT

- 정수

## DECIMAL

- 소수, 고정소수점
- 예: 999.99 → DECIMAL(5, 2)
- 넘치면 MAX 값으로 삽입됨
    - 예: DECIMAL(5, 2)에 100000 삽입 → 999.99 삽입
- 소수부가 넘치면 반올림해서 삽입됨
    - 예: DECIMAL(5, 2)에 1.9999 삽입 → 2.00 삽입

## FLOAT

- 소수, 부동소수점

## DOUBLE

- 소수, 부동소수점

<aside>
💡 부동소수점은 적은 공간으로 많은 숫자를 저장 가능

</aside>

<aside>
💡 정밀도를 중요시 하고 범위를 컨트롤 할 수 있는 경우에는 DECIMAL

</aside>

# Dates & Times

## DATE

- ‘YYYY-MM-DD’ format

## TIME

- ‘HH:MM:SS’ format

## DATETIME

- ‘YYYY-MM-DD HH:MM:SS’ format

## Functions for Dates & Times

<aside>
💡 공식 독스 보면 훨씬 많은 것들이 있음

</aside>

### CURDATE()

- 현재 날짜

### CURTIME()

- 현재 시간

### NOW()

- 현재 날짜 시간

### 등등…

### 예:

```sql
SELECT CURDATE();
SELECT CURTIME();
SELECT NOW();

INSERT INTO people (name, posting_time) 
VALUES ('Bae', NOW()); -> 현재 DATETIME INSERT
```

## Formatting Dates & Times

### DAY()

- 날짜 추출

### DAYNAME()

- 요일 추출

### DAYOFWEEK()

- 일요일을 1로 하여 주에서 몇 번째 날짜인지

### DAYOFYEAR()

- 1/1을 1로 하여 연내 몇 번째 날짜인지 (윤년 반영)

### 등이 있는데…

### 예:

```sql
SELECT name, birthday, DAYNAME(birthday) FROM people;
```

### **DATE_FORMAT() → 이걸 사용하는 것이 좋음**

- format string 이용하여 추출 가능
- %D, %W, %d, …

### 예:

```sql
SELECT DATE_FORMAT('2009-10-04 22:23:00', 'Line: %W %M %Y');
-> 'Line: Sunday October 2009' 추출
```

## Date math

### DATEDIFF()

- 날짜 차이 계산

### DATE_ADD()

- ‘INTERVAL number 단위’로 더하기
- + / - 사인으로 간략하게 쓸 수 있음

### 예:

```sql
SELECT DATEDIFF(NOW(), birthdate) FROM people;
-> 12240

SELECT DATE_ADD(birthdate, INTERVAL 1 MONTH) FROM people;
-> '1944-01-25' (원래는 '1943-12-25')

SELECT '2008-12-31 23:59:59' + INTERVAL 1 SECOND;
-> '2009-01-01 00:00:00'

SELECT '2008-12-31 23:59:59' + INTERVAL 1 SECOND - INTERVAL 2 SECOND;
-> '2008-12-31 23:59:58'
```

# TIMESTAMP

<aside>
💡 뭔가를 삽입한 대에 관한 정보를 저장하는 것, MySQL에서는 하나의 자료형이기도 함

</aside>

- DATETIME과 형식이 같음
    - 그러나 DATETIME의 범위는 1000-01-01 00:00:00 ~ 9999-12-31 23:59:59
    - TIMESTAMP의 범위는 1970-01-01 ~ 현재로부터 약 20년 뒤
        - 공간을 더 적게 차지!

### 예:

```sql
CREATE TABLE comments (
	content VARCHAR(200),
	created_at TIMESTAMP DEFAULT NOW()
);
```