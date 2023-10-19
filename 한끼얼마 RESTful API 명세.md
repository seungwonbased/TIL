# 요청 URL

|Index|Method|URI|Description|
|---|---|---|---|
|1|GET|/posts|레시피 리스트 조회|
|2|GET|/posts/{int:post_id}|레시피 상세 조회|
|3|DELETE|/posts/{int:post_id}|레시피 삭제|
|4|POST|/posts/forms|레시피 신규 등록|
|5|GET|/posts/forms/ingredients|재료 리스트 조회|
|6|GET|/posts/forms/{int:post_id}|레시피 수정 폼|
|7|PUT|/posts/forms/{int:post_id}|레시피 수정 등록|
|8|POST|/posts/{int:post_id}/likes|레시피 좋아요|
|9|POST|/posts/{int:post_id}/comments|댓글 작성|
|10|POST|/posts/comments/likes|댓글 좋아요|
|11|PUT|/posts/comments/{int:comment_id}|댓글 수정|
|12|DELETE|/posts/comments/{int:comment_id}|댓글 삭제|
|13|POST|/members/forms|신규 회원 가입|
|14|POST|/members/login|로그인|
|15|POST|/members/logout|로그아웃|
|16|GET|/members/tokens|JWT 토큰 재발급|

# 상세 명세
## 1. `GET` /posts

> 레시피 리스트 조회

### Request
#### Request Parameters

|Parameter|Value|Description|
|---|---|---|
|pageNo|1|조회하려는 페이지 번호를 전달|
|pageSize|10|조회하려는 페이지에서 데이터의 개수|

### Response Body

```json
{
	"posts": [
		{
			"id": 14,
			"title": "레시피 제목",
			"create_date": "작성일",
			"user_id": "유저 ID"
		},
		{
			"id": 13,
			"title": "레시피 제목",
			"create_date": "작성일",
			"user_id": "유저 ID"
		},
		{
			"id": 12,
			"title": "레시피 제목",
			"create_date": "작성일",
			"user_id": "유저 ID",
			"user_id": "유저 ID"
		},
		...
		{
			"id": 5,
			"title": "레시피 제목",
			"create_date": "작성일",
			"user_id": "유저 ID"
		}
	],
	"pageNo": 1,
	"pageSize": 10
}
```

## 2. `GET` /posts/{int:post_id}

> 레시피 상세 조회

### Response Body

```json
{
	"post": {
		"id": 13,
		"title": "레시피 제목",
		"content": "레시피 내용",
		"user_id": "레시피 작성자",
		"create_date": "레시피 작성일",
		"modify_date": "레시피 수정일",
		"price": 13400,
		"like": 5
	},
	"comment": [
		{
			"id": 16,
			"user_id": "댓글 작성자",
			"content": "댓글 내용",
			"create_date": "댓글 작성일",
			"modify_date": "댓글 수정일",
			"like": 3
		},
		{
			"id": 15,
			"user_id": "string, 댓글 작성자",
			"content": "text, 댓글 내용",
			"create_date": "string, 댓글 작성일",
			"modify_date": "댓글 수정일",
			"like": 0
		},
		{
			"id": 17,
			"user_id": "string, 댓글 작성자",
			"content": "text, 댓글 내용",
			"create_date": "string, 댓글 작성일",
			"modify_date": "댓글 수정일",
			"like": 1
		},
		...
	]
}
```

## 3. `DELETE` /posts/{int:post_id}

> 레시피 삭제

### Request Header

|Key|Value|Desc|
|---|---|---|
|Autorization|eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ...|JWT 토큰|
|X-CSRF-Token|cc4cb641-57f3-4f03-9ff9-690a49227fec|CSRF 토큰|

### Request Body

```json
{  
    "result": "failed",  
    "message": "삭제 권한이 없음"  
}

or 

{  
    "result": "success",  
    "message": "13 deleted"  
}
```

## 4. `POST` /posts/forms

> 레시피 신규 등록

### Request Header

|Key|Value|Desc|
|---|---|---|
|Autorization|eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ...|JWT 토큰|
|X-CSRF-Token|cc4cb641-57f3-4f03-9ff9-690a49227fec|CSRF 토큰|

### Request Body

```json
{
	"post": {
		"title": "레시피 제목",
		"content": "레시피 내용"
	},
	"ingredients": [
		{
			"id": 43,
			"quantity": 500,
			"price": 3000,
			"unit": 200
		},
		{
			"id": 12,
			"quantity": 300,
			"price": 3000,
			"unit": 200
		},
		{
			"id": 33,
			"quantity": 250,
			"price": 3000,
			"unit": 200
		},
		...
	]
}
```

