# Amazon Transcribe

- 이름에서 알 수 있듯이, 자동으로 음성을 텍스트로 변환시켜 줌
	- 즉, 오디오를 넣으면 자동으로 텍스트로 변환됨
- 작동 원리
	- 자동 음성 인식(ASR)이라는 딥러닝 프로세스를 사용하여 음성을 텍스트로 매우 빠르고 정확하게 변환
- 몇 가지 기능
	- 알아둬야 함
	- 첫 번째는 Redaction을 사용하여 개인 식별 정보(PII)를 자동으로 제거할 수 있다는 것
		- 즉, 예를 들어 누군가의 나이, 이름, 사회보장번호가 있다면 자동으로 제거됨
	- 두 번째로는 다국어 오디오를 자동으로 언어 식별할 수 있음
		- 프랑스어, 영어, 스페인어가 섞여 있어도 Transcribe는 똑똑하게 모두 인식
- 사용 사례
	- AS 전화의 대본을 자동으로 폐쇄 자막이나 자막으로 만드는 것
	- 완전히 검색 가능한 아카이브를 만들기 위해 미디어 자산에 대한 메타데이터를 만듬
