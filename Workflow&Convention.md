# 협업 Workflow

1. 원본 리포지토리를 각자의 Github로 fork

2. git clone [fork 떠온 리포지토리 주소]

3. git remote add upstream [원본 원격 리포지토리 주소]

4. git checkout -b [Branch 이름]
- 한끼얼마 Branch Naming 컨벤션
	- feature-기능_이름: 기능을 개발할 때 사용
	- fix-기능_이름: 버그를 수정할 때 사용
	- docs-문서_이름: 문서를 수정할 때 사용
	- refactor-기능 이름: 기능을 리팩토링할 때 사용

----- 기능 구현 및 개발 -----

5. git add .

6. git commit -m "메세지 내용"
- 한끼얼마 Commit 메시지 컨벤션
	- 한 줄로 표현
	- Header
		- feat: 새로운 기능에 대한 커밋
		- fix: 버그 수정에 대한 커밋
		- docs: 문서 수정에 대한 커밋
		- refactor: 리팩토링에 대한 커밋

```bash
git commit -m "fix: 로그인 토큰 오류 슈정"
```

7. git push origin [현재 Branch 이름]

8. Pull Request
- 한끼얼마 Pull Request Message 컨벤션
	- PR의 목적을 한 문장으로 표현
	- 피드백 받기를 원하는 지점을 명시

----- Code Review -----

9. git checkout main

10. git fetch upstream

11. git merge upstream/main

12. git push origin main

----- 한 사이클 끝, 새로운 기능 개발 시 4번부터 -----

# Conventions

- VSCode Extensions
	- Prettier
		- 코딩 스타일에 맞게 코드를 변환
	- ESLint
		- 문법 에러와 코드 품질 검사
- Airbnb Style Guide

# Tools

- IDE
	- FE: VSCode
	- BE: PyCharm
- Library: Recoil, styled
- Test Tools: Postman
