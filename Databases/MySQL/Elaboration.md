# 선택사항 정교화

# Distinct

- 중복 제거, 고유한 값으로 가져다 줌

```sql
SELECT DISTINCT col FROM tab
```

# ORDER BY

- 정렬

```sql
SELECT col FROM tab ORDER BY col
```

- 기본적으로 오름차순 (Ascending)
- 마지막에 DESC 추가하면 descending / ASC는 ascending

# LIMIT

- 몇 개의 결과값을 조회할 지 특정할 수 있는 연산자
- 정렬하기와 엮어서 많이 사용하는 편

```sql
SELECT col FROM tab LIMIT (몇 개: 1부터 or 시작 행: 0부터, 몇 개)
```

# LIKE

- 데이터 검색을 돕는 연산자
- WHERE은 정확한 값을 특정함

```sql
WHERE col LIKE ‘문자열 with WILDCARDS’
```

- 와일드카드 종류
    
    ![elaboration](https://github.com/seungwonbased/TIL/blob/main/Databases/assets/Eleboration.png)
    
    - %: 포함, 검색하고자 하는 문자열 양쪽에 위치하면 그 문자열을 포함한 모든 걸 조회 가능, 뒤에만 있다면 그 문자열로 시작하는 모든 걸 조회 가능 …
    - _: 하나의 문자를 특정시킴, 가령 ‘____ ‘이면 네 문자 길이의 문자열을 특정
    - Escape character: /, 와일드카드인 심볼 자체를 특정하고 싶을 때