### Response Body

```json
{  
    "result": "failed",  
    "message": "레시피 등록 권한이 없음"  
}

or

{  
    "result": "success",  
    "message": "레시피 등록 성공"  
}
```

## 5. `GET` /posts/forms/ingredients

> 재료 리스트 조회

### Response Body

```json
{
	"ingredients": [
		{
			"id": 1,
			"name": "재료 이름",
			"price": 25000,
			"unit": 500
		},
		{
			"id": 2,
			"name": "재료 이름",
			"price": 25000,
			"unit": 500
		},
		{
			"id": 3,
			"name": "재료 이름",
			"price": 25000,
			"unit": 500
		},
		...
	]
}
```

## 6. `GET` /posts/forms/{int:post_id}

> 레시피 수정 폼

### Request Header

|Key|Value|Desc|
|---|---|---|
|Autorization|eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ...|JWT 토큰|
|X-CSRF-Token|cc4cb641-57f3-4f03-9ff9-690a49227fec|CSRF 토큰|

### Response Body

```json
{  
    "result": "failed",  
    "message": "수정 권한이 없음"  
}

or

{  
    "id": 13,  
    "title": "레시피 제목",  
    "content": "레시피 내용"
}
```

## 7. `PUT` /posts/forms/{int:post_id}

> 레시피 수정 등록

### Request Body

```json
{
	"post": {
		"title": "string, 레시피 제목",
		"content": "text, 레시피 내용"
	},
	"ingredients": [
		{
			"id": 43,
			"quantity": 500,
			"price": 3000,
			"unit": 200
		},
		{
			"id": 12,
			"quantity": 300,
			"price": 3000,
			"unit": 200
		},
		{
			"id": 33,
			"quantity": 250,
			"price": 3000,
			"unit": 200
		},
		...
	]
}
```

### Response Body

```json
{  
    "result": "success",  
    "message": "레시피 수정 성공"  
}
```

## 8. `POST` /posts/{int:post_id}/likes

> 레시피 좋아요

### Request Header

|Key|Value|Desc|
|---|---|---|
|Autorization|eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ...|JWT 토큰|
|X-CSRF-Token|cc4cb641-57f3-4f03-9ff9-690a49227fec|CSRF 토큰|

### Response Body

```json
{  
    "result": "failed",  
    "message": "자신이 작성한 레시피 좋아요 불가"  
}

or 

{  
    "result": "failed",  
    "message": "중복 좋아요 불가"  
}

or

{  
    "result": "success",  
    "message": "레시피 좋아요 성공"  
}
```

## 9. `POST` /posts/{int:post_id}/comments

> 댓글 작성
### Request Header

|Key|Value|Desc|
|---|---|---|
|Autorization|eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ...|JWT 토큰|
|X-CSRF-Token|cc4cb641-57f3-4f03-9ff9-690a49227fec|CSRF 토큰|

### Request Body

```json
{
	"content": "댓글 내용"
}
```

### Response Body

```json
{  
    "result": "success",  
    "message": "댓글 작성 성공"
}
```

## 10. `POST` /comments/{int:comment_id}/likes

> 댓글  좋아요

### Request Header

|Key|Value|Desc|
|---|---|---|
|Autorization|eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ...|JWT 토큰|
|X-CSRF-Token|cc4cb641-57f3-4f03-9ff9-690a49227fec|CSRF 토큰|

### Response Body

```json
{  
    "result": "failed",  
    "message": "자신이 작성한 댓글 좋아요 불가"  
}

or

{  
    "result": "failed",  
    "message": "중복 좋아요 불가"  
}

or 

{  
    "result": "success",  
    "message": "댓글 좋아요 성공"  
}
```

## 11. `PUT` /comments/{int:comment_id}

> 댓글 수정

### Request Header

|Key|Value|Desc|
|---|---|---|
|Autorization|eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ...|JWT 토큰|
|X-CSRF-Token|cc4cb641-57f3-4f03-9ff9-690a49227fec|CSRF 토큰|

### Request Body

```json
{
	"content": "댓글 내용"
}
```

### Response Body

```json
{  
    "result": "failed",  
    "message": "댓글 수정 권한이 없음"  
}

or 

{  
    "result": "success",  
    "message": "댓글 수정 성공"  
}
```

## 12. `DELETE` /comments/{int:comment_id}

> 댓글 삭제

### Request Header

|Key|Value|Desc|
|---|---|---|
|Autorization|eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ...|JWT 토큰|
|X-CSRF-Token|cc4cb641-57f3-4f03-9ff9-690a49227fec|CSRF 토큰|

