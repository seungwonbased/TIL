# Amazon Kendra

![kendra](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/kendra1.png)

- Kendra는 머신 러닝으로 제공되는 완전 관리형 문서 검색 서비스
- 문서 내에서 답변을 추출할 수 있게 도와줌
	- 이때 문서는 text, pdf, HTML PowerPoint, MS Word, FAQ 등
- 이런 문서가 존재하는 다양한 데이터 소스가 있고, Amazon Kendra는 이런 문서를 인덱싱하여 머신 러닝으로 작동되는 지식 인덱스를 내부적으로 구축
- 최종 사용자 관점에서 바로 자연어 검색 능력에 도움
	- 예를 들어 사용자가 Amazon Kendra에 'IT의 지원 데스크 위치가 어디야?'라고 물으면 Kendra는 '1층입니다'라고 대답할 수 있음
	- 이게 가능한 이유는 Kendra가 모든 리소스를 검색하여 IT 지원 데스크의 위치가 1층임을 알 수 있기 때문
- 물론 일반적인 검색도 가능
- 사용자의 상호 작용 및 피드백에서 학습하고 선호되는 검색 결과를 내놓는 증분식 학습
- 검색 결과를 조정할 수도 있음
	- 데이터의 중요성 및 새로움, 사용자 정의 필터를 기반으로 조정 가능
- 시험 문제에서 문서 검색 서비스를 보게 되면 Amazon Kendra를 생각