# Amazon Personalize

![person](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/person1.png)

- Amazon Personalize는 실시간 맞춤화 추천으로 애플리케이션을 구축
- 추천하는 항목
	- 맞춤화된 제품 추천
	- 재순위화(re-ranking)
	- 맞춤화된 직접 마케팅
	- 사용자가 원예 도구를 많이 구매했다면 Personalize 서비스를 기반으로 다음번 구매 제품을 추천
- Amazon.com에서 사용하는 기술과 같음
	- Amazon.com에서 쇼핑할 때 몇 가지 제품을 구입하면 Amazon.com에서 상품을 추천하기 시작
	- 동일하거나 전혀 다른 범주의 상품이 될 수 있는데, 검색한 내용이나 구매 내역 및 사용자 관심 등을 기반으로 나오는 결과
- AWS에서 Personalize로 동일하게 할 수 있음
	- Amazon S3으로부터 입력 데이터를 읽음
		- 사용자 상호 작용 등의 데이터
	- Amazon Personalize API를 사용하여 Amazon Personalize 서비스에 실시간 데이터를 통합할 수 있음
	- 그리고 웹사이트, 애플리케이션 및 모바일 앱 맞춤형 API를 활용
	- 맞춤화를 위해 SMS나 이메일을 보낼 수도 있음
	- 이런 모든 걸 통합할 수 있음
		- 몇 달이 아닌 며칠이면 이런 모델을 구축할 수 있음
		- ML 솔루션을 구축, 훈련 및 배포할 필요가 없음
		- 제공되는 번들 그대로 사용하면 됨
- 사용 사례는 소매 상점, 미디어 그리고 엔터테인먼트 등
- 시험을 대비해서는 추천 및 맞춤화된 추천을 위한 머신 러닝 서비스가 나올 때마다 Amazon Personalize를 생각