### Response Body

```json
{  
    "result": "failed",  
    "message": "댓글 삭제 권한이 없음"  
}

or

{  
    "result": "success",  
    "message": "댓글 삭제 성공"  
}
```

## 13. `POST` /members/forms

> 회원가입

### Request Body

```json
{
	"username": "유저네임 (ID)",
	"password": "패스워드",
	"email": "email@email.com"
}
```

### Response Body

```json
{  
    "result": "failed",  
    "message": "중복 가입"  
}

or

{  
    "result": "success",  
    "message": "회원 가입 성공"  
}
```

## 14.  `POST` /members/login

> 로그인

### Request Body

```json
{
	"username": "유저네임 (ID)",
	"password": "패스워드"
}
```

### Response Header

|Key|Value|Desc|
|---|---|---|
|Set-Cookie|access_token_cookie=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmcmVzaCI6ZmFsc2UsImlhdCI6MTY5NzY5MTIyMCwianRpIjoiOGRhM2M2MzUtOTk5NS00MjBiLWFjM2MtODE0NTVkYWEyZTliIiwidHlwZSI6ImFjY2VzcyIsInN1YiI6InBvc3RtYW4wIiwibmJmIjoxNjk3NjkxMjIwLCJjc3JmIjoiNDIwMmRhYjItNDdhMy00Y2I5LWJiMjgtZWJjOGZmY2Q1NGExIiwiZXhwIjoxNjk3NjkzMDIwfQ.V8SGeXBVa7ZnmmJ2nMkmUKnPxjIvh-wJqUTnaKcVGWk; HttpOnly; Path=/|JWT 토큰|
|Set-Cookie|csrf_access_token=4202dab2-47a3-4cb9-bb28-ebc8ffcd54a1; Path=/|CSRF 토큰|
|Set-Cookie|refresh_token_cookie=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.e
|Set-Cookie|csrf_refresh_token=4202dab2-47a3-4cb9-bb28-ebc8ffcd54a1; Path=/|CSRF 토큰|

### Response Body

```json
{  
    "result": "failed",  
    "message": "등록되지 않은 아이디거나 비밀번호가 일치하지 않음"  
}

or

{  
    "result": "success",  
    "message": "로그인 성공"  
}
```

## 15.  `POST` /members/logout

> 로그아웃

### Request Header

|Key|Value|Desc|
|---|---|---|
|Autorization|eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ...|JWT 토큰|
|X-CSRF-Token|cc4cb641-57f3-4f03-9ff9-690a49227fec|CSRF 토큰|

### Response Body

```json
{  
    "result": "success",  
    "message": "로그아웃 성공"  
}
```

## 16.  `POST` /members/tokens

> 토큰 리프레시

### Request Header

|Key|Value|Desc|
|---|---|---|
|Autorization|eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ...|JWT 토큰|
|X-CSRF-Token|cc4cb641-57f3-4f03-9ff9-690a49227fec|CSRF 토큰|
|refresh_token_cookie|eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.e|JWT 리프레시 토큰|
|csrf_refresh_token|4202dab2-47a3-4cb9-bb28-ebc8ffcd54a1; Path=/|CSRF 토큰|

### Response Header

|Key|Value|Desc|
|---|---|---|
|Set-Cookie|access_token_cookie=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmcmVzaCI6ZmFsc2UsImlhdCI6MTY5NzY5MTIyMCwianRpIjoiOGRhM2M2MzUtOTk5NS00MjBiLWFjM2MtODE0NTVkYWEyZTliIiwidHlwZSI6ImFjY2VzcyIsInN1YiI6InBvc3RtYW4wIiwibmJmIjoxNjk3NjkxMjIwLCJjc3JmIjoiNDIwMmRhYjItNDdhMy00Y2I5LWJiMjgtZWJjOGZmY2Q1NGExIiwiZXhwIjoxNjk3NjkzMDIwfQ.V8SGeXBVa7ZnmmJ2nMkmUKnPxjIvh-wJqUTnaKcVGWk; HttpOnly; Path=/|JWT 토큰|
|Set-Cookie|csrf_access_token=4202dab2-47a3-4cb9-bb28-ebc8ffcd54a1; Path=/|CSRF 토큰|
|Set-Cookie|refresh_token_cookie=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.e
|Set-Cookie|csrf_refresh_token=4202dab2-47a3-4cb9-bb28-ebc8ffcd54a1; Path=/|CSRF 토큰|

### Response Body

```json
{  
    "result": "failed",  
    "message": "refresh failed"  
}

or 

{  
    "result": "success",  
    "message": "refresh success"  
}
```
