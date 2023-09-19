# Trigger

- 데이터 검정 (Validating data) → 사실 이건 클라이언트 사이드에서 하는 게 좋음
- 특정 동작을 일으킨 원본 테이블을 기초로 다른 테이블을 조작
- …

# The syntax

```sql
CREATE TRIGGER trigger_name
		trigger_time trigger_event ON table_name FOR EACH ROW
		BEGIN
		...
		END;
```

## trigger_time

- Before
- After

## trigger_event

- INSERT
- UPDATE
- DELETE

## table_name

- photos
- users
- …