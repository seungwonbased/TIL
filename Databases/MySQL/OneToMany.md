# Relationship: One to Many

# Relationship basics

- One-to-one relationship
- One-to-many relationship
- Many-to-many relationship

# 참고

```sql
CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(100),
    last_name VARCHAR(100), 
    email VARCHAR(100)
);

CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    order_date DATE,
    amount DECIMAL(8, 2),
    customer_id INT,
    FOREIGN KEY(customer_id) 
				REFERENCES customers(id)
				ON DELETE CASCADE <- 외래키에 상응하는 열이 삭제되면 이 열도 삭제
);
```

# JOIN

<aside>
💡 조건을 기준으로 데이터를 결합

</aside>

## Cross join

- 전부 곱해서 Join, 쓸모 X

### 예:

```sql
SELECT * FROM customers, orders;
-> 25 rows 조회
```

## INNER JOIN

- 필요한 것을 취해서 Join

### Implicit inner join

- WHERE에 조건을 걸어서 Join

### Explicit inner join

- (INNER) JOIN ~ ON ~ 사용

### 예:

```sql
/* Implicit inner join */
SELECT * FROM customers, orders
WHERE customers.id = orders.customer_id;
-> 5 rows 조회

/* Explicit inner join */
SELECT * FROM customers
(INNER) JOIN orders
		ON customers.id = orders.customer_id;
```

## LEFT JOIN

- 첫번째 (왼쪽) 테이블에서 모든 걸 취해서 Join

### 예:

```sql
SELECT * FROM customers
LEFT JOIN orders
		ON customers.id = orders.customer_id;
-> 예를 들어 위와 같이 주문한 사람과 주문하지 않은 사람 모두를 살펴보고 싶을 때 사용...
-> 주문하지 않았다면 주문 부분에 NULL 값이 삽입됨

SELECT
		first_name,
		last_name,
		IFNULL(SUM(amount), 0) AS total_spent -> 첫번째 파라미터가 NULL이면 0으로
FROM customers
LEFT JOIN orders
		ON customers.id = orders.customer_id
GROUP BY customers.id
ORDER BY total_spent;

SELECT
    title AS 'unreviewed_series'
FROM series
LEFT JOIN reviews
    ON series.id = reviews.series_id
WHERE rating is NULL;
-> 리뷰가 아직 없는 시리즈만 조회
```

## RIGHT JOIN

- 두번째 (오른쪽) 테이블에서 모든 걸 취해서 Join

## LEFT & RIGHT JOIN FAQ

- Q: 좌측 조인과 우측 조인은 큰 차이가 있나?
    - A: 아니다. 결합하는 것들의 순서를 변경하면 된다.