# Solutions Architecture: MyClothes.com
## Stateful Web App: MyClothes.com

-  상태 유지 웹 애플리케이션인 MyClothes.com
- MyClothes.com은 사람들이 온라인으로 옷을 살 수 있게 해주고 장바구니가 있음
- 동시에 수백 명의 사용자가 있고 이 모든 사용자들이 웹사이트를 둘러봄
- 확장을 할 수 있어야 하고 수평 확장성을 유지하며 애플리케이션의 웹 티어를 최대한 무상태로 유지하고 싶음
- 비록 장바구니의 상태가 존재하지만 웹 애플리케이션을 최대한 쉽게 확장할 수 있어야 함
	- 이는 사용자들이 웹사이트를 둘러볼 때 장바구니를 잃어버리면 안된다는 뜻
- 또한 주소 등의 사용자 정보를 효과적으로 보관하고 어디에서나 접근 할 수 있는 데이터베이스에 저장할 것임

![mcl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/mcl1.png)

- 사용자가 있고 Route 53과 다중 AZ ELB가 있으며 오토 스케일링 그룹과 세 개의 AZ가 기본적으로 있음
- 애플리케이션이 ELB에 접근하고 ELB는 '이 인스턴스와 대화하세요' 라고 함
- 장바구니를 생성하고 다음 요청은 같은 인스턴스가 아니라 다른 인스턴스로 감
	- 그러면 장바구니가 사라짐
- 사용자는 '아마도 작은 버그가 있는 것 같군 다시 해봐야지'라고 생각
	- 그래서 장바구니에 뭔가를 넣은 다음 이번에는 세 번째 인스턴스로 리디렉션되는데 또 장바구니가 사라짐

## Stickiness (Session Affinity)

![mcl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/mcl2.png)

- 고착도, 즉 세션 밀접성을 도입할 수 있음
	- 이는 ELB의 기능
- ELB Stickiness를 활성화
	- 이제 사용자가 첫 번째 인스턴스에 접속하여 뭔가를 장바구니에 추가
	- 그리고 고착도 덕분에 두 번째 요청도 동일한 인스턴스로 가게됨
	- 세 번째 요청도 마찬가지로 같은 인스턴스로 감
	- 사실 모든 요청이 고착도 덕분에 동일한 인스턴스로 가게 될 것임
- 아주 잘 작동하지만 만약 EC2 인스턴스가 어떤 이유로든 종료되면 장바구니를 잃어버리게 됨
- 어쨌든 고착도과 세션 밀접성 덕분에 조금은 개선된 것은 사실

## User Cookies

![mcl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/mcl3.png)

- 이제 완전히 다른 접근 방법인 사용자 쿠키 도입
- 기본적으로 EC2 인스턴스가 장바구니 내용을 저장하는 대신 사용자쪽에서 장바구니 내용을 저장하도록 하는 것
	- 로드 밸런서에 접속할 때마다 '내 장바구니에는 이런 것들이 있어'라고 말하게 하는 것
- 이는 웹 쿠키를 통해 이루어짐
- 첫 번째 서버에 접속하거나 두 번째 또는 세 번째 서버에 접속해도 사용자가 직접 EC2 인스턴스로 장바구니 내용을 보내주기 때문에 각각의 서버가 장바구니의 내용을 알 수 있음
- 이제 각각의 EC2 인스턴스가 이전에 있었던 일을 알 필요가 없는 무상태를 달성함
	- 이전에 있었던 일은 사용자가 말해줄 것임
- 그러나 HTTP 요청이 점점 더 무거워지는 문제가 있음
	- 왜냐하면 웹 쿠키를 통해 장바구니 내용을 보낼 때 장바구니에 뭔가를 추가할수록 점점 더 많은 데이터를 보내기 때문
- 또한 쿠키가 공격자에 의해 변경됨으로써 사용자의 장바구니가 갑자기 수정될 수 있기 때문에 어느 정도의 보안 위험도 존재
- 따라서 이런 종류의 아키텍처에서는 EC2 인스턴스가 반드시 사용자 쿠키의 내용을 검증해야 함
- 또한 전체 쿠키의 크기는 4KB 이하까지만 가능해 쿠키 내에는 매우 작은 정보만 저장할 수 있음
	- 대량의 데이터 셋을 저장할 수는 없음
- 이것은 많은 웹 애플리케이션 프레임워크에서 실제로 사용하는 패턴

## Server Session

![mcl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/mcl4.png)

- 서버 세션 개념을 도입
- 전체 장바구니를 웹 쿠키로 보내는 대신에 단순히 세션 ID만 보내는 것
	- 이것은 사용자에 대한 세션 ID
- 백그라운드에는 ElastiCache 클러스터가 존재
	- 세션 ID를 보낼 때 EC2 인스턴스에게 '이 물건을 장바구니에 추가할 거야'라고 말함
	- 그러면 EC2 인스턴스는 장바구니 내용을 ElastiCache에 추가하고 이 장바구니 내용을 불러올 수 있는 ID가 바로 세션 ID가 됨
	- 그래서 사용자가 세션 ID와 함께 두 번째 요청을 보내면 이는 다른 EC2 인스턴스로 가게 되고 그 EC2 인스턴스는 세션 ID를 사용하여 ElastiCache로부터 장바구니 내용을 찾아서 세션 데이터를 불러올 수 있음
- 마지막 요청에 대해서도 동일한 패턴
- ElastiCache의 또 다른 장점은 1천분의 1초 이하의 성능을 가졌다는 점
	- 따라서 이 모든 것은 매우 빠르게 진행됨
- 참고로 세션 데이터 저장의 또 다른 방식으로 DynamoDB가 있음
- ElastiCache가 정보의 출처이고 공격자들은 ElastiCache의 내부를 수정할 수 없기 때문에 훨씬 안전해짐
- 훨씬 더 안전한 패턴이고 실제로 많이 사용됨

## Storing User Data in a Database

![mcl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/mcl5.png)

- 사용자 데이터를 데이터베이스에 저장하려고 함
	- 사용자 주소 등을 저장하고 싶음
- 따라서 다시 한번 EC2 인스턴스와 통신을 할 텐데 이번에는 RDS 인스턴스와 통신할 것
	- RDS는 장기적인 저장을 위한 것이라 좋음
- RDS와 직접 통신함으로써 주소, 이름 등의 사용자 데이터를 저장하거나 불러올 수 있음
- 그리고 각각의 인스턴스가 RDS와 통신할 수 있으며 일종의 다중 AZ 무상태 솔루션을 효과적으로 얻을 수 있음

## Scaling Reads

![mcl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/mcl6.png)

- 트래픽이 늘어나고 웹사이트가 잘 운영됨
	- 사용자도 계속 늘어남
- 우리는 사용자들이 대부분의 시간을 웹사이트를 둘러보며 보낸다는 걸 알게됨
	- 읽어서 제품 정보를 얻는 등의 일
- RDS를 사용할 때
	- 쓰기를 수행하는 RDS 마스터를 사용할 수 있음
	- 복제가 일어나는 RDS 읽기 전용 복제본을 사용할 수도 있음
- RDS에서는 다섯 개의 읽기 전용 복제본을 가질 수 있음
	- 이는 RDS 데이터베이스의 읽기를 확장할 수 있도록 해줌

## Scaling Reads (Alternative) - Lazy Loading

![mcl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/mcl7.png)

- 또 다른 패턴으로는 캐시를 사용하는 쓰기 모드도 있음
- 사용자가 EC2 인스턴스와 통신하는 방식으로 작동
	- 캐시를 살펴보고 '이런 정보를 가지고 있나요?'라고 물어봄
	- 가지고 있지 않다면 RDS로부터 읽어 들여서 ElastiCache에 집어넣음
		- 즉 이 정보가 캐싱 되는 것
- 다른 EC2 인스턴스들도 같은 방식으로 작동합니다
	- 단 이번에는 ElastiCache와 통신할 때 정보를 얻게 되고 캐시 히트
	- 그러면 캐싱이 됐기 때문에 즉시 응답을 받음
	- 이 패턴을 통해 RDS상의 트래픽을 줄일 수 있음
	- 기본적으로 RDS상의 CPU 사용을 줄이고 동시에 성능을 향상시키는 것
- 그러나 이제는 캐시 유지 보수가 필요
	- 이는 꽤 어려운 일이고 애플리케이션 쪽에서 이루어져야 함

## Multi-AZ - Survive Disaster

![mcl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/mcl8.png)

- 이제 재해에 대비할 차례
- 사용자가 Route 53과 통신을 하는데 이제 우리는 다중 AZ ELB가 있음
	- 그런데 Route 53은 이미 가용성이 높음
	- 뭔가를 더 할 필요가 없음
- 로드 밸런서는 다중 AZ로 만들 것임
	- 오토 스케일링 그룹도 다중 AZ
- 다음으로 RDS 역시 다중 AZ 기능이 있음
- 또 다른 방법으로는 재해가 발생할 경우 인계받을 수 있는 대기 복제본이 있음
- 레디스를 사용한다면 ElastiCache도 다중 AZ 기능을 가지고 있음

## Security Groups

![mcl](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/mcl9.png)

- 보안 그룹에 대해서는 매우 안전해야 함
- ELB 쪽 어디에서나 HTTP HTTPS 트래픽을 열 수 있음
- EC2 인스턴스 측면에서는 로드 밸런서로부터 오는 트래픽만 제한
- ElastiCache 측면에서는 EC2 보안 그룹으로부터 오는 트래픽만 제한
- RDS는 EC2 보안 그룹으로부터 오는 트래픽을 제한

## 3-Tier Architecture for Web Application  설계 완료

- ELB 고정 세션
- 쿠키 저장을 위한 웹 클라이언트 웹 앱을 무상태로 만드는 법
- ElastiCache를 위한 세션 ID와 세션 캐시의 사용
	- 그리고 대안으로서의 사용할 수 있는 DynamoDB
- 읽기의 경우 RDS로부터 데이터 캐시를 위해 ElastiCache를 사용할 수 있음
- 또 재해에 대비하기 위해 다중 AZ를 사용할 수 있음
- 사용자 데이터를 저장하기 위해 더 오래가는 데이터 형식인 RDS를 사용할 수 있음
- 읽기 전용 복제본은 읽기 확장에 사용하고, ElastiCache를 사용할 수도 있음
- 재해 복구를 위한 다중 AZ가 있음
- 서로 참조하는 보안 그룹을 위해 철저한 보안을 추가함
- 클라이언트 티어, 웹 티어, 그리고 데이터베이스 티어 이렇게 세 개의 티어
	- 이것은 매우 보편적인 아키텍처