# Solutions Architectures: WhatIsTheTime.com

## Stateless Web App: WhatIsTheTime.com

- WhatIsTheTime.com은 사람들에게 시간을 알려줌
- 데이터베이스가 필요 없음
- 각각의 인스턴스와 서버는 시간이 몇 시인지 알고 있음
- 다운타임을 수용할 수 있지만, 사람들은 전 세계에 걸쳐 시간을 알고 싶어하기 때문에 다운타임을 제거할 수 있도록 수직 및 수평적으로 확장할 필요가 있음

## Starting Simple

![wtt](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/wtt1.png)

- PoC (Proof of Concept)
	- t2.micro 인스턴스와 사용자가 있음
	- 사용자가 시간을 물어봄
	- 오후 5시 30분이라고 대답
	- 공용 EC2 인스턴스가 있고 무슨 일이 생기면 재시작할 수 있도록 EC2 인스턴스가 고정 IP 주소를 갖도록 하고 싶음
		- 그렇기에 탄력적 IP 주소를 연결

## Scaling Vertically

![wtt](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/wtt2.png)

- 사용자들이 우리 애플리케이션에 접근할 수 있고 반응도 매우 좋음
- 애플리케이션이 점점 더 많은 트래픽을 갖게 되면서 t2.micro 인스턴스로는 충분하지 않아짐
- 부하를 처리하기 위해 t2.micro 인스턴스를 조금 더 큰 것으로 교체
	- 수직 확장
- 따라서 m5.large 유형의 인스턴스로 교체
	- 인스턴스를 중지시키고, 인스턴스 유형을 바꾸고, 그 후에 다시 인스턴스를 시작
- 탄력적 IP를 가지고 있기 때문에 동일한 공용 IP를 가지게 되고 사람들은 여전히 애플리케이션에 접근할 수 있음
- 그러나 M5로 업그레이드 하는 동안 다운타임이 발생했고 사용자들은 이를 그리 좋아하지 않음
	- 그동안에 애플리케이션에 접근할 수 없었기 때문
- 어쨌든 해결은 됐지만 아주 잘 된 것은 아님

## Scaling Horizontally

![wtt](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/wtt3.png)

- 다음으로, 정말로 큰 인기를 얻어 수평 확장을 해야 할 때가 됨
- 이 M5 애플리케이션은 하나의 공용 IP를 가지고 있고 탄력적 IP가 연결되어 있음
- 이제 정말 많은 사용자들이 와서 수평 확장을 하려고 함
- 먼저 EC2 인스턴스들을 추가
	- 모두 m5.large
	- 이것들 모두 탄력적 IP가 연결되어 있음
	- 즉 세 개의 EC2 인스턴스가 있고 세 개의 탄력적 IP가 있음
	- 따라서 우리 사용자들이 인스턴스들과 통신하려면 이 세 개의 탄력적 IP의 정확한 값을 알고 있어야 함
- 이것이 수평 확장
- 나름 잘 하고 있지만 이제 한계가 느껴짐
	- 사용자들은 점점 더 많은 IP를 알아야 함
	- 더 많은 인프라를 관리해야 함

![wtt](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/wtt4.png)

- 접근 방식을 바꿔야 함
- M5 유형의 EC2 인스턴스 세 개
- 탄력적 IP를 제거
	- 관리하기 너무 어렵기 때문
	- 한 계정에서 리전마다 겨우 다섯 개의 탄력적 IP만을 가질 수 있음
- 그 대신 사용자들은 Route 53를 활용하게 될 것임
- 이를 위해 Route 53를 설정했고 웹사이트 URL은 api.whatisthetime.com
- TTL이 한 시간인 A 레코드로 설정
	- A 레코드로 정했다는 건 이와 같이 DNS로부터 IP 리스트를 받는다는 의미
	- Route 53의 A 레코드는 IP
- 사용자들이 Route 53을 쿼리
- 그러면 EC2 인스턴스들의 IP 주소들을 얻게 되고, 이는 시간에 따라 변하며 Route 53이 업데이트될 것이니 전혀 문제가 되지 않음
	- 업데이트를 하고 동기화를 유지할 것임
- 이제 사용자들은 EC2 인스턴스에 접근할 수 있고 관리할 탄력적 IP도 더 이상 존재하지 않음
- Route 53을 사용하여 상당한 개선을 이룸

## Scaling Horizontally, Adding and Removing Instances

![wtt](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/wtt5.png)

- 이번에는 상황에 따라 즉시 인스턴스를 추가하고 제거할 수 있도록 확장하고 싶어짐
- 인스턴스를 제거하면 가장 위쪽 사용자들이 이 m5.large 인스턴스와 통신 중이었는데, 그것이 없어짐
- 그리고 TTL이 한 시간이었기 때문에 Route 53 쿼리를 시도하면 동일한 응답을 한 시간 동안 사용하게 됨
	- 따라서 사용자들이 한 시간 동안 그 인스턴스에 접속하려 하겠지만 그 인스턴스는 더 이상 존재하지 않음
	- 이건 그리 바람직하지 않음
- 사용자들이 아마 한 시간 후에는 이 두 인스턴스에는 접속할 수 있어 다시 즐길 수 있겠지만 지금 당장은 만족할 수 없기 때문
	- 그들은 애플리케이션이 다운됐다고 생각할 것

## Scaling Horizontally with a Load Balancer

![wtt](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/wtt6.png)

- 로드 밸런서 추가
- 이제 공용 인스턴스는 더 이상 없음
	- 대신 사설 EC2 인스턴스들이 있음
	- 이들을 같은 가용 영역에서 실행할 것임
- 로드 밸런서에는 상태 확인 기능도 있음
	- 이는 한 인스턴스가 다운되거나 작동하지 않으면 사용자로부터 해당 인스턴스로 트래픽을 전송하지 않음
- 이렇게 둘을 연결하면 ELB는 공개되겠지만 사설 EC2 인스턴스들은 뒤에 숨어 있음
- 그리고 보안 그룹 규칙을 사용하여 이 둘 사이의 트래픽을 제한
- WhatIsTheTime.com에 대해 사용자들이 쿼리하지만 로드 밸런서가 IP 주소를 지속적으로 바꾸기 때문에 이번에는 A 레코드가 될 수 없음
	- 대신, 로드 밸런서이기 때문에 별칭 레코드를 사용할 수 있음
- 별칭 레코드는 Route 53으로부터 ELB를 가리킬 것이고 모든 것이 매우 잘 작동할 것임
- 여기서 DNS를 바꾸지만 사용자들은 이제 로드 밸런서에 접속
	- 그리고 로드 밸런서는 EC2 인스턴스로 리디렉션하고 트래픽의 균형을 잡음
- 이제 로드 밸런서로 이 인스턴스들을 추가 및 제거하고 정렬할 수 있음
- 또한 상태 확인 기능 덕분에 사용자들에게 다운타임도 없을 것임
- 그러나 수동으로 인스턴스를 추가하고 제거하는 것은 꽤 귀찮음

## Scaling Horizontally with an Auto Scaling Group

![wtt](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/wtt7.png)

- 오토 스케일링 그룹을 실행
- 이제 왼쪽에는 API, 즉 Route 53 ELB가 있고 오른쪽에는 가용 영역이 있음
- 그리고 사설 EC2 인스턴스를 실행
- 그러나 이번에는 오토 스케일링 그룹이 인스턴스들을 관리
	- 즉 기본적으로 오토 스케일링 그룹이 요청에 따라 확장하도록 하는 것
- 이제 요청에 따른 확장이 가능
	- 확장과 축소
- 애플리케이션에 다운타임은 없고 오토 스케일링과 로드 밸런싱이 있음
- 그러나 이번에는 지진이 발생
	- 가용 영역 1번이 다운
	- 애플리케이션도 완전히 다운

## Making App Multi-AZ

![wtt](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/wtt8.png)

- 이번에는 ELB가 필요
- 상태 확인 뿐만 아니라 다중 AZ도 추가됨
- AZ 1부터 3에서 실행될 것이며 이 ELB는 세 개의 AZ가 있음
- 오토 스케일링 그룹 역시 다중 AZ에 걸쳐 있게됨
- 예를 들어 AZ 1에 두 개의 인스턴스가 있고 AZ 2에 두 개의 인스턴스가 있고 AZ 3에는 하나가 있다고 가정
	- 이 경우 AZ 1이 다운되더라도 AZ 2와 AZ 3이 있기 때문에 사용자를 위한 트래픽을 처리할 수 있다는 장점이 있음
- 이제 앱을 다중 AZ로 만들었고 높은 가용성을 확보했으며 장애 발생에도 대비

## Minimum 2 AZ => Reserve Capacity

![wtt](https://github.com/seungwonbased/TIL/blob/main/AWS/assets/wtt9.png)

- 두 개의 AZ가 있음
	- 그리고 각각의 AZ에는 최소 하나 이상의 인스턴스가 실행 중임
- 용량을 예약해 기본적으로 애플리케이션의 비용을 줄이는 작업을 시작
	- 1년 내내 두 개의 인스턴스가 항상 실행 중일 것이기 때문
	- 즉 오토 스케일링 그룹의 용량을 최소화하기 위해 인스턴스를 예약함으로써 미래에 상당한 비용을 절감할 수 있을 것임
	- 새로운 인스턴스가 실행되더라도 일시적일 것이고 요청에 따른 것은 괜찮음
- 좀 더 극단적으로는 비용 절감을 위해 스팟 인스턴스를 사용할 수도 있음
	- 그러나 이는 인스턴스들을 종료시키게 될 수도 있음
