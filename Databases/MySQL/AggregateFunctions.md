# Aggregate Functions, 집계 함수

# COUNT

- Counts whatever i want

```SQL
SELECT COUNT(col (혹은 *)) FROM tab;
```

- String functions, 선택 정교화와 함께 사용 가능
    
    ```sql
예: SELECT COUNT(DISTINCT author_fname) FROM books;
    ```
    

# GROUP BY

- 다른 함수와 함께 사용해야 함
- 동일한 데이터를 각각의 단일 행으로 요약 또는 집계

```sql
SELECT col FROM tab GROUP BY col;
```

# HAVING

- GROUP BY 절에서 조건을 주려면 WHERE이 아닌 HAVING을 이용해야 함

```sql
SELECT col FROM tab GROUP BY col HAVING COUNT(col) = 1;
```

# MIN and MAX

```sql
SELECT MIN or MAX(col) FROM tab
```

- 만약: SELECT MIN(pages), title FROM books; 쿼리를 날린다면?
    - pages column의 가장 작은 값과 title의 가장 앞에 있는 값이 조회될 것
        - 이것은 의도한 바가 아님, MIN(pages)의 title을 조회하고 싶음
        - 서브 쿼리 이용
        
        ```sql
SELECT * FROM books
WHERE pages = (SELECT MIN(pages)
				FROM books);
        ```
        

# SUM

- 합

```sql
SELECT SUM(col) FROM tab GROUP BY col;
```

# AVG

- 평균

```sql
SELECT AVG(col) FROM tab GROUP BY col;
```