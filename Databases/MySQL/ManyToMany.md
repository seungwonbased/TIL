# Relationship: Many to Many

# 예:

- 리뷰 ↔ 리뷰어
- 유저 ↔ 포스트
- 포스트 ↔ 해시태그
- 학생 ↔ 수업

# How to set relationship

## reviewers ← reviews → movies

- 와 같이 엮어주는 테이블을 만듬

## 예:

```sql
SELECT
		title,
		rating,
		CONCAT(first_name, ' ', last_name) AS reviewer
FROM reviewers
INNER JOIN reviews
		ON reviewers.id = reviews.reviewer_id
INNER JOIN series
		ON series.id = reviews.series_id
ORDER BY title;
```