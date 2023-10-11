# Image Tagging

- 태그를 명시하지 않으면 자동으로 latest 태그로 이미지가 생성됨
- Dockerfile 내용 변경 없이 이미지 빌드 ⇒ 캐시를 사용하므로 새로 이미지를 만들지 않음
- --no-cache 옵션과 함께 이미지 빌드 ⇒ 캐시를 사용하지 않고 이미지를 새로 생성
	- 동일한 이미지 이름을 사용하므로 이전 이미지의 이름이 삭제됨
		- dangling 이미지 생성됨
- --filter 옵션 사용하여 dangling 상태의 이미지를 조회
- dangling 이미지 생성을 방지하기 위해서는 이미지 생성 전에 tag 명령으로 이미지 태그를 변경한 후 이미지를 생성하는 것이 필요