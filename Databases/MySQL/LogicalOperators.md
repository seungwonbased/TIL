# LogicalOperators

# Equal, Not equal

- =, !=

# LIKE, NOT LIKE

- 문자열에서 패턴을 매칭할 때 사용

## 예:

```sql
SELECT title FROM books WHERE title LIKE 'W';
-> 'W'인 로우 조회

SELECT title FROM books WHERE title LIKE 'W%';
-> 'W'로 시작하는 로우 조회

SELECT title FROM books WHERE title LIKE '%W%';
-> 'W'가 포함된 로우 조회
```

# Greater than, less than

- Greater than, greater than or equal to
- Less than, less than or equal to

<aside>
💡 참고: MySQL에서 Boolean - True: 1 / False: 0

</aside>

<aside>
💡 참고: MySQL에서 대문자와 소문자 비교 시 True 반환

</aside>

# AND, OR

- AND와 && 둘 다 사용 가능
- OR과 || 둘 다 사용 가능

# BETWEEN x AND y, NOT BETWEEN x AND y

- 사이에 있는 값을 찾을 수 있음 (양 끝 값 포함)
- <, >, = 들로도 구현 가능
- BETWEEN을 날짜나 시간 값과 사용할 때 값을 원하는 자료형으로 변환하기 위해 CAST()를 사용하라
    - 사실 사용 안 해도 될 수도 있는데 쓰는게 좋을듯

## 예:

```sql
SELECT title, released_year FROM books
WHERE released_year BETWEEN 2004 AND 2015;

SELECT birthdt FROM people
WHERE birthdt BETWEEN CAST('2023-01-26' AS DATETIME)
								AND CAST('2024-01-26' AS DATETIME);
```

# IN, NOT IN

- 값 집합을 제공할 수 있게 해줌

## 예:

### IN을 사용하지 않았을 때

```sql
SELECT title, author_lname FROM books
WHERE author_lname = 'Carver' OR
		author_lname = 'Lahiri' OR
		author_lname = 'Smith';

SELECT title, author_lname FROM books
WHERE author_lname != 'Carver' AND
		author_lname != 'Lahiri' AND
		author_lname != 'Smith';

		-> NOT IN 일 때는 AND를 써야함!!!
```

### IN을 사용했을 때

```sql
SELECT title, author_lname FROM books
WHERE author_lname IN ('Carver', 'Lahiri', 'Smith');

SELECT title, author_lname FROM books
WHERE author_lname NOT IN ('Carver', 'Lahiri', 'Smith');
```

→ 간결하다.

## 참고: % 연산자

```sql
SELECT title, released_year FROM books
WHERE released_year >= 2000 AND
		released_year NOT IN (2000, 2002, 2004, 2006, 2008, 2010, ...);

=

SELECT title, released_year FROM books
WHERE released_year >= 2000 AND
		released_year % 2 != 0;
```

# CASE statements, IF()

```sql
SELECT title, released_year, **<- 콤마 꼭 써야함!!!**
			CASE
				WHEN released_year >= 2000 THEN 'Modern Lit'
				ELSE '20th Century Lit'
			END AS GENRE
FROM books;
-> title, released_year, GENRE (released_year가 2000보다 크거나 같으면
								장르가 'Modern Lit'이 되고, 그렇지 않으면
								장르가 '20th Century Lit'이 됨) 를 출력

IF()를 쓰면?

SELECT 
		title, 
		released_year,
		IF(released_year >= 2000, 'Modern Lit', '20th Century Lit') AS 'GENRE'
FROM books;		

SELECT title, stock_quantity, **<- 콤마 꼭 써야함!!!**
			CASE
				WHEN stock_quantity BETWEEN 0 AND 50 THEN '*' <- 콤마 쓰면 안 됨!!!
				WHEN stock_quantity BETWEEN 51 AND 100 THEN '**'
				ELSE '***'
			END AS STOCK
FROM books;
```