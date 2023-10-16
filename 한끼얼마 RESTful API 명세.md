# 요청 URL

|Index|Method|URI|Description|
|---|---|---|---|
|1|GET|/posts|레시피 리스트 조회|
|2|GET|/posts/{int:post_id}|레시피 상세 조회|
|3|DELETE|/posts/{int:post_id}|레시피 삭제|
|4||/posts/forms|레시피 신규 작성 폼|
|5|POST|/posts/forms|레시피 신규 등록|
|6|GET|/posts/forms/{int:post_id}|레시피 수정 폼|
|7|PUT|/posts/forms/{int:post_id}|레시피 수정 등록|
|8|POST|/posts/{int:post_id}/likes|레시피 좋아요|
|9|POST|/posts/{int:post_id}/comments|댓글 작성|
|10|POST|/posts/{int:post_id}/comments/likes|댓글 좋아요|
|11|PUT|/posts/{int:post_id}/comments/{int:comment_id}|댓글 수정|
|12|DELETE|/posts/{int:post_id}/comments/{int:comment_id}|댓글 삭제|
|13||/member/forms|신규 회원 가입 폼|
|14|POST|/member/forms|신규 회원 가입|
|15|POST|/login|로그인|
|16|POST|/logout|로그아웃|

# 상세 명세
## `GET` 1. /posts
### Request
#### Request Parameters

|Parameter|Value|Description|
|---|---|---|
|pageNo|1|조회하려는 페이지 번호를 전달|
|pageSize|10|조회하려는 페이지에서 데이터의 개수|

#### Request Message

```json
// header
{
	"Content-Type": "application/json"
}
```

### Response
#### Response Message

```json
// body
{
	"posts": [
		{
			"id": 14,
			"title": "레시피 제목",
			"create_date": "작성일"
		},
		{
			"id": 13,
			"title": "레시피 제목",
			"create_date": "작성일"
		},
		{
			"id": 12,
			"title": "레시피 제목",
			"create_date": "작성일"
		},
		...
		{
			"id": 5,
			"title": "레시피 제목",
			"create_date": "작성일"
		}
	],
	"pageNo": 1,
	"pageSize": 10
}
```

## 2. `GET` /posts/{int:post_id}
### Request
#### Request Message

```json
// header
{
	"Content-Type": "application/json"
}
```

### Response
#### Response Message

```json
// body
{
	"post": {
		"id": 13,
		"title": "string, 레시피 제목",
		"content": "text, 레시피 내용",
		"user_id": "string, 레시피 작성자",
		"create_date": "string, 레시피 작성일",
		"price": 13400,
		"like": 5
	},
	"comment": [
		{
			"comment_id": 16,
			"user_id": "string, 댓글 작성자",
			"content": "text, 댓글 내용",
			"create_date": "string, 댓글 작성일",
			"like": 3
		},
		{
			"comment_id": 15,
			"user_id": "string, 댓글 작성자",
			"content": "text, 댓글 내용",
			"create_date": "string, 댓글 작성일",
			"like": 0
		},
		{
			"comment_id": 14,
			"user_id": "string, 댓글 작성자",
			"content": "text, 댓글 내용",
			"create_date": "string, 댓글 작성일",
			"like": 1
		},
		...
	]
}
```

## 5. `POST` /posts/forms
### Request

```json
{
	"post": {
		"title": "string, 레시피 제목",
		"content": "text, 레시피 내용",
	},
	"food": [
		{
			
		}
	]
}
```