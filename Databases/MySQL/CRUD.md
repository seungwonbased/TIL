# CRUD

# CREATE: CREATE

```SQL
CREATE TABLE (테이블 이름) (column, column, …);
```

# READ: SELECT

```SQL
SELECT column, column, … FROM table;
```

- *: All columns

## WHERE

```SQL
SELECT columns FROM table WHERE 조건;
```

## Aliases

```SQL
SELECT column AS (출력할 이름), column AS (출력할 이름), column, … FROM table;
```

# UPDATE: UPDATE

```SQL
UPDATE table SET (업데이트 내용) WHERE 조건;
```

- 업데이트 내용: column=(업데이트 내용)
- 조건: 바꾸려는 row의 조건

# DELETE: DELETE

```SQL
DELETE FROM table WHERE 조건;
```

```SQL
DELETE FROM cats (모든 row 삭제)
```

- 지우기 전에 SELECT table FROM ~ 로 조회 후 DELETE FROM table ~ 와 같이 복사해서 쓰면서 한 번 더 확인하는 것도 좋을듯