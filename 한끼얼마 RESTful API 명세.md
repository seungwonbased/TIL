
|Index|Method|URI|Description|
|---|---|---|---|
|1|GET|/posts|레시피 리스트 조회|
|2|GET|/posts/{int:post_id}|레시피 상세 조회|
|3|DELETE|/posts/{int:post_id}|레시피 삭제|
|4|GET|/posts/forms|레시피 신규 작성 폼|
|5|POST|/posts/forms|레시피 신규 등록|
|6|GET|/posts/forms/{int:post_id}|레시피 수정 폼|
|7|PUT|/posts/forms/{int:post_id}|레시피 수정 등록|
|8|POST|/posts/{int:post_id}/likes|레시피 좋아요|
|9|POST|/posts/{int:post_id}/comments|댓글 작성|
|10|POST|/posts/{int:post_id}/comments/likes|댓글 좋아요|
|11|PUT|/posts/{int:post_id}/comments/{int:comment_id}|댓글 수정|
|12|DELETE|/posts/{int:post_id}/comments/{int:comment_id}|댓글 삭제|
|13|GET|/member/forms|신규 회원 가입 폼|
|14|POST|/member/forms|신규 회원 가입|
|15|POST|/login|로그인|
|16|POST|/logout|로그아웃